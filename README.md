# NOTE
I no longer use boxen. Instead I've crafted my own set of setup files in my dotfiles repo: https://github.com/niallmccullagh/dotfiles

# my-Boxen Base Box

A [bunch of puppet](http://boxen.github.com/) for OSX development environments maintained by my webdevs. This community-curated environment automates most of your laptop setup and maintenance.

# Getting Started

You'll need to manually install Xcode with the command line extensions, and turn on FileVault's full disk encryption.

Then, visit http://niallmccullagh.github.io/our-boxen/ and follow the on screen instructions. To inspect exactly what's going to be run, click the instructions to see the script that will be run.

You'll be prompted with a few questions initially, but most of the install will pass by unattended. At the end you'll be asked to open a new shell or source something -- just open a new shell.

Afterwards, periodically run the command `boxen` to pull down updates. You probably don't want to do this every time you source a shell, but daily or weekly is a good idea. Consider setting up a cron.

# What will it install?

It will install a bunch of stuff into `/opt/boxen`, and set up a repository in `~/src/our-boxen`. It will also setup a couple common tools and development environments, including --

* git
* homebrew
* iterm2
* nginx
* node
* ruby
* dnsmasq w/ .dev resolver for localhost
* ack
* Findutils
* GNU tar
* Hipchat
* Java 1.7
* Alfred	
* Google chrome
* Google notifier
* Intellij

and more. Details on _exactly_ what is setup for everyone is in the [`site.pp`](https://github.com/niallmccullagh/our-boxen/tree/master/manifests/site.pp).

# Customizing it

First cd into `~/src/our-boxen`, or wherever your local boxen repo is located. This is a git repo containing the group's boxen. You can create branches, make your modifications, push to your fork, and make PRs like any other repo. The `boxen` command tries to update this git repo first, so if you have a dirty tree or if you've diverged from master it will complain loudly. Local changes will still apply, but you won't be able to pull down updates automatically if you diverge. Be careful modifying this -- the boxen is a shared resource across projects.

You can customize your install with a [personal manifest](https://github.com/niallmccullagh/our-boxen/tree/master/modules/people). Check out existing manifests for examples. For a list of software you can include, see the [Puppetfile](https://github.com/niallmccullagh/our-boxen/tree/master/Puppetfile#L39).

You can also define [project manifests](https://github.com/niallmccullagh/our-boxen/tree/master/modules/projects). Project manifests can then be `include`d in personal manifests for users who want to work on a given project.

New software packages can be added as options as well. Start by modifying the `Puppetfile` to include a new module:

```
github "{{package name}}", "{{ tag }}"[, :repo => "{{ ghuser }}/{{ repo name}}"]
```

Packages follow this particular format corresponding to a github repo. By default, the `github` macro assumes your package is on github in the repo `boxen/puppet-{{package name}}`. If it's located elsewhere (like some of our custom packages) you'll need to specify the optional `:repo` symbol.

Then, run `boxen` locally. Boxen will install the package and cache a tarball. `git add` the changes, including the cached tarball, to the repository. Don't forget that you'll need to include the new package in `site.pp`, a project, or your personal manifest for it to take effect. Then commit your changes and open a PR to get it upstreamed into the our-boxen.

If you're using a Github Enterprise instance rather than github.com,
you will need to set the `BOXEN_GITHUB_ENTERPRISE_URL` and
`BOXEN_REPO_URL_TEMPLATE` variables in your
[Boxen config](config/boxen.rb).

# Troubleshooting

Boxen relies heavily on the Github API, and when you're developing for Boxen you can run out of API requests. To help mitigate this, Boxen will try to auth with Github and vendor things in the repo. If you run out of API requests, all you can do is wait.

Any other issues can be filed against this repo as issues.
