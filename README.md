# credis - C client library for Redis

**Credis is a client library in plain C** for communicating with Redis servers. Redis is a high performance key-value database, refer to [Redis project page](https://redis.io) for more information.

Credis aims to be fast and minimalistic with respect to memory usage. It supports connections to multiple Redis servers. It runs on Linux, OS X, Windows, FreeBSD and should run on most POSIX like systems. It is released under the New BSD License.

## Why

Redis seemed like what I was looking for in a social web project with high demands on scalability. Unfortunately a plain C client library was not available - until now.

## Status

It was recently moved from [http://code.google.com/p/credis] to github to keep it alive. It has been forked to github before and is actively maintained in those forks.

Current status is "almost complete". Credis implements all commands supported by Redis 1.02, almost all of 1.2.6 and most of 2.0.2. Credis is work in progress but still quite mature. The long-term commitment is to provide full support for all Redis commands. A simple credis test client is available as example and for test. Refer to credis.h for a complete list of supported commands.

# Credits

Credis is written and _was actively maintained_ by Jonas Romfelt.

List, in no particular order, of people who has contributed to Credis:
* Florian octo Forster format warnings and a new build system waiting to be merged to trunk 
* Louis-Philippe Perron for help with debugging 
* Dean Banks for pointing out and sending me a patch to remove non-reentrant calls in credis_connect() 
* Dmitriy Lyfar for suggesting a patch for timed connect. 
* Jeff Buck for a credis_info() patch. 
* Fernando Pardo for a WIN32 patch and help with pub/sub implementation. 
* Jonathan Ragan-Kelley for proposing simplifications to the initial API. 
* akitada making Credis build under Mac OS X. 
* Matthieu Tourne for C/C++ source mixing. 
* Daniel Korger for help with bug reporting and testing fixes.

# Build credis

To build credis-test and credis shared and static libraries simply run: 

´´´make´´´

To run through a number of credis tests run (presupposed that a Redis server is listening on the default port on localhost):

´´´./credis-test´´´

To perform a simplistic performance benchmark test and measure number of set-commands per second, run:

´´´./credis-test 10000´´´

Where 10000 is the number of set-commands to execute. 

Running the benchmark and a redis srver instance locally on my machine (old dual-core AMD64 and 2 GB RAM running Debian) roughly results in 33000 commands/second. Slightly (10-15%) faster than the benchmark provided with the redis server. 

# Example: connecting to a Redis server

The example below connects to a Redis server running on the same machine. Sets a key value; and retrieves and displays the newly set key value. Then the example closes the connection. 

Error handling has been left out for simplicity.

´´´c
#include <stdio.h>
#include "credis.h"

int main(int argc, char **argv)
{
  REDIS rh;
  char *val;

  /* create handle to a Redis server running on localhost, port 6789,
     with a 2 second response timeout */
  rh = credis_connect(NULL, 6789, 2000);

  /* ping server */
  credis_ping(rh);

  /* set value of key "foo" to "bar" */
  credis_set(rh, "foo", "bar");

  /* get value of key "foo" */
  credis_get(rh, "foo", &val);
  printf("get foo returned: %s\n", val);

  /* close connection to redis server */
  credis_close(rh);

  return 0;
}
´´´
