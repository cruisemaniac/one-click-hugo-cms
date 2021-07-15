+++
authors = ["Ashwin M"]
date = 2020-06-28T17:30:00Z
description = "Getting on the Docker bandwagon is no longer an option - Its the way to go. How do you do it right for your Ruby on Rails app "
hero = ""
images = ["/images/1__uq2o1kma6dbi3_nnobvoa.png"]
share = true
tags = ["rails", "docker", "containerisation", "cloud"]
timeToRead = 0
title = "Dockerizing your rails app - Only the gotchas!"

+++
Containerisation is all the rage these days with every cloud provider offering some way or the other to deploy and manage containers. Running your Ruby on Rails application on a containerised platform will let you not only save, it also lets you get your changes faster to your end users. Are you doing it right or missing the obvious??? <!--more-->

I have been meaning to run a few blog posts regarding containerising your apps and this is the first of them.

I've worked with Rails apps for a good part of my career and am proud to say we've put quite a few of them into containers and pushed them onto the cloud. Here goes -

### Start with your Dockerfile

In a majority of cases, your app isnt starting from the ground up. You're re-modelling the deployment - taking it from a traditional compute virtualised architecture to a container based one.

This means one major thing - You need to get your Dev and Ops folks together and get started on the `Dockerfile`.

> Help your Devs with the `Dockerfile`! Ops know's how to optimise it, but more often than not, Devs know what goes where!

Here's a sample from a Dockerfile template that I often start off of:

    FROM ruby:2.5-alpine
    LABEL maintainer="Ashwin Murali <mail@cruisemaniac.com>"
    RUN apk update && apk add build-base nodejs
    postgresql-dev
    RUN mkdir /app
    WORKDIR /app
    COPY Gemfile Gemfile.lock ./
    RUN bundle install
    COPY . .
    CMD puma -C config/puma.rb

Notice from the snippet above that I do not use Ubuntu / Debian as a base image. And there's a reason. Alpine is light enough for you to run Ruby on Rails and there's more often than not, nothing else required for your app ecosystem.

> Remember, Your container is a runtime. Do not treat it as an OS / Ecosystem.

As an Ops person, you should make it a point to work with your dev teams to get this running and building properly.

### YOU DO NOT NEED UBUNTU:16.04

Have I said that before? Well, let me say that again! YOU DO NOT NEED UBUNTU:16.04. There's a very rare case that you need specialised libraries that alpine linux does not provide!

An example is [jemalloc](https://github.com/jemalloc/jemalloc "jemalloc") - the malloc implementation that helps handle fragmentation and high concurrency. I've been there!

Alpine doesnt support jemalloc as of this writing. So switch to something that does, but not a full OS - we used [Debian-slim](https://hub.docker.com/layers/debian/library/debian/buster-slim/images/sha256-7c459309b9a5ec1683ef3b137f39ce5888f5ad0384e488ad73c94e0243bc77d4?context=explore "Debian Buster Slim"). There's always workarounds! Explore them!

### Local Development

![](/images/dev-it-works.jpg#center)

Now that you have a base `Dockerfile` in place, it is important that you as an ops person enable local development using said Dockerfile. The tool you should spend time on - `docker-compose`.

Your devs should be enabled to run their full app stacks using `docker-compose up`. At this point, your developing on local machines, testing them inside docker, having them break - this recurrence should be a thing of the past.

#### The thing about data

Its a usual demand that devs and test folks need data to work through the myriad of use cases in your app. As an org, you should make it a process to mock that data - whether locally stored data or coming in via 3rd party end points - And running a container that mocks data is an absolutely trivial task.

Invest in it. You'll thank me later!

### Stage / UAT / Prod

This is where most of the Oh! Crap! moments come in. And here's a non-definitive list to look out for:

#### Separation of concerns

Ensure that each docker image runs one process. And runs it well. Don't go lets run rails, let's run sidekiq, let's run cron, let's run iron man on the container!

> Run one process and one process only per container.

Use docker's `CMD` to override the command you'll execute on the same codebase.

#### Work with DNS

Your entire docker environment - swarm, kubernetes, fargate, whatever - they're going to run on ephemeral IPs. That is because Docker orchestration systems consider the container itself to be an ephemeral entity. It can run, it can be killed, it can be restarted again to serve a customer's request.

This guarantees one thing - you never know where another service is - if you're looking with an IP. Learn to use DNS. Learn to use service discovery.

> `http://servicename:3000` is any day better!

#### Statelessness is the game

I left this for the last!

> DO not write to your docker file system!

Use your persistent stores to store final state data - your databases, your file blob stores (s3, cloud storage, etc).

Use a sidecar to pick up logs and move it out of the container. You want your logs to move at the pace of your container. You do not want to be left in the lurch if the container has to be stopped and your logs havent been picked up.

I hope the list above is of a bit of help when wading through the process.

Until next time!