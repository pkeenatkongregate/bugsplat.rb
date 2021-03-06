Title: Mastering Modern Payments: Using Stripe with Rails by Pete Keen
Id: mmp
Layout: book_layout
View: book
Skip_title_suffix: true

<h1 class="book">Mastering Modern Payments <small>Using Stripe with Rails</small></h1>

<div class="question">Want to make sure your Stripe integration is right?</div>

My new guide <strong><em>Mastering Modern Payments: Using Stripe with Rails</em></strong> will teach you:

<div>
<img style="float: right; height: 200px; padding-top: 20px;" src="http://files.bugsplatcdn.com/files/3b437de8dd3500b8d649/book_stack-1.png" />

<ul>
<li>Why a simple 10-minute integration <strong>isn't enough</strong>
<li>Dealing with security including PCI-DSS
<li>How and why to use Stripe's <code>stripe.js</code> and <code>checkout.js</code>
<li>How to build custom payment forms
<li>How to set up Rails to keep an audit trail for you automatically
<li>How to handle subscription billing
<li>How to create marketplaces with Stripe Connect and Stripe Payouts
<li>How and why you should process payments using a background worker
<li>How to email your customers effectively
</ul>

<div class="well" style="margin-top: 2em; margin-bottom: 2em; text-align: center;">
<h3>Get a free chapter and 10% off</h3>
<form action="http://bugsplat.us6.list-manage.com/subscribe/post?u=4d4742d4ee66f8c62af747acb&amp;id=1920a1a25a" method="post" class="form form-big form-inline" target="_blank">
    <div class="input-append">
	<input type="email" class='text input-xlarge' value="" name="EMAIL" id="mce-EMAIL" placeholder="Email address">
	<input type="submit" value="Get Free Chapter" name="subscribe" id="mc-embedded-subscribe" class="btn btn-primary btn-large">
    </div>
</form>
<div><small>Register to get 10% off on launch day, August 15th 2013</small></div>
</div>

<p>
Thousands of small businesses are using Stripe to process recurring or one-time payments for their Ruby on Rails SaaS offerings. Most of them, probably including yours, are using a simple integration copied from a "Stripe in 10 minutes" tutorial, but what these tutorials don't cover could cost you customer good will and  <strong>thousands of dollars</strong> in lost payments and lost opportunities.</p>

* Expired credit cards
* Lost transactions due to timeouts and bugs
* Lost time digging through Stripe's dashboard or the Rails console to see your quarterly revenue or to find a specific customer's transaction

<div style="margin-top: 2em; margin-bottom: 2em">
<img style="float: right; margin-left: 20px; padding-top: 100px;" src="http://files.bugsplatcdn.com/files/a8dab64c9e6402ee7b16/stripe_rails.png">
<h2>Table of Contents</h2>
<ol>
<li>Introduction
<li>Initial Example Application
<li>The Simplest Stripe Integration
<li>Security and PCI Compliance
<li>Custom Payment Forms
<li>State and History
<li>Handling Webhooks
<li>Processing Payments with Background Workers
<li>Subscriptions
<li>Marketplaces
<li>Additional Resources
</ol>
</div>

<div style="margin-top: 2em; margin-bottom: 2em">
<h2>About the Author</h2>
<p>
<img class="thumbnail" src="http://files.bugsplatcdn.com/files/54919f94183b56488a1e/me-small.png" style="float:left; margin-right: 20px; height:100px;">
I'm Pete Keen. I've been a professional software developer for seven years and live in Portland, Oregon. I write a blog at petekeen.net with articles on topics ranging from Ruby and Rails to Perl and robotics and scattered book reviews.
</p>
</div>

<div style="margin-top: 2em; margin-bottom: 2em">
<h2>What others are saying</h2>
<blockquote>
<img class="thumbnail pull-right" src="http://files.bugsplatcdn.com/files/2a34b4be575a85bdf517/matt_vanderpol.jpg" style="margin-left: 1em;">
Mastering Modern Payments is a fantastic resource for integrating Stripe billing with your Rails app. Pete does a great job of pulling together existing resources and blending them with his own experience to provide a clear, valuable guide for billing with Stripe.
<br><br>
Matt Vanderpol - founder QAtab
</blockquote>
</div>

<div class="well">
<h3>Guide + Code + Team License</h3>
<ul class="archive-list">
<li><span class="mmp-icon"><i class="icon-edit"></i></span> 112 page PDF with more than 100 code samples
<li><span class="mmp-icon"><i class="icon-download-alt"></i></span> ePub for Nook and Mobi for Kindle reading
<li><span class="mmp-icon"><i class="icon-html5"></i></span> Single-page HTML format for easy reference and searching
<li><span class="mmp-icon"><i class="icon-code"></i></span> The full source code for the Rails application I use to sell the guide
<li><span class="mmp-icon"><i class="icon-group"></i></span> License to share with up to 50 members of your team
</ul>
<span class="pull-right date">More below <i class="icon-arrow-down"></i></span>
<a class="btn btn-large btn-success disabled">Buy on August 15th for $299</a>
</div>

<div class="well highlight">
<h3>Guide + Code</h3>
<ul class="archive-list">
<li><span class="mmp-icon"><i class="icon-edit"></i></span> 112 page PDF with more than 100 code samples
<li><span class="mmp-icon"><i class="icon-download-alt"></i></span> ePub for Nook and Mobi for Kindle reading
<li><span class="mmp-icon"><i class="icon-html5"></i></span> Single-page HTML format for easy reference and searching
<li><span class="mmp-icon"><i class="icon-code"></i></span> The full source code for the Rails application I use to sell the guide
</ul>
<a class="btn btn-large btn-success disabled">Buy on August 15th for $59</a>
</div>

<div class="well">
<h3>Just the Guide</h3>
<ul class="archive-list">
<li><span class="mmp-icon"><i class="icon-edit"></i></span> 112 page PDF with more than 100 code samples
<li><span class="mmp-icon"><i class="icon-download-alt"></i></span> ePub for Nook and Mobi for Kindle reading
<li><span class="mmp-icon"><i class="icon-html5"></i></span> Single-page HTML format for easy reference and searching
</ul>
<a class="btn btn-large btn-success disabled">Buy on August 15th for $29</a>
</div>

## Questions

### Which package should I buy?

That depends on your budget. The guide by itself is still 100+ pages packed with code samples and tips that will get you on the right path, right away.

### How do I find out more about you?

I've been writing about software development and lots of other topics on my blog <a href="http://www.petekeen.net">petekeen.net</a> since 2009 and I tweet at <a href="https://twitter.com/zrail">@zrail</a>.</p>

### What if I hate the guide?

If you're not happy I'm not happy. Just reply to your receipt email within 30 days and you'll get a full refund

### I have another question!

Email me at <a href="mailto:pete@petekeen.net">pete@petekeen.net</a> and I'll do my best to help you out.

<div class="well" style="text-align: center" id="signup">
<p>Sign up and you'll get progress updates, preview chapters, and a <br><em><strong>10% off discount code</strong></em> when the guide is published <br>currently scheduled for August 15th 2013.</p>

<form action="http://bugsplat.us6.list-manage.com/subscribe/post?u=4d4742d4ee66f8c62af747acb&amp;id=1920a1a25a" method="post" class="form form-big form-inline" target="_blank">
    <div class="input-append">
	<input type="email" class='text input-xlarge' value="" name="EMAIL" id="mce-EMAIL" placeholder="Email address">
	<input type="submit" value="Get Free Chapter" name="subscribe" id="mc-embedded-subscribe" class="btn btn-primary btn-large">
    </div>
</form>
</div>

