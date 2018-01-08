---
layout: post
title: "Setting Up Travis CI With Fastlane for iOS Project"
date: 2017-12-22T16:02:51+08:00
categories: [iOS]
published: false
---

## Why Travis CI?

If your repository hosted in github. It does not support bitbucket etc..

Very easy to setup, with Xcode images.

## How much it cost?

It is FREE, IF your project is open source eg. public Github project.

First 100 builds are free too, as a trial.

Afterwhich, it starts from [$69/mth](https://travis-ci.com/plans) for 1 concurrent job. Read about alternatives in last section of this post.

## Setting Up

https://docs.travis-ci.com/user/languages/objective-c/

## Encrypting secrets

https://docs.travis-ci.com/user/encryption-keys/

## Dependencies in multiple repository

https://docs.travis-ci.com/user/private-dependencies/

Multiple ways:

1. Deploy key
2. User key

User key is the best practice, with a dedicated [CI user account](https://docs.travis-ci.com/user/private-dependencies/#Dedicated-User-Account). This means creating an independent Github account, with access to those repositories, then generate the user's SSH key for use in travis.

User key is preferred over Deploy key because Deploy key is per repo, so it get hard to maintain so many keys as you have more dependencies.


## Firstly, what is your build process?

What is your git flow? What kind of branches and tags?

When do you need to release beta etc?

## When to build

There are 2 options to turn on/off for each project:

1. Build branch updates
2. Build pull request updates

The default is both ON.

Having (1) ON could be build crazy, because every commit and push will trigger a build.

Hence, you need to [restrict further when to build](https://docs.travis-ci.com/user/customizing-the-build/#Building-Specific-Branches). You can safelist branches and tags, with regex.

So let's say you want only build with tag such as "build-123" to build:

    # safelist
    branches:
      only:
      - master
      - /^build-\d+$/

## After build comes deploy

Deploy is an optional phase, the CD in `CI/CD`.

If build is successful, then the deploy phase will start.

For iOS, the deploy phase could be pushing the build to Testflight.

## Build stages

https://docs.travis-ci.com/user/build-stages/

## Encrypting secure env var

In `.travis.yml`, you will frequently see `secure` key.

These are [environment variables](https://docs.travis-ci.com/user/environment-variables/) that you can use everywhere - travis.yml, Fastfile or any script.

As they are sensitive values, you have to encrypt them in `.travis.yml`.

    # Install the tool
    gem install travis

    # Login
    travis login --pro

    # Encrypt the env var and add to travis.yml
    travis encrypt SOMEVAR="secretvalue" --add

For every key-value, it will add a "secure" to the env.global list

    env:
      global:
      - secure: some-encrypted-value
      - secure: another-encrypted-value

## More resources

https://www.objc.io/issues/6-build-tools/travis-ci/
https://medium.com/practo-engineering/build-distribution-automation-with-fastlane-and-travis-ci-ios-f959dff2799f
http://thebugcode.github.io/ios-continous-integration-choosing-a-build-server-and-tooling/

## Alternatives

The thing with Travis CI is that it is not cheap.

[Bamboo](https://www.atlassian.com/software/bamboo) is part of Atlassian, so it integrates well with Bitbucket, Jira, etc. It starts from $10/mth. [Setup for Xcode](https://confluence.atlassian.com/bamboo/xcode-354353193.html).

[Shippable](https://www.shippable.com/pricing.html) has a generous free hosted service providing 150 builds/mth for private project.

[Xcode Server](https://honzadvorsky.com/articles/2015-08-04-xcs_tutorials_1_getting_started/) aka XCS have bots (like jobs). It integrates to Xcode nicely.

[Jenkins](https://medium.com/@cherrmann.com/continuous-integration-and-delivery-for-ios-with-jenkins-and-fastlane-part-1-3b17f1901a73) is free and you can self host it, on a macOS machine, that also means maintaining it. 

[Spotify CI scripts](https://github.com/spotify/ios-ci) for their open source projects. They use Travis CI, but also has
