Title: How I run my own DNS servers
Date:  2012-12-31 12:15:00
Id:    f699a
Tags:  DNS, Devops, Email
Show_upsell: true

*This article was featured in [Hacker Monthly Issue 35](http://hackermonthly.com/issue-35.html).*

For the longest time I used [zoneedit][] as my DNS provider of choice. All of my important domains were hosted there, they never really did me wrong. A few months back I decided that I wanted to learn how DNS actually works in the real world, though. Like, what does it actually take to run my own DNS servers?

[zoneedit]: http://www.zoneedit.com/

--fold--

*Important note: I'm no longer running my own DNS servers. I [wrote a post about why](/how-and-why-im-not-running-my-own-dns).*

### Step 0: Why would you ever do that?!

I'm mostly motiviated by curiosity, but also by frustration. When something isn't going my way it just starts to make sense to do it myself. My frustration with zoneedit wasn't anything super specific. Their dynamic DNS system wasn't too terribly dynamic and adding and editing zones through their web interface got to be pretty tedious after awhile. I have a bunch of zones (32 at last count), most of which are very simple setups. `bugsplat.info` is way more complicated, but we'll get into that later.

### Step 1: The Hardware

I decided that if I'm going to do this, I'm going to go all out. To that end, I rented two VPSs, one from [RamNode][] (notice: affiliate link) in Atlanta and another from [Prgmr][] in San Jose. Overall I would say that my RamNode experience has been more positive than my Prgmr experience. The network links have gone down twice in the past six months at Prgmr, which isn't the end of the world when you're running a redundant service but it's still pretty annoying. Ramnode has had 100% uptime so far.

Specs on these bad boys:

* prgmr (teroknor.bugsplat.info): 1 core, 1024MiB ram, 24GiB Disk, 160GiB transfer
* ramnode (empoknor.bugsplat.info): 4 core, 2048MiB ram, 30GiB SSD-backed Disk, 4000GiB transfer

I'm not even close to exploiting these two machines. I'm planning on moving more and more of my apps and sites over to them, but right now they're mainly handling this site and my email and DNS.

Why two machines? To host your own DNS servers the registrars require you to list two IP addresses with the idea that you'll be providing redundant service. The one thing you don't want is downtime with DNS, it screws everything up.

### Step 2: The Software

Once you decide to down this DNS rabbit hole there are a bunch of decisions to make on the software side. I considered PowerDNS and BIND and finally settled on tinydns managed via puppet and supply drop. [Tinydns][] is a project started by Daniel J. Bernstein many years ago and has proven to be extremely reliable when run as intended (no axfr, configuration propogation via scp, etc). My setup is thus:

* [Puppet][] managing the config for both boxes
* [Supply drop][supply-drop] deploys this configuration via [Capistrano]
* Tinydns has a static config file checked into git controlling most of my zones
* Tinydns also has a dynamic file that does my dynamic DNS updates for the home router

`bugsplat.info` is my oldest and thus most complicated domain. It's not even that complicated, really, it just handles a lot of stuff. My Mac mini runs a cron job every minute that ssh's into both machines and rebuilds the tinydns config file if it's IP has changed. That IP is then assigned to `subspace.bugsplat.info` and I have a wildcard CNAME for `*.bugsplat.info` pointing at `subspace`. This lets me do things like various services running on that mac mini with distinct hostnames, all hiding behind a common nginx. In addition, each VPS has a wildcard CNAME pointing to it from `*.<hostname>.bugsplat.info` which lets me set up new apps and sites easily. 

### Step 3: The Email

One of the other problems I had with zoneedit was their free email forwarding setup. It was slow. So slow. Slower than molasses spread onto the back of the slowest dog. Even before this whole DNS adventure started I knew I wanted to get rid of that.

Each VPS runs it's a copy of my [Postfix][] setup (also managed via puppet), which mostly just forwards incoming email into my gmail account. I don't send through it, since I haven't quite figured out all of the various DKIM and DMARC and SenderID and SPF things I need to do, and besides which Gmail won't send out through my SMTP server anyway. 

### Step 4: Logging

One of the more interesting aspects of this whole project has been getting a comprehensive view of everything that goes on in my little empire. The other day I set up global logging using [Papertrail][], a hosted logging service. It doesn't do a whole lot, mostly it just seeps up logs from all of my services including these two VPSs and a bunch of Heroku apps, makes them searchable for a few days, and drops tarballs of them onto S3 nightly. It's given me really valuable insight into at least two things: my gmail backup wasn't working, and I get hit a *lot* by Chinese and India SSH breakin attempts. Still working on how to deal with that one, but the gmail backup is up and running.

### Conclusion

So after all of that, what have I learned? Mostly that I'm a very particular person with regards to this stuff. It's fun right now but I can see it getting kind of tedious down the line. We'll find out! It's been an interesting ride thus far and I've learned quite a bit which is the most important thing.

[RamNode]: https://clientarea.ramnode.com/aff.php?aff=142
[Prgmr]: http://prgmr.com/xen/
[Puppet]: http://puppetlabs.com/
[supply-drop]: https://github.com/pitluga/supply_drop
[Capistrano]: https://github.com/capistrano/capistrano
[Tinydns]: http://tinydns.org/
[Postfix]: http://www.postfix.org/
[Papertrail]: http://www.papertrailapp.com/
