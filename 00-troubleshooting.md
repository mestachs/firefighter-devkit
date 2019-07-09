
# Troubleshooting

## Keep calm

@import "assets/keepcalm.png" {width="300px" height="300px" title="keep calm and don't restart your computer" alt="keep calm and don't restart your computer"}

* Keep calm, you won’t be fired
   - They should reflect on how you ended up messing up with their production.
* Don’t just restart the server, find the root cause.
   - except if you know that something really wrong is going on and it's better to unplug everything : no service is sometime better than wrong/bad service.
* If after 30m/1h you don’t have a clue than may be restarting can be a way of getting back to business. 

> I know this doesn't sound webscale^tm^, but you are not google.


## Document & follow procedures

- Communicate to users and management, service desks : 
   - dedicated slack channel
   - status page, twitter account
- Apologize and commit to keep them updated
   - As a service provider, even if it’s not your fault understand that you block their business
   - Keep them informed of the progress or non progress : nothing is more fustrating that no news.
- Create a jira/github/... issue
- Notify your manager, assign a "incident lead"
- Comment as you find information 
   - What is working
   - What is not working, when it last time reported working,…
   - What was modified lately (lastest code modified, deployment, infra, …)
- Try to find and document a workaround if applicable 
   - eg : don't search by name, but by inventory code


## Reproduce - Diagnose - Fix - Reflect

**Clarify and collect informations**

 - Where : 
   - staging, production,... 
   - client, server, db,...
   - which browser
   - which mobile device
 - What :
   - Error, missing data, bad data, empty screen,…
 - How :
    - go to that page, fill in with this content, ... 
 - Why :
    - Every beginning of the month, I'm supposed to send the report to accounting. I was trying to generate the monthly report but then everything went dark    
 - Evidences
   - Stacktrace, error message
   - thread dump, heap dump,...
   - Logs (application, web server access logs)
   - Intermittent issue : it works but one request out of two is failing
   - Developper console errors in IE/Chrome/Firefox
   - Which query are running in prod

> Automate evidence collections : 
    - log forwarded to aggregator, 
    - sentry alike service, 
    - shell script to run jstack, collect db queries,
    - APM,
    ...

**Try to locate the problem**

* Use a dichotomic algorithm
* Use heuristic « gut feeling »
* Use brute force in last ressort

**Try to relate to a previous issue or a change in your code/infrastructure**

* a new feature added to the product ? 
* a new firewall rule added ?
* ...
* a new user behavior
* a user is abusing the system (github.com : commit message holding a dvd iso)
* marketing just sent user "Deals" or black friday ?

**Update the documentation / FAQ, Improve our tests** 

Document and reflect on the issue

- Do we have already an entry in our FAQ (or start one) ?
- Did you find something via google/stackoverflow ?
- Can we have similar problem in another service/app, in another team ?
- What can we improve to prevent this ?




## More on the subject

- http://www.catb.org/esr/faqs/smart-questions.html 
- https://www.amazon.com/DevOps-Troubleshooting-Linux-Server-Practices/dp/0321832043 
- https://pragprog.com/book/pbdp/debug-it 
- https://twitter.com/b0rk/status/1146159551315677189
- https://twitter.com/b0rk/status/1145350304583622656
- https://twitter.com/b0rk/status/1146149101995778054
- https://twitter.com/b0rk/status/1148046227621199873
- https://twitter.com/b0rk/status/1144352412557283328
- https://twitter.com/b0rk/status/1148037549308481537
- https://twitter.com/b0rk/status/1147267035275104256
- https://twitter.com/b0rk/status/1147261917918109696
- https://twitter.com/b0rk/status/1146431464701124608