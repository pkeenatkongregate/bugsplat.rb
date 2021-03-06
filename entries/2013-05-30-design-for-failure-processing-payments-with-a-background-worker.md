Title: Design for Failure: Processing Payments with a Background Worker
Id:    7dc8b
Tags:  Programming
Show_upsell: true

[stripe]: https://stripe.com/docs/tutorials/checkout
[guide]: /mastering-modern-payments
[docs]: https://stripe.com/docs/api
[sucker_punch]: https://github.com/brandonhilkert/sucker_punch
[Celluloid]: https://github.com/celluloid/celluloid/


Processing payments correctly is hard. This is one of the biggest lessons I've learned while writing my various [SaaS projects](/projects.html). Stripe does everything they can to make it easy, with [quick start guides][stripe] and [great documentation][docs]. One thing they really don't cover in the docs is what to do if your connection with their API fails for some reason. Processing payments inside a web request is asking for trouble, and the solution is to run them using a background job. 

--fold--

### The Problem

Let's take Stripe's example code:

```ruby
Stripe.api_key = ENV['STRIPE_API_KEY']

# Get the credit card details submitted by the form
token = params[:stripeToken]

# Create the charge on Stripe's servers - this will charge the user's card
begin
  charge = Stripe::Charge.create(
    :amount => 1000, # amount in cents, again
    :currency => "usd",
    :card => token,
    :description => "payinguser@example.com"
  )
rescue Stripe::CardError => e
  # The card has been declined
end
```
    
Pretty straight-forward. Using the `stripeToken` that `stripe.js` inserted into your form, create a charge object. If this fails due to a `CardError`, you can safely assume that the customer's card got declined. Behind the scenes, `Stripe::Charge` makes an `https` call to Stripe's API. Typically, this completes almost immediately.

But what if it doesn't? The internet between your server and Stripe's could be slow or down. DNS resolution could be failing. There's a million reasons why this code could take awhile. Browsers typically have around a one minute timeout and application servers like Unicorn usually will kill the request after 30 seconds. That's a long time to keep the user waiting just to end up at an error page.

### The Solution

The solution is to put the call to `Stripe::Charge.create` in a background job. This example is going to use a very simple background worker system named [Sucker Punch][sucker_punch]. It runs in the same process as your web request but uses [Celluloid][celluloid] to do things in a background thread.

First, let's create a job class:

```ruby
class StripeCharger
  include SuckerPunch::Worker

  def perform(event)
    ActiveRecord::Base.connection_pool.with_connection do
      token =  event[:token]
      txn = Transaction.find(event[:transaction_id])

      begin
        charge = Stripe::Charge.create(
          amount: txn.amount,
          currency: "usd",
          card: token,
          description: txn.email
        )
        txn.state = 'complete'
        txn.stripe_id = charge.id
        txn.save!
      rescue Stripe::Error => e
        txn.state = 'failed'
        txn.error = e.json_body
        txn.save!
      end
    end
  end
end
```

Again, pretty straightforward. Sucker Punch will create an instance of your job class and call `#perform` on it with a hash of values that you pass in to the queue, which we'll get to in a second. We look up a `Transaction` record, initiate the charge, and capture any errors that happen along the way.

`Transaction` in this case is a simple `ActiveRecord` object with just a few attributes, just enough to capture what Stripe gives us:

```ruby
class Transaction < ActiveRecord::Base
  attr_accessible :stripe_id, :state, :amount, :error, :email
end
```
    
Sucker Punch needs to know about our job class, so let's tell it in an initializer:

```ruby
SuckerPunch.config do
  queue name: :payments_queue, worker: StripeCharger, workers: 10
end
```

Now for the controller that ties it all together:

```ruby
class TransactionsController < ApplicationController

  def create
    txn = Transaction.new(
      amount: 1000,
      email: params[:email],
      state: 'pending'
    )
    if txn.save
      SuckerPunch::Queue[:payments_queue].async.perform(
        transaction_id: txn.id,
        token: params[:stripeToken]
      )
      render json: txn.to_json
    else
      render json: {error: txn.error_messages}, status: 422
    end
  end
  
  def show
    txn = Transaction.find(params[:id])
    raise ActionController::RoutingError.new('not found')
      unless txn

    render json: txn.to_json
  end
end
```

The `create` method creates a new `Transaction` record, setting it's state to `pending`. It then queues the transaction to be processed by `StripeCharger`. The `show` method simply looks up the transaction and spits back some JSON. On your customer-facing page you'd do something like this:

```javascript
function doPoll(id){
    $.get('/transactions/' + id, function(data) {
        if (data.state === "complete") {
          window.location = '/thankyou';
        } elsif (data.state === "failed") {
          handleFailure(data);
        } else {
          setTimeout(function(){ doPoll(id); }, 500);
        }
    });
}
```
    
Your page will poll `/transactions/<id>` until the transaction ends in either success or failure. You'd probably want to show a spinner or something to the user while this is happening.

With this setup, you've insulated yourself from problems in your connection to Stripe, your connection to your customer, and everything in between.

*This is an excerpt from my upcoming guide [Mastering Modern Payments: Using Stripe with Rails][guide].*
