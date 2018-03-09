---
layout: post
title: "Setting Up Travis CI With Fastlane for iOS Project"
date: 2018-02-22T16:02:51+08:00
categories: [iOS]
---

[Travis CI](https://travis-ci.com) is a service, providing machines, to help build your app, test, deploy, and more.

Note: It integrates very easily with github, but does NOT support bitbucket etc.

It is FREE for open source projects eg. public Github project.

First 100 builds are free too, as a trial. Afterwhich, it starts from [$69/mth](https://travis-ci.com/plans) for 1 concurrent job. Read about alternatives in last section of this post.

## [Setting Up](https://docs.travis-ci.com/user/languages/objective-c/)

1. Create `.travis.yml` in project root and configure (see below)
2. Push to repository
3. Sign in your Github account on [travis-ci.com](https://travis-ci.com/)
4. Configure on the dashboard

The most important step is with `.travis.yml` configuration file. 

You can [customize](https://docs.travis-ci.com/user/customizing-the-build) all you want. A bare minimum one for swift looks like this:

```yaml
osx_image: xcode9.2
language: swift
script:
  - fastlane run_tests
```

The [lane "run_tests"](https://docs.fastlane.tools/actions/scan/), builds and run unit tests. The result will then be available to travis-ci.

## When to build

There are 2 options to turn on/off in the dashboard:

1. Build branch updates
2. Build pull request updates

The default is both ON.

Having (1) ON could be build crazy, because every commit and push will trigger a build. But you could do tweak more, with advanced configuration.

## When to build (Advanced)

You may [further restrict when to build](https://docs.travis-ci.com/user/customizing-the-build/#Building-Specific-Branches). You can safelist branches and tags, with regex.

So let's say you want only build with tag such as "build-123", you can use regex in the yaml file:

```yaml
# safelist
branches:
  only:
  - master
  - /^build-\d+$/
```

Another way is to write bash commands in the `script` phrase.

For example, if you want to run a lane named "beta" when the commit message has "[beta]" in it:

```yaml
script: 
  - if [[ "$TRAVIS_COMMIT_MESSAGE" = *"[beta]"* ]]; then bundle exec fastlane beta; else bundle exec fastlane some_other_lane; fi
```

`TRAVIS_COMMIT_MESSAGE` is [one of many environment variables](https://docs.travis-ci.com/user/environment-variables/#Default-Environment-Variables) provided in travis.

## After build comes deploy

Deploy is an optional phase, the CD in `CI/CD`.

If build is successful, then the deploy phase will start.

For iOS, the deploy phase could be pushing the build to Testflight.

## Encrypting secure env var

In `.travis.yml`, you will frequently see `secure` key.

These are [environment variables](https://docs.travis-ci.com/user/environment-variables/) that you can use everywhere - travis.yml, Fastfile or any script.

As they are sensitive values, you have to encrypt them in `.travis.yml`.

Run this in terminal:

```bash
# Install the tool
gem install travis

# Login
travis login --pro

# Encrypt the env var and add to travis.yml
travis encrypt SOMEVAR="secretvalue" --add
```

For every key-value, it will add a "secure" to the env.global list.

```yaml
env:
  global:
  - secure: some-encrypted-value
  - secure: another-encrypted-value
```

You can then [use the key](https://docs.travis-ci.com/user/encryption-keys/) eg. `SOMEVAR` as an environment variable.

## Bonus: Dependencies in multiple repository

If your project has [dependencies to multiple repositories](https://docs.travis-ci.com/user/private-dependencies/), there are 2 approach:

1. Deploy key
2. User key

User key is the best practice, with a dedicated [CI user account](https://docs.travis-ci.com/user/private-dependencies/#Dedicated-User-Account). This means creating an independent Github account, with access to those repositories, then generate the user's SSH key for use in travis.

User key is preferred over Deploy key because Deploy key is per repo, hence it gets hard to maintain many keys (1 key per repo).

## Alternatives

The thing with Travis CI is that it is expensive, starting from $69/mth.

[Bamboo](https://www.atlassian.com/software/bamboo) is part of Atlassian, so it integrates well with Bitbucket, Jira, etc, and starts from $10/mth. [Setup for Xcode](https://confluence.atlassian.com/bamboo/xcode-354353193.html).

[Shippable](https://www.shippable.com/pricing.html) has a generous free hosted service providing 150 builds/mth for private project.

[Xcode Server](https://honzadvorsky.com/articles/2015-08-04-xcs_tutorials_1_getting_started/) aka XCS have bots (like jobs). It integrates to Xcode nicely.

[Jenkins](https://medium.com/@cherrmann.com/continuous-integration-and-delivery-for-ios-with-jenkins-and-fastlane-part-1-3b17f1901a73) is free and you can self host it, on a macOS machine, that also means maintaining it. 

Also, sample [Spotify CI scripts](https://github.com/spotify/ios-ci) for their open source projects. They use Travis CI and others. 
