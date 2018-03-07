### Test Template Engine
#### Prepare Work
test environment: windows10 4C 8G

test tools: apache-ab

spring root version: 2.0.0.RELEASE

test method: 20 requests per time and 1000 requests total

test data
```$xslt
{
        ArrayList arrayList = new ArrayList();
        // prepare info
        for (int i = 0; i < 10; i++) {
            Map<String, Object> userInfo = new HashMap<String, Object>();
            userInfo.put("username", "ares" + (i + 1));
            userInfo.put("age", 18 + i);
            if(i%2==0){
                userInfo.put("gender","male");
            }else{
                userInfo.put("gender","female");
            }
            arrayList.add(userInfo);
        }
    }

    @RequestMapping("testFreeMark")
    public String testFreeMark(Map<String, Object> model) {
        model.put("myList", arrayList);

        return "freeMark";
    }

    @RequestMapping("testThymeleaf")
    public String testThymeleaf(Model model){
        model.addAttribute("myList",arrayList);
        return "thymeleaf";
    }
```
freemark template
```$xslt
<html>
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<h3>This template engine is freemark!</h3>
<table border="1">
    <tr>
        <th>Name</th>
        <th>Age</th>
    </tr>
    <#list myList as item>
        <#if item.gender == "male">
            <tr>
                <td>${item.username}</td>
                <td>${item.age}</td>
            </tr>
        </#if>
    </#list>
</table>

</body>
</html>
```
thymeleaf template
```$xslt
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8"/>
    <title>Title</title>
</head>
<body>
<h3>This template engine is thymeleaf!</h3>
<table border="1">
    <tr>
        <th>Name</th>
        <th>Age</th>
    </tr>
    <tr th:each="item,iterStat :${myList}" th:if="${item.gender == 'male'}">
        <td th:text="${item.username}"></td>
        <td th:text="${item.age}"></td>
    </tr>
</table>
</body>
</html>
```

#### Test Result
##### freemark
result1:
```$xslt
D:\easternbay\Apache\Apache24\bin>ab -n 1000 -c 20 http://localhost:8080/testFreeMark
This is ApacheBench, Version 2.3 <$Revision: 1807734 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8080

Document Path:          /testFreeMark
Document Length:        739 bytes

Concurrency Level:      20
Time taken for tests:   0.189 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      876000 bytes
HTML transferred:       739000 bytes
Requests per second:    5305.01 [#/sec] (mean)
Time per request:       3.770 [ms] (mean)
Time per request:       0.189 [ms] (mean, across all concurrent requests)
Transfer rate:          4538.27 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.3      0       1
Processing:     0    4   0.7      4       6
Waiting:        0    3   0.9      3       5
Total:          0    4   0.7      4       6

Percentage of the requests served within a certain time (ms)
  50%      4
  66%      4
  75%      4
  80%      4
  90%      5
  95%      5
  98%      5
  99%      5
 100%      6 (longest request)
```
result2:
```$xslt
D:\easternbay\Apache\Apache24\bin>ab -n 1000 -c 20 http://localhost:8080/testFreeMark
This is ApacheBench, Version 2.3 <$Revision: 1807734 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8080

Document Path:          /testFreeMark
Document Length:        739 bytes

Concurrency Level:      20
Time taken for tests:   0.192 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      876000 bytes
HTML transferred:       739000 bytes
Requests per second:    5195.80 [#/sec] (mean)
Time per request:       3.849 [ms] (mean)
Time per request:       0.192 [ms] (mean, across all concurrent requests)
Transfer rate:          4444.85 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.3      0       1
Processing:     0    4   0.9      4      10
Waiting:        0    3   0.9      3       9
Total:          0    4   0.9      4      10

Percentage of the requests served within a certain time (ms)
  50%      4
  66%      4
  75%      4
  80%      4
  90%      5
  95%      5
  98%      6
  99%      7
 100%     10 (longest request)
```
result3:
```$xslt
D:\easternbay\Apache\Apache24\bin>ab -n 1000 -c 20 http://localhost:8080/testFreeMark
This is ApacheBench, Version 2.3 <$Revision: 1807734 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8080

Document Path:          /testFreeMark
Document Length:        739 bytes

Concurrency Level:      20
Time taken for tests:   0.171 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      876000 bytes
HTML transferred:       739000 bytes
Requests per second:    5833.52 [#/sec] (mean)
Time per request:       3.428 [ms] (mean)
Time per request:       0.171 [ms] (mean, across all concurrent requests)
Transfer rate:          4990.40 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.3      0       1
Processing:     0    3   0.6      3       7
Waiting:        0    2   0.8      2       7
Total:          0    3   0.7      3       7

Percentage of the requests served within a certain time (ms)
  50%      3
  66%      3
  75%      4
  80%      4
  90%      4
  95%      4
  98%      4
  99%      5
 100%      7 (longest request)
```
#### thymeleaf
result1
```$xslt
D:\easternbay\Apache\Apache24\bin>ab -n 1000 -c 20 http://localhost:8080/testThymeleaf
This is ApacheBench, Version 2.3 <$Revision: 1807734 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8080

Document Path:          /testThymeleaf
Document Length:        632 bytes

Concurrency Level:      20
Time taken for tests:   0.266 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      769000 bytes
HTML transferred:       632000 bytes
Requests per second:    3763.64 [#/sec] (mean)
Time per request:       5.314 [ms] (mean)
Time per request:       0.266 [ms] (mean, across all concurrent requests)
Transfer rate:          2826.41 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.4      0       1
Processing:     1    5   2.9      4      24
Waiting:        1    4   2.7      4      24
Total:          2    5   2.9      4      24

Percentage of the requests served within a certain time (ms)
  50%      4
  66%      5
  75%      6
  80%      6
  90%      8
  95%     11
  98%     15
  99%     17
 100%     24 (longest request)
```
result2
```$xslt
D:\easternbay\Apache\Apache24\bin>ab -n 1000 -c 20 http://localhost:8080/testThymeleaf
This is ApacheBench, Version 2.3 <$Revision: 1807734 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8080

Document Path:          /testThymeleaf
Document Length:        632 bytes

Concurrency Level:      20
Time taken for tests:   0.262 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      769000 bytes
HTML transferred:       632000 bytes
Requests per second:    3821.32 [#/sec] (mean)
Time per request:       5.234 [ms] (mean)
Time per request:       0.262 [ms] (mean, across all concurrent requests)
Transfer rate:          2869.72 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.3      0       1
Processing:     1    5   2.9      4      24
Waiting:        1    5   2.8      4      24
Total:          1    5   2.9      4      24

Percentage of the requests served within a certain time (ms)
  50%      4
  66%      5
  75%      6
  80%      6
  90%      9
  95%     11
  98%     13
  99%     16
 100%     24 (longest request)
```
result3:
```$xslt
D:\easternbay\Apache\Apache24\bin>ab -n 1000 -c 20 http://localhost:8080/testThymeleaf
This is ApacheBench, Version 2.3 <$Revision: 1807734 $>
Copyright 1996 Adam Twiss, Zeus Technology Ltd, http://www.zeustech.net/
Licensed to The Apache Software Foundation, http://www.apache.org/

Benchmarking localhost (be patient)
Completed 100 requests
Completed 200 requests
Completed 300 requests
Completed 400 requests
Completed 500 requests
Completed 600 requests
Completed 700 requests
Completed 800 requests
Completed 900 requests
Completed 1000 requests
Finished 1000 requests


Server Software:
Server Hostname:        localhost
Server Port:            8080

Document Path:          /testThymeleaf
Document Length:        632 bytes

Concurrency Level:      20
Time taken for tests:   0.270 seconds
Complete requests:      1000
Failed requests:        0
Total transferred:      769000 bytes
HTML transferred:       632000 bytes
Requests per second:    3707.75 [#/sec] (mean)
Time per request:       5.394 [ms] (mean)
Time per request:       0.270 [ms] (mean, across all concurrent requests)
Transfer rate:          2784.44 [Kbytes/sec] received

Connection Times (ms)
              min  mean[+/-sd] median   max
Connect:        0    0   0.4      0       1
Processing:     2    5   3.2      4      35
Waiting:        1    5   3.1      4      35
Total:          2    5   3.2      4      36

Percentage of the requests served within a certain time (ms)
  50%      4
  66%      5
  75%      6
  80%      6
  90%      9
  95%     11
  98%     16
  99%     20
 100%     36 (longest request)
```
### Result

&nbsp;| freemark | thymeleaf 
---- | --- | ---
test1 | 0.189 | 0.266
test2 |  0.192 | 0.262
test3 |  0.171 | 0.270