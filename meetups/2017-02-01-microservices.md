
# Microservices - Adrian Cockcroft

## Intro

Formerly Netflix, then Battery Ventures, now @ AWS working on "cloud architecture strategy".

- Slide decks from previous presentations on [github.com/adrianco](https://github.com/adrianco)
- Setting up open source program

In the last 10 years we've seen a lot of change

metric|then|now|change
---|---|---|---
CPU performance | | | increased modestly
storage | spining rust | SSD | much faster
network | 1Gb/s | core network at AWS is 25Gb/s | 25x
protocols | Soap/XML | AVRO, gRPC, Thrift, JSON | 10 to 100x faster
architecture | monolith | microservices and functions (lambda) |
utilization | 5-10% monolith with handcrafted snowflake | highly utilized containers provisionined on demand
deployment | handcrafted snowflakes | container based | 

Netflix used to release every two weeks at 2am on the Thursday and bounce the db.  It worked - sometimes.

## Questions

### What does an all lambda architecture look like?
- Things like...
	- CQRS,
	- make all requests idempotent, 
	- Uber - double deliver: removes the long tail in performance
	- all data stores as append only logs
- Pat Helland: [memories guesses and apologies](https://blogs.msdn.microsoft.com/pathelland/2007/05/15/memories-guesses-and-apologies/) ([video](https://channel9.msdn.com/Shows/ARCast.TV/ARCastTV-Pat-Helland-on-Memories-Guesses-and-Apologies)),
- error handling code is where a lot of bugs are but you only find out about them when stuff goes wrong
- HPTS - high performance transaction systems

### Why AWS?
- really good ability to execute at high rate
- everything they are doing is driven by customers who needed it
- how many years ahead of competition they are is increasing (he claims)

### Antipatterns in microservices and lambda
- versioning - having only one version of a service in production.  Intuitively makes things simple, but creates hidden complexity.  Need to create version aware routing (yes!). Always synchronously at any time introduce new versions, take away old ones when no one is talking to them.
- Netflix had a once a quarter "break the build day" where old versions would be removed and the build would break forcing everyone to upgrade

### How to exploit fast pace of progress without breaking?
- build safety into systems so you have a certain amount of margin before people "die"
- feature flags per customer if you have enterprise customers

### How to turn off mainframes and bring microservices to finance?
- SSN issuing system is 56 years old, written in mainframe assembly code
- lots of bits of Cobol that read and write files - like lambda functions on S3
- state of the art *is* public cloud, can't actually build it yourself
- eventually will get to the point where you won't be able to pass audit unless you're in the cloud because it allows for cryptographically secure log of all changes so you can prove something is secure and uncompromized

### How quickly will lambda be adopted - why would cloud providers do that?
- the cheaper something gets the more someone do of it
- AWS compensates sales reps to reduce customer bill because it make more load be moved to the cloud
- so if lambda is 1/10th the cost then you'll get more than 10x the workload which means more money

### Local dev gets more difficult as everything gets moved to the cloud
- some things have dummy clients for local dev
- has "lambda everywhere" post
- 1st wave was to add triggers to existing AWS features like S3
- 2nd wave was for infrastructure instances
- 3rd wave is lambda at the edge - triggered by CDN hits and misses, 
- [AWS snowball edge](https://aws.amazon.com/snowball-edge/) instance that can be shipped as a 50lb box

### What about immutable infrastructure?
- Heroku and cloud foundry are like straightjackets that provide constraints, good if you want to build lots of copies of the same thing
- For things like single global services like Netflix you want something that can evolve

### How do we do stress testing, not just ensure compliance?
- There's a lot of systems that don't believe they will be attacked but are under attack, or will be under attack
- UK government is focusing on ability to respond vs. ability to prevent because you can't legislate against failure so detect and respond quickly.  Take down for fake emails used to take days and now can be done in minutes.  Makes spoof emails unprofitable and attackers go away.  Make your system harder than others (unless you have specific assets that are unique to you).
- (from the audience) you can create a clone that can be stress tested 
- You can run experiments at scale which don't cost much so long as you shut them down quickly, allows for full scale testing
- if you can prevent and detect before things go bad then your MTR can go negative
- planning departments and building inspectors save more lives than firemen, they are the real heroes but unpopular
- prevention leaves you with only the interesting Byzantine problems to solve
- need to find a way to claim credit for such savings so people don't eliminate them
- you need a Red Team to attack if you want security, Chaos Monkey type system - only way to assure you have a margin for security

### How does the world of DevOps and SRE look with lambda especially since most companies don't have an amazing team like NetFlix?
- companies that get out of the way of the talent and let them performcan all have engineers like Netflix - it's not that Netflix has special engineers
- Google SRE book is a good read. [online for free](https://landing.google.com/sre/book.html)
- Netflix model for SRE - has an SRE team, develop tooling that measure availability - successful starts not uptime minutes, and tooling to measure what is going wrong and tooling to route errors to devs.  If devs are on call they write much better code :-) 
- Google model is an ownership model - when things get mature something is run by operations for you.  
- Google model can lead to apps that stop evolving (like Blogger). Netflix model is more agile friendly as devs never lose ownership.


> Written with [StackEdit](https://stackedit.io/).
