# Introduction

## What can go wrong ?

> Everything. 
> -- said the developer

### Let the music play

Accessing a website triggers plenty of activity on plenty of components/server

```sequence {theme="simple"}
Title: Let the request flow
Browser->DNS: whois example.com 
Browser->Load balancer: get http://example.com
Load balancer->Browser: moved to https://example.com
Load balancer->App server: http request
App server--> Database: sql
App server--> Database: sql
Browser->Load balancer: get https://example.com/style.css
Load balancer->App server: http request 
Browser->Load balancer: get https://example.com/bundle.js
Load balancer->App server: http request
Load balancer -> Browser:
Browser->Load balancer: get https://example.com/extra.json
Load balancer->App server: http request
App server--> Database: sql
App server--> Database: sql
App server-> Load balancer:
Load balancer -> Browser:

Browser->Load balancer: post https://example.com/trigger {somejson}
Load balancer->App server: http post
App server-> Database: sql
App server-> Redis: enqueue background job
App server-> Database: sql
App server-> Load balancer:
Load balancer -> Browser:
Worker-> Redis: get next job
Worker-> Database: sql
Worker-> External Service: scrapping or external api
Worker-> External Service: mail

```

### The Bad news
> Every arrow can fail, take hours to complete, never complete,... 
> Every box can mis-behave, welcome to the internet.

### The Good news

You are reading this book
the skills :
  - awareness : most developers don't know the complete stack, never observe their application failing, learn from your mistakes 
  - evidence collection : get the logs, traces with the correct tool

