---
layout: post
category : general
tags : [buri]
---
{% include setup %}

As of the v0.20 release of [Buri](https://github.com/viafoura/buri), we can now build all of the @Netflix stuff from GitHub at build time, or fetch binaries from a URL as before. Each role supporting source build provides a task sequence in tasks/acquire_build.yml, which downloads or builds each item. There are some similarities between some builds, but there is a fair bit of variance too, so each gets a custom "job".

Read on for more about this release.

<!--fold-->

The dev_vm all-in-one target now defaults to source builds, as does any component for which no stable or reasonable snapshot builds are available as far as I can find. (IE: all the ones I have provided before as binaries until this release)

By overriding the build targets in your local environment, you can pin it to snapshots, or any fork/branch in git you need. By doing the former, you can link it you an existing continuous integration setup doing testing/output of each build. By doing the latter, you can follow a any branch, fixed tag/ref, etc from Git. The process is smart enough to leave builds and detect when no change has happened in the update of the repository to skip unnecessary building of packages. This speeds up runs on the all-in-one when used for incremental development.

Here's what the build spec variable look like for a given role. All follow this pattern:

{% highlight yaml %}
turbine_build_source: True
# If false use this war file from URL:
turbine_build_url: http://search.maven.org/remotecontent?filepath=com/netflix/turbine/turbine-web/0.4/turbine-web-0.4.war
# If true use this git repo and branch, tag or other ref:
turbine_git_repo: https://github.com/Netflix/turbine.git
turbine_git_ref: master
{% endhighlight %}
</br>

Other things updated in this release:

- Interactions with jetty upstart from buri is improved, re-pushing to live systems like the all-in-one will restart the respective services as they should. (At least for the Netflix stuff)
- There are various cleanups around Buri not leaving junk around on the build server, would be bad for a CI. May one day make an option to let it be left, as it can be useful to debug.
- Lastly, we no longer use Ubuntu apt-based packages for ansible, but install it via pip, to get the latest 1.6.6, and more importantly, a consistent version regardless of LTS release used.

Thanks for your interest, please consider giving the project a star on GitHub or a click of the social buttons below. :)

@jhohertz

