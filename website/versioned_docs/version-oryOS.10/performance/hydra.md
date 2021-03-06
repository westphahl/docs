---
id: version-oryOS.10-hydra
title: ORY Hydra
original_id: hydra
---

In this document you will find benchmark results for different endpoints of ORY Hydra. All benchmarks are executed
using [rakyll/hey](https://github.com/rakyll/hey). Please note that these benchmarks run against the in-memory storage
adapter of ORY Hydra. These benchmarks represent what performance you would get with a zero-overhead database implementation.

We do not include benchmarks against databases (e.g. MySQL or PostgreSQL) as the performance greatly differs between
deployments (e.g. request latency, database configuration) and tweaking individual things may greatly improve performance.
We believe, for that reason, that benchmark results for these database adapters are difficult to generalize and potentially
deceiving. They are thus not included.

This file is updated on every push to master. It thus represents the benchmark data for the latest version.

All benchmarks run 10.000 requests in total, with 100 concurrent requests. All benchmarks run on Circle-CI with a
["2 CPU cores and 4GB RAM"](https://support.circleci.com/hc/en-us/articles/360000489307-Why-do-my-tests-take-longer-to-run-on-CircleCI-than-locally-)
configuration.

## BCrypt

ORY Hydra uses BCrypt to obfuscate secrets of OAuth 2.0 Clients. When using flows such as the OAuth 2.0 Client Credentials
Grant, ORY Hydra validates the client credentials using BCrypt which causes (by design) CPU load. CPU load and performance
depend on the BCrypt cost which can be set using the environment variable `BCRYPT_COST`. For these benchmarks,
we have set `BCRYPT_COST=8`.

## OAuth 2.0

This section contains various benchmarks against OAuth 2.0 endpoints

### Token Introspection

```

Summary:
  Total:	0.5342 secs
  Slowest:	0.0211 secs
  Fastest:	0.0001 secs
  Average:	0.0051 secs
  Requests/sec:	18719.2954
  
  Total data:	1550000 bytes
  Size/request:	155 bytes

Response time histogram:
  0.000 [1]	|
  0.002 [2852]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.004 [1007]	|■■■■■■■■■■■■
  0.006 [1504]	|■■■■■■■■■■■■■■■■■■
  0.008 [3274]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.011 [993]	|■■■■■■■■■■■■
  0.013 [245]	|■■■
  0.015 [68]	|■
  0.017 [37]	|
  0.019 [8]	|
  0.021 [11]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0011 secs
  50% in 0.0060 secs
  75% in 0.0077 secs
  90% in 0.0090 secs
  95% in 0.0101 secs
  99% in 0.0133 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0211 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0046 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0056 secs
  resp wait:	0.0050 secs, 0.0001 secs, 0.0178 secs
  resp read:	0.0000 secs, 0.0000 secs, 0.0031 secs

Status code distribution:
  [200]	10000 responses



```

### Client Credentials Grant

This endpoint uses [BCrypt](#bcrypt).

```

Summary:
  Total:	18.9665 secs
  Slowest:	0.6887 secs
  Fastest:	0.0166 secs
  Average:	0.1824 secs
  Requests/sec:	527.2467
  
  Total data:	1570000 bytes
  Size/request:	157 bytes

Response time histogram:
  0.017 [1]	|
  0.084 [1336]	|■■■■■■■■■■■■■■■■■■
  0.151 [2835]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.218 [2917]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.285 [1257]	|■■■■■■■■■■■■■■■■■
  0.353 [919]	|■■■■■■■■■■■■■
  0.420 [558]	|■■■■■■■■
  0.487 [92]	|■
  0.554 [69]	|■
  0.621 [13]	|
  0.689 [3]	|


Latency distribution:
  10% in 0.0745 secs
  25% in 0.1040 secs
  50% in 0.1824 secs
  75% in 0.2346 secs
  90% in 0.3096 secs
  95% in 0.3832 secs
  99% in 0.4826 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0001 secs, 0.0166 secs, 0.6887 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0130 secs
  req write:	0.0001 secs, 0.0000 secs, 0.0779 secs
  resp wait:	0.1813 secs, 0.0165 secs, 0.6887 secs
  resp read:	0.0007 secs, 0.0000 secs, 0.1658 secs

Status code distribution:
  [200]	10000 responses



```

## OAuth 2.0 Client Management

### Creating OAuth 2.0 Clients

This endpoint uses [BCrypt](#bcrypt) and generates IDs and secrets by reading from  which negatively impacts
performance. Performance will be better if IDs and secrets are set in the request as opposed to generated by ORY Hydra.

```
This test is currently disabled due to issues with /dev/urandom being inaccessible in the CI.
```

### Listing OAuth 2.0 Clients

```

Summary:
  Total:	0.3493 secs
  Slowest:	0.0372 secs
  Fastest:	0.0001 secs
  Average:	0.0033 secs
  Requests/sec:	28631.7497
  
  Total data:	5090000 bytes
  Size/request:	509 bytes

Response time histogram:
  0.000 [1]	|
  0.004 [6190]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.008 [2217]	|■■■■■■■■■■■■■■
  0.011 [1145]	|■■■■■■■
  0.015 [328]	|■■
  0.019 [77]	|
  0.022 [18]	|
  0.026 [12]	|
  0.030 [6]	|
  0.034 [3]	|
  0.037 [3]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0001 secs
  50% in 0.0009 secs
  75% in 0.0055 secs
  90% in 0.0092 secs
  95% in 0.0110 secs
  99% in 0.0153 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0372 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0079 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0089 secs
  resp wait:	0.0030 secs, 0.0000 secs, 0.0351 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0064 secs

Status code distribution:
  [200]	10000 responses



```

### Fetching a specific OAuth 2.0 Client

```

Summary:
  Total:	0.3579 secs
  Slowest:	0.0255 secs
  Fastest:	0.0001 secs
  Average:	0.0033 secs
  Requests/sec:	27942.0599
  
  Total data:	5070000 bytes
  Size/request:	507 bytes

Response time histogram:
  0.000 [1]	|
  0.003 [5582]	|■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
  0.005 [1471]	|■■■■■■■■■■■
  0.008 [1403]	|■■■■■■■■■■
  0.010 [883]	|■■■■■■
  0.013 [392]	|■■■
  0.015 [176]	|■
  0.018 [68]	|
  0.020 [15]	|
  0.023 [7]	|
  0.026 [2]	|


Latency distribution:
  10% in 0.0001 secs
  25% in 0.0002 secs
  50% in 0.0011 secs
  75% in 0.0058 secs
  90% in 0.0090 secs
  95% in 0.0110 secs
  99% in 0.0151 secs

Details (average, fastest, slowest):
  DNS+dialup:	0.0000 secs, 0.0001 secs, 0.0255 secs
  DNS-lookup:	0.0000 secs, 0.0000 secs, 0.0066 secs
  req write:	0.0000 secs, 0.0000 secs, 0.0075 secs
  resp wait:	0.0031 secs, 0.0000 secs, 0.0255 secs
  resp read:	0.0001 secs, 0.0000 secs, 0.0091 secs

Status code distribution:
  [200]	10000 responses



```
