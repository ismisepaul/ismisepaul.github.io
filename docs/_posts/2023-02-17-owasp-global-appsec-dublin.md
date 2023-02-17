---
layout: post
title:  "OWASP AppSec Dublin 2023"
date:   2023-02-17 15:19:12 +0000
categories: conference appsec prodsec
tag: owasp appsec prodsec threat-modelling
---

This week I attended [OWASP Global AppSec Dublin 2023](https://dublin.globalappsec.org/) and it was good to be
back at an OWASP event and connect with some familiar faces but also great to see new faces too. I think the overall 
attendance was around the 500, and it was really well run - we were well fed and watered! It was also great to see all 
four keynotes delivered by woman. However, I'm still not sure about the rebrand announcement. I get you got to keep 
the wasp but Open _Worldwide_ Application Security Project doesn't seem quite right...


It's always unfortunate when there's a clash of talks that you're really interested in, but you've got to pick one. The 
morning sessions especially suffered from this and can't wait to see the videos go up to catch what I missed. 

Over the two conference days I attended the following talks 

### Day 1

- A Taste of Privacy Threat Modeling (Kim Wuyts)
- Far from green fields - introducing Threat modelling to established teams (Sarah-Jane Madden)
- Ten DevSecOps Culture Failures (Chris Romeo)
- Why winning the war in cybersecurity means winning more of the everyday battles (Jessica Robinson)
- Bootstrap and increase your software assurance with OWASP SAMM v2.1 (Sebastien Deleersnyder & Bart De Win)
- Server Side Prototype Pollution (Gareth Heyes)

### Day 2 

- Shifting Security Everywhere (Tanya Janca)
- OWASP Serverless Top 10 (Tal Melamed)
- AI-Assisted Coding: The Future of Software Development; between Challenges and Benefits (Dr. Magda Chelly)
- Developer Driven Security in high-growth environments (Jakub Kaluzny)
- Get On With The Program: Threat Modeling In and For Your Organization (Izar Tarandach)

If there was an overall theme I think it'd be _Threat Modelling_ and I was happy to see a lot of talks on the topic as
I'm interested in how different people perform threat modelling, how to scale it and what tools people use. 

Before delving into each talk here a list of interesting / useful tools mentioned within each of the talks (also 
scattered thoughout this post)

- [LINDDUN](https://www.linddun.org/) - what STRIDE is for security, LINDDUN is for privacy threat modelling
- [Adam Shostack's 4 Questions](https://github.com/adamshostack/4QuestionFrame) - the four questions you should be 
asking yourself when you threat model
- [Threat Modeling Manifesto](https://www.threatmodelingmanifesto.org/) - sharing distilled collective knowledge
- [OWASP Threat Dragon](https://owasp.org/www-project-threat-dragon/) - a tool to threat model


### A Taste of Privacy Threat Modeling (Kim Wuyts)

Kim kicked off the conference with the first of many threat modelling talks but this one had a twist where it was about
privacy rather than security and highlighted the use of [LINDDUN](https://www.linddun.org/). I like when people bring
real life threat modelling into their talks and Kim didn't disappoint. She used the analogy of her kid going into an 
ice-cream shop and wanting pretty much everything and settling on a cone with three scoops. As a parent you do a threat 
model and think - the top scoops is going to fall off, the middle scoop the child doesn't even like that flavour, the 
bottom scoop is going to melt and you're going to have to eat it and the child is simply going to take a big bite out 
of the bottom of the cone! Even with this real life example you can utilise the [4 questions](https://github.com/adamshostack/4QuestionFrame) (
modifyied slightly) from Adam Shostack

1. What's going on 
2. What can go wrong 
3. What to do about it 
4. Wait a minute!

Kim made the argument why privacy matters with many examples like those from [Strava](https://www.theguardian.com/world/2018/jan/28/fitness-tracking-app-gives-away-location-of-secret-us-army-bases), 
[Roomba](https://www.technologyreview.com/2022/12/19/1065306/roomba-irobot-robot-vacuums-artificial-intelligence-training-data-privacy/)
and with the restrictions around abortion in the US the ability to figure out if [someone is pregnant](https://www.forbes.com/sites/kashmirhill/2012/02/16/how-target-figured-out-a-teen-girl-was-pregnant-before-her-father-did/).

Like security, privacy threat modelling should be done as early as possible with the following and used an example of a 
doll that was [banned in Germany](https://www.reuters.com/article/us-germany-cyber-dolls-idUSKBN15W20Q) to perform a 
privacy threat model which was fun and showed it all starts with a diagram and thinking about the flow of data. The 
process is (think of the 4 questions)

1. Model the system - create a DFD / whiteboard
2. Elicit Threats - map model components and identify threats
3. Mitigate threats - assess & prioritize then mitigate
4. Reflect - reflect and repeat

Kim argued that privacy and security don't have to be at logger heads but can work together e.g. for security we want
logs and very detailed logs so we can figure out what happened. For privacy we're worried about what we're collecting
from those logs and does it violate privacy. 

Security - protecting data, company assets, (external) attackers
Privacy - protecting personal data, data subject assets, attacker + (internal) misbehaviour

In closing, she said to do threat modelling early ideally the sooner the better, but it's never too late and that the 
outcome of threat modelling should be meaningful where it has actual value to stakeholders. 


### Far from green fields - introducing Threat modelling to established teams (Sarah-Jane Madden)

Sarah-Jane's talk was more focused on the challenges of introducing Threat Modelling to well established software teams
where the organisation comprises of legacy applications, new application as well as acquisitions. She made a great 
opening remark that _Cyber is about hearts and minds as much as its about tech_. After studying the documentation on
threat modelling she wondered - Why can't we just *do* threat modelling!! and why not shift everything so far left 
we don't have to worry about pentesting etc. As with a lot of things the theory soon gives way to reality. She went on 
to talk about the various mistakes that were made over the course of rolling out threat modelling _from the trenches_.

Her first

