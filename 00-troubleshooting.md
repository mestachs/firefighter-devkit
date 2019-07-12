
# Troubleshooting

Ok it's monday morning, you are super happy, you are at work early, and the first at work !
The slack channel for monitoring is full of messages saying servers are down, returning errors,...
 Take a deep breath, let your heart calm down. Think twice before your intervention.


## Keep calm

@import "assets/keepcalm.png" {width="400px" height="400px" style="display: block; margin-left: auto;  margin-right: auto; marfin-down: 10px" title="keep calm and don't restart your computer" alt="keep calm and don't restart your computer"}

* Keep calm, you won’t be fired
   - Even if you think you might be responsible, they should reflect on how you ended up messing up with their production.
* Don’t just restart the server, find the root cause.
   - except if you know that something really wrong is going on and it's better to unplug everything : no service is sometime better than wrong/bad service.
* If after 30m/1h you don’t have a clue than may be restarting can be a way of getting back to business. 

> I know this doesn't sound webscale^tm^, but you are not google.


## Communicate & follow procedures

> Let's assume your company is not the perfect work environment, especially at hosting stuff. Some developers knows a bit but you don't really have a procedure in place to handle such incidents.

1. Communicate to users and manager, service desks : 
   - setup dedicated slack channel #warroom
   - if you have status page, twitter account let's inform users that you have troubles and that your are investigating.
2. Apologize and commit to keep them updated
   - As a service provider, even if it’s not your fault understand that you block their business
   - Keep them informed of the progress or non progress : nothing is more fustrating that no news.
3. Create a jira/github/... issue
4. Follow the procedure (if you have one)
   - Notify your manager, 
   - assign an "incident lead" to handle the communication
   - find "experts" to help you test/confirm your analysis
5. Comment as you find information in the jira or slack channel
   - What is working
   - What is not working, when it last time reported working,…
   - What was modified lately (latest code modified, deployment, infra, …)
   - Paste commands/logs/screenshots that looks relevent
6. Try to find and document a workaround if applicable 
   - eg : don't search by name, but by inventory code
7. Don't forget to update regularly status/twitter account/...


## Reproduce - Diagnose - Fix - Reflect

**Clarify and collect informations**

 - Where : 
   - staging, production,... 
   - client, server, db,...
   - which browser
   - which mobile device
 - What :
   - What is the symptom ? Error, missing data, bad data, empty screen, never ending spinner, browser/app freezing,…
 - How :
    - if only a part of the site is non functional, try document how to reproduce the issue
    - go to that page, fill in with this content, click search... boom
 - Why :
    - if you have access to users, try understand the context, why the user was doing what he was doing when incident started.
    - Every beginning of the month, I'm supposed to send the report to accounting. I was trying to generate the monthly report but then everything went dark    
 - Evidences, collect all you can    
   - Stacktrace, error message-codes
   - Thread dump, heap dump, core dump, tcp dump...
   - Logs (application, web server access logs)
   - Intermittent issue : it works but one request out of two is failing
   - Developer console errors in IE/Chrome/Firefox
   - Which queries are running on the database

> Automate evidence collections : 
    - log forwarded to aggregator, 
    - sentry alike service, 
    - shell script to run jstack, collect db queries,
    - APM,
    ...

**Try to locate the problem**

* Use a [dichotomic search](https://en.wikipedia.org/wiki/Dichotomic_search) : narrow the problem in 2 points of the stack, of the code.
* Use heuristic « gut feeling », perhaps based on latest incidents knowledge
* Use brute force in last resort

**Try to relate to a previous issue or a change in your code or infrastructure**

* a nightly unattended os update ?
* a new feature added to the product ? 
* a new firewall rule added ?
* ...
* a new user behavior
* a user is abusing the system (github.com : commit message holding a dvd iso)
* marketing just sent user "Deals" or black friday ?


**Make assumption and verify you have a safetynet, change one thing at a time**

* make sure you have backups (db, config files,...)
* once you have mental model of the issue, test you hypothesis : with "readonly" commands
* avoid additional incidents, only one person change one thing at a time.
* if something needs to be changed, do one thing at a time to be sure on what was the corrective action.
* keep logs of commands and outputs for postmortem


**Update the documentation / FAQ, Improve our tests** 

Document and reflect on the issue

- Do we have already an entry in our FAQ (or start one) ?
- Did you find something via google/stackoverflow ?
- Can we have similar problem in another service/app, in another team ?
- What can we improve to prevent this ?
- Do a post mortem on what to improve in your procedure

## More on the subject

- http://www.catb.org/esr/faqs/smart-questions.html 
- https://www.amazon.com/DevOps-Troubleshooting-Linux-Server-Practices/dp/0321832043 
- https://pragprog.com/book/pbdp/debug-it 
- https://jvns.ca/blog/2019/06/23/a-few-debugging-resources/
- https://twitter.com/b0rk/status/1146159551315677189
- https://twitter.com/b0rk/status/1145350304583622656
- https://twitter.com/b0rk/status/1146149101995778054
- https://twitter.com/b0rk/status/1148046227621199873
- https://twitter.com/b0rk/status/1144352412557283328
- https://twitter.com/b0rk/status/1148037549308481537
- https://twitter.com/b0rk/status/1147267035275104256
- https://twitter.com/b0rk/status/1147261917918109696
- https://twitter.com/b0rk/status/1146431464701124608