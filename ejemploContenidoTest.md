This is ApacheBench, Version 2.3 <$Revision: 655654 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking 192.168.246.129 (be patient)


Server Software:        nginx/1.4.4
Server Hostname:        192.168.246.129
Server Port:            80

Document Path:          /index.html
Document Length:        233 bytes

Concurrency Level:      50
Time taken for tests:   0.283 seconds
Complete requests:      1000
Failed requests:        334
   (Connect: 0, Receive: 0, Length: 334, Exceptions: 0)
Write errors:           0
Total transferred:      2336336 bytes
HTML transferred:       2070334 bytes
Requests per second:    3534.76 [#/sec] (mean)
Time per request:       14.145 [ms] (mean)
Time per request:       0.283 [ms] (mean, across all concurrent requests)
Transfer rate:          8064.82 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    1   1.0      0       6
Processing:     4   13  26.4      9     214
Waiting:        4   13  26.4      9     214
Total:          4   14  27.0      9     219
WARNING: The median and mean for the initial connection time are not within a normal deviation
        These results are probably not that reliable.

Percentage of the requests served within a certain time (ms)
  50%      9
  66%     10
  75%     12
  80%     13
  90%     14
  95%     19
  98%     31
  99%    217
 100%    219 (longest request)
