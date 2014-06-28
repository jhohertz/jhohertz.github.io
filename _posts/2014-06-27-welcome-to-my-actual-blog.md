---
layout: post
category : general
tags : [welcome]
---
{% include setup %}

Welcome to my GitHub hosted blog! The one I will actually use, and post things on, and get twittered or whatever.

It's mostly going to be about various things typical of what you might find on someones GitHub account, some others, less so. As this is this first post, there is not much here as of yet.

And contrary to what you might think if you read this whole post, I actually want to do some things that are accessible to people getting into programming. Like this blog, as in the engine that drives it. It's actually really easy to use it to make a blog. And anyone who doesn't already know Git or other software configuration management will get to know the basic operations of it if they get to know how to blog with it. Sneaky. Anyway more on that below.

I have recently decided to embrace using GitHub and push out all the various silly projects I have, for whatever reason hesitated to publish before now, and am in the process of cleaning things up to post for anyone who finds them of use. I've been using GitHub, and other software configuration management systems for nearly 20 years, even built my own private work alike to GitHub using [Redmine](http://www.redmine.org/), which eventually I will publish the recipe for. But anything I want to put out to the public, I am going to push to here.

I didn't want to maintain my own infrastructure, or require much in the way of specific tooling to work with the blog. I found a way to do it using just git and and editor with GitHub Pages. (Or in ways that are both more, and less convenient, using just their website)

Read on if you care to learn more about how I host this blog, and other projects I working on.

<!--fold-->

There have been many attempts at a blog before, and I have to admit, that in the end, I put together [my own blogging thing](http://github.com/jhohertz/dr-henry) to make it seem worth the while. :) I really didn't want to use hosted blogging in the usual sense. I've at various times, played with the usual LAMP-based suspects, supported some in a working context, but they all seemed a chore for what simple things I'm looking for.

Then I found out about GitHub Pages. Not only can you push a static site for free, there is a way to generate pages which is actually which is really quite powerful. Various data is made available, all templates see all pages data, not just their own, and you can build a lot of crazy things with it, for being a static website in the end. It kind of gives you a middle ground. Versus the usual model-view-controller or other such thing, and in the context of that, it it like there is no controller, and every data object is handed to you.

You can even point your own custom DNS at your account to host on your own domain.

There are some obvious limits to such an approach. The first is that it can not alone do any kind of server-side processing. The second is there is a limit to how much "site" you can push to GitHub. GitHub also places some strict-but-fair rules on your generated site, relative to what you can do by using the same Jekyll tool they use, locally, and just pushing the output to GitHub Pages.

So that's [Dr. Henry](http://github.com/jhohertz/dr-henry).

The other main project, is Buri, which I get to work on at work and put back as open source. It's a toolkit geared specifically towards working with the components that Netflix has made open-source for bonkers-scale computing. They have made huge portions of their infrastructure components on GitHub, and others have made available integrations from both an application focus, such as Flux Capacitor, or from a deployment focus, such as netflixoss-ansible.

If you're new to these Netflix components, please checkout this set of introduction slides, and the ones it links to.

Buri works to address the various challenges of bootstrapping the stack into the cloud, as Netflix *might* run it. It also recently allows pushing an all-in-one environment into a single virtual machine. (And like 10 Java virtual machines inside that)

The all-in-one deploys all the various components that Flux Capacitor uses, that can run outside of the cloud. Besides setting up sudo and an ssh key before running Buri, and adding Cassandra schema after, you get an operational and integrated:

- Flux Capacitor edge and middle tier services.
- Cassandra running without Priam
- Eureka, with the edge and middle tier registering and discovering though it
- Exhibitor, with the attachment Flux Capacitor provides for Archaius Zookeeper support
- Turbine, aggregating the middle / edge instances
- Hystrix Dashboard, for visualizing real time system performance
- Graphite, for longer term data collection, provided by Servo, providing hundreds of data points even for the simple demonstration application Flux Capacitor provides.

Under the covers it is really just some Ansible config, with a few shell script helpers.

So that's the all-in-one environment. (Getting started guide to come!)

But this is all brand new, and nowhere close to where Buri started. And with that we turn to what it does for the cloud.

One of the Netflix components is something called AMInator, and the capitalization I have added tells you why. It is all about creating so-called AMI images used to boot up a computing environment in Amazon EC2. It does one thing, and does it well, and that it to find an existing AMI image, connect it's backing volume snapshot, do something to modify it, re-snapshot it, and create a new AMI. It's fairy generic, and there are plug-ins for all your favourite system provisioners like Chef, Puppet, and of course Ansible.

It's really about something called Immutable Deployment, which just means you never change anything on a deployed systems. You build new ones and switch them out, retiring the old. Building every image from scratch would take a while to get done. So what Aminator lets you do is build up your images in layers, a base, something even more fundamental than a base, and your specific images built atop those. (You could go as layered as you like really)

As Buri got started, we had two main problems with using Aminator directly (some of which may be addressed now)

- We had to use a different type of image for newer "HVM" machine types than Aminator assumed. There has been some work in this area in the current testing branch of Aminator.
- You need to provide your own foundation and base AMIs. Aminator doesn't help build these.

So the first thing you can do with Buri, is bootstrap the creation of an Ubuntu LTS (12.04 or 14.04) foundation AMI in your account. You can not access the snapshots of the official Ubuntu LTS AMI's to work with in an incremental manner, so you need to "install" your own. This is the foundation. It requires no configuration to create, and is what all other AMIs will eventually trace back to.

From here, you build a generic base image with the stuff you want. Buri include some basic tools, editors, etc, plus specific tooling Buri uses to simplify some operations in the cloud. Roles may depend on these, especially those that use ephemeral storage, which Buri by default just stripes together and mounts back on /mnt. It also provides a hook for a role to specify directory structures needed to be built under /mnt for it to operate.

And lastly, you can build role based images on top of the base. Each of these is configured for the cloud, and in some cases require multiple nodes to operate by default. 

It can run multi-zone Priam, Exhibitor, and Eureka clusters in EC2. Most of the hooks in Priam for multi region are there but there are other aspects to multi-region deployment not yet in scope for Buri that need to be addressed. This is mostly because if not running Priam in a VPC, it needs to think it is multi-region even if it is not.

There are also several cloud specific roles like:

- Ice, which gives you Amazon billing data analysis, this is standalone from the rest, and can be used even if you have no interest in the rest of the components.
- Edda, which records change in your cloud, and lets you query the history. You can literally tail a log of diff fragments showing the changes in your cloud configuration. 
- Asgard is being worked on now.

Buri is still a young project. It's still a bit of a moving target. There are some awkward bits to it, and others due to change as it grows.

The concept of "environment" is being looked at. I've largely deferred it, as once Asgard is available, it will turn some of how we would handle it otherwise inside-out. Environment will be controlled by it, and the cues that tell the system where it is, will be inject into it as cloud data. Still, to support development VMs, and the like, we need at least a little bit of it, or to provide something similar to what Asgard provides, which at first, is just a bunch of system environment variables.

In normal Ansible, it would be handling this, and defined as part of it's inventory structure. But there's a lot that Buri does which is not the way traditional Ansible works, because we are mostly dealing with transient chroot environments that we also have to setup, tear down, and post-process.

Other long term visions for Buri:

- A public AMI that adds an installer like experience. Boot it up, log in to it, config forms for a given setup, save/load configs to S3, and it generates an AMI set, to that configuration, with feedback from Ansible, which provides detailed reporting on progress, boots up Asgard and off you go. (This is not on the horizon for a while)
- There is a lot more automation on activating IAM policy, security groups, etc that could be done. Waiting to see what Asgard needs/provides before going there.
- The all in one VM will be a priority to maintain.
- While not a focus, trying not to get in the way of using the roles in Buri as "normal" Ansible is respected. This overlaps a bit with the all-in-one vm's needs.
- Eventually, support for Eucalyptus to allow local simulated EC2 clouds for integration testing things requiring that.
  - There will be a sub-project, maybe in Buri, maybe on it's own, which will be an Ansible driven Eucalyptus deployment, based on eucadev.
- Keep adding Netflix components, like Genie, Suro, the Simian Army, and more.
- A clean way to provision WAR files by way of locally building is needed, with component repo+branch/hash/tag spec.
  - where available generic builds are used today.
  - where they are not, builds have been made and are a part of the default in the various roles.

If you have made it this far, thanks, and please consider adding a star to any of the projects you found interesting here on GitHub.

I have an obscure but fun project nearly ready to publish, something completely different... 


