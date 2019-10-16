---
layout: post
title: "Migrating App Engine to Go 1.12"
date: 2019-10-16T17:29:59+08:00
categories: [Golang, AppEngine]
---

I was working one of those dreadful migration task today.

Google **deprecated Go versions 1.9**, that means I can no longer deploy my app engine projects that use the older versions.

There are [quite many changes](https://cloud.google.com/appengine/docs/standard/go112/go-differences#migrating-appengine-sdk).

Some are considerably minor:

- change of `app.yaml` structure,
- the need for a `main` package, and
- replacing `google.golang.org/appengine` (urlfetch, log) with golang ones

Minor, yet it does have much code change.

## No More Memcache

The biggest change turned out to be memcache.

> To use a memcache service on App Engine, use Redis Labs Memcached Cloud instead of App Engine Memcache.

Ouch.

I now need to use a managed service from [Redis](https://redislabs.com), with it's own pricing.

Luckily, Redis starts with a **free tier of 30mb (memory)**, which might be sufficient for my small project.

## A guide to Redis with golang

App Engine has [a guide to setting up redis](https://cloud.google.com/appengine/docs/flexible/python/using-redislabs-redis).

Good enough guide, except for **one big problem**. There is a [typo](https://github.com/GoogleCloudPlatform/golang-samples/issues/1023).

![](/images/appengine-redis-typo.jpg)

`REDIS_PASSSWORD` is wrongly spelled with triple-s.. That wasted 1 hour of my time.

They would have [probably fixed](https://github.com/GoogleCloudPlatform/golang-samples/issues/1023) by the time you are reading this. But one wonders how Google make such mistake.

## Using redigo

[redigo](https://godoc.org/github.com/gomodule/redigo/redis) is the Redis library in golang.

I have a very simple use case. Set a key-value with expiration, and get it.

### 1. Set with expiration

This can be archived with [SETEX](https://redis.io/commands/setex), which is like SET + EXPIRE but atomic.

```go
func saveToMemcache(key string, value string) {
  redisConn := redisPool.Get()
  defer redisConn.Close()

  expiration := 30
  _, err := redisConn.Do("SETEX", key, expiration, value)
  ...
}
```

### 2. Get from Redis

```go
func getFromMemcache(key string) {
  redisConn := redisPool.Get()
  defer redisConn.Close()

  value, err := redis.String(redisConn.Do("GET", key))
  ...
}
```

Note that if the key is unavailable/expired, there will be an error.

## Redis CLI

You can also connect to redis via CLI. Install with `brew install redis`, then:

    redis-cli -u redis://ADDRESS:PORT -a PASSWORD

You can then send commands. For example:

- `SETEX key 30 value` to set with expiration 30 seconds
- `TTL key` to know the number of seconds to expiration
- `GET key`
- `FLUSHALL` to remove all keys
    
## Deploy

Another change is that you have to deploy using `gcloud` with the project id and version, instead of specifying them in `app.yaml`.

    gcloud app deploy --project myproject --version 123

The specified version will also be promoted as the default version.
