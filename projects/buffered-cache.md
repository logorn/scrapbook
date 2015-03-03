---
layout: project
title: Buffered cache
description: BufferedStore helps reduce requests to your real cache. If you need the request the same value more than once (from various places in your code), it can be a pain to keep that value around. Requesting it again from cache would be easier, but then you get some latency from the connection to the cache server.
weight: 2
icon: fa fa-rocket
project: scrapbook/buffered-cache
class: Scrapbook\Buffered\BufferedStore
---

```php
// create \Memcached object pointing to your Memcached server
$client = new \Memcached();
$client->addServer('localhost', 11211);
// create Scrapbook cache object
$cache = new \Scrapbook\Adapters\Memcached($client);

// create buffered cache layer over our real cache
$buffered = new \Scrapbook\Buffered\BufferedStore($cache);

// set a value
$buffered->set('key', 'value'); // returns true

// get a value
// it won't need to get it from memcached, it'll be read from PHP memory
$buffered->get('key'); // returns 'value'
```

BufferedStore helps reduce requests to your real cache. If you need the request
the same value more than once (from various places in your code), it can be a
pain to keep that value around. Requesting it again from cache would be easier,
but then you get some latency from the connection to the cache server.

BufferedStore will keep known values (items that you've already requested or
written yourself) in memory. Every time you need that value in the same request,
it'll just get it from memory instead of going back to the cache server.

BufferedStore just wraps around any KeyValueStore and is itself a KeyValueStore.
Just use it like you would call any cache, but enjoy the reduced headaches about
requesting the same date multiple times!