# 4.6 things we learned using ab (apache bench)  tool.

From the apache docs: _“ab is a tool for benchmarking your Apache Hypertext Transfer Protocol (HTTP) server. It is designed to give you an impression of how your current Apache installation performs. This especially shows you how many requests per second your Apache installation is capable of serving.”_

For extra info go to https://httpd.apache.org/docs/2.4/programs/ab.html

ok the things we learned clickbait: 

##  1. -v 2  
    **-v 2**  option is your friend. It sets the verbosity of the report.  
	  **-v 1** shows you not much  
	  **-v 2** shows just about right amount (the most important thing status codes of responses!)  
	  **-v 4** shows matrix like stream of stuff 

## 2.  -c 
    Setting up cookies for your requests is almost easy as docs says that _“This field is repeatable.”_  
    well it is **NOT** so set it this way: (if you need to set more then one)

  ```-C "cookie-name-1=abc; cookie-name-2=def"```

## 3. -m 
  -m quite important a parameter  “Custom HTTP method for the requests. Available in 2.4.10 and later.”  you can set it to -m         DELETE | PUT | POST | GET (default)  but if you posting the file 
  -p (see 4) you can’t set it as by default it will be POST

## 4. -p - T
  To test POST endpoints with body you need to use -p body.json with filename to json data and set -T  i.e. -T  “application/json"  the file has to be in format { “key”:  {  “foo": “bar” } }. 
  Keys have to b in wrapped in “” !!!!! Remember “key” not key

##4.6 so lets run this thing the simple command for GET api cold look like this: 

```ab -n 10 -c 10 -v 2 http://quotesondesign.com/wp-json/posts?filter[orderby]=rand&filter[posts_per_page]=1```
-n 10 means 10 connections
-c 10 means 10 concurrent connections
Please note that url has to be after all parameters

Please note that :
Complete requests:      10
Failed requests:        9
   (Connect: 0, Receive: 0, Length: 9, Exceptions: 0)

That means only that responses length were different not necessarily that request failed 

to test POST endpoint 

```ab -n 25 -c 25 -C “cookie_id_1=70269997;cookie_id_2=ee7776cb-5127-477f-ae5c-5188e3527fde;cookie_id_3=92e24a8c-2190-4097-a36c-fb3dacc9e731” -p data.json -v 2 -T "application/json" https://your.api.com/api/feed/me/with/json```
