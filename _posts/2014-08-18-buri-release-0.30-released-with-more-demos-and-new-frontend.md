---
layout: post
category : general
tags : [buri]
---
{% include setup %}

I am pleased to announce the next release of [Buri](https://github.com/viafoura/buri), version 0.3.0.

We have new demonstration application it can deploy, lots of fixes, and a shiny new front-end which has a cleaner look, and built-in help on the various commands/actions Buri supports. There are new roles, a new class of role for generic java daemons, and a focus on supporting Cassandra 2.0 where we can.

Read on for more about this release.

<!--fold-->

Here is a quick summary of what is new in Buri v0.3.0:

- new "buri" tool replaces the various helper scripts for launching builds, provides on-line help for the various commands
- AMI building now uses the python-based [awscli](http://aws.amazon.com/cli/) tools instead of the older java-based [EC2 API tools](https://aws.amazon.com/developertools/351). This mostly has the effect of speeding things up.
- source building of Cassandra is now possible in deployments, and we are defaulting to building 2.0 versions now
- Priam is now defaulting to a source build from a fork which includes some fixes, as well as changes to make it work under Cassandra 2.0 better.
- Initial implementation of a NAT role, for providing outbound access to VPC networks.
- Implementation of "jsvc_daemon_instance", which similarly to the existing "jetty9_instance", allows the deployment of multiple applications with common handling. This is for supporting java components that do NOT run under a servlet container.
- Implementation of an all-in-one role for the [Netflix Recipes RSS demo](https://github.com/Netflix/recipes-rss) app, which deploys similarly to the existing Flux Capacitor based demo
- various fixes to issues found by @cfregly, which should make running the ansible from a Mac work better than in did.
- other fixes including:
  - correction to Cassandra init scripts, preventing proper restart, affecting Priam's ability to manage process
  - ensuring JAVA_HOME unformly set in environment
  - various sanity checks in AMI building tasks added

The new front end collects all the various shell scripts previously supplied into one tool, which when invoked with no parameters gives some basic hints as to it's use:

    $ ./buri 
    usage: buri [--help] [--loglevel <level>] [--environment <env_name>]
            
                {fluxdemo,rssdemo,apply,buildhost,foundation,resnap,cleanup_fail,keys_cassandra,keys_bundle}
                ...
    buri: error: too few arguments

Help on indivudual commands can be requested:

    $ ./buri resnap --help
    usage: buri resnap <parent_ami_id> <role>
    buri resnap: error: too few arguments

To deploy the new RSS demo application, you can run:

    $ ./buri rssdemo <ip of your vm>

Which is just shorthand for:

    $ ./buri --environment dev_vm apply all_in_one_rss <ip of your vm>

It's a very simple front end script, should make the use of Buri a little bit easier.

Thanks for listening, if you find buri interesting or useful, please consider giving the [project](https://github.com/viafoura/buri) a star on GitHub, or a click of the social buttons below. :)

@jhohertz

