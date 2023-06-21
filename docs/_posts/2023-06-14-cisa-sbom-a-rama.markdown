---
layout: post
title:  "CISA SBOM-a-rama 2023"
date:   2023-06-14 23:30:12 +0000
categories: software-supply-chain-security
tag: cisa sbom
---
Sharing some highlights and notes from CISA's [SBOM-a-rama](https://www.cisa.gov/news-events/events/sbom-rama) I 
virtually attended. Talks categorized into 3 - Governments & Industry, CISA Working Groups, and Discussion


## Overview

* Schedule [https://www.cisa.gov/news-events/events/sbom-rama](https://www.cisa.gov/news-events/events/sbom-rama)
* Talks from industry, governments and CISA SBOM working groups with a discussion section
* They’ll be setting up an asynchronous way of working through a mailing list (coming soon)
* Sharing the slides is going to take some time after the event

## Highlights
* Interesting use case from the financial sector where they share SBOMs internally so when a team imports an internal tool they have full visibility into the 3rd party components
* It's more than the US government moving towards asking vendors for SBOMs - EU (27 countries) and Japan presented
* There are a number of VEX implementations (considering standards Elastic Security Statements)
  * [CSAF 2.0 Profile 5: VEX](https://docs.oasis-open.org/csaf/csaf/v2.0/os/csaf-v2.0-os.html#45-profile-5-vex)
  * [CycloneDX](https://cyclonedx.org/capabilities/vex/)
  * [OpenVEX](https://github.com/openvex/spec)
  * SPDX 3.0 Release Candidate [https://spdx.dev/news/](https://spdx.dev/news/)
* The state of SBOM is still in its infancy but is moving very fast with more attention being drawn to them from government demands etc.
* Folks are thinking beyond SBOM to cover Cloud, AI, Quantum etc. suggestion is to rename SBOM as it doesn’t capture where the industry is heading
* Folks are talking about the end of CVSS with
  * [Known Exploited Vulnerabilities Catalog](https://www.cisa.gov/known-exploited-vulnerabilities-catalog) KVE
  * Exploit Prediction Scoring System (EPSS) [https://www.first.org/epss/](https://www.first.org/epss/)
  * Stakeholder-Specific Vulnerability Categorization - SSVC
* Most new folks join the On Ramps & Adoption Working group
* They have an SBOM calendar [https://calendar.google.com/calendar/u/0/embed?src=hqnbr3lk8ngjgv04g6ir5d5duc@group.calendar.google.com](https://www.first.org/epss/)


## Governments & Industry

### EU Cyber Resilience Act

[https://digital-strategy.ec.europa.eu/en/library/cyber-resilience-act](https://digital-strategy.ec.europa.eu/en/library/cyber-resilience-act)

* Set up to decrease the amount of vulnerabilities in hardware and software.
* If you want to place hardware and software within the 27 member states there are obligations for manufactures, distributors and importers to adhere to cybersecurity best practices.
* In its current form the security of a product must be maintained for 5 years.
* This is set to include everything from laptop to network equipment, CPUs, OSs, games. With OSS out of scope, SaaS (covered by NIS2) and others already covered such as cars, medical devices etc.
* At the very least manufacturers need to draw up an SBOM with top-level dependencies, no requirement to make it publically available.
* This proposal was made in September 2022 with them looking to put it into enforcement in 2024. 

![](../assets/images/2023-06-cisa-sbom/2023-06-eu-timeline-cyber-reslience-act.png)


### Citi

* Evaluating SBOM technologies amongst other capabilities, still early in terms of SBOM adoption
* Building capabilities to manage and review SOM's as part of wider strategy
* Threat based approach firstly done
* What details are expected from a vendor and the minimum fields required
* How to define exploitability and what level of status from a VeX document
* As an extension of vulnerability management financial institutions are using SBOMs to share them internally and focusing on internal pipelines
* Gain understanding of where and what open-source software is deployed, building open-source inventory
* Ensure that any SBOM's created in the development environment correlate with SBOM's generated later in pipeline


Two main issues identified so far…
* SBOM quality
  * Inconsistent between vendors
  * Lacking quality benchmarks
  * What does good actually look like?
  * There’s no common approach - what is the depth - some are just touching the surface
* SBOM naming problem
  * Difficult to identify a library
  * Forked projects only partially match libraries
  * More work is required but a lot of financial groups are involved


### New York-Presbyterian Hospital

![](../assets/images/2023-06-cisa-sbom/2023-06-work-in-progress.png)

* SBOM are in a state like the early days of the internet you’d go to a site and there’d be a gif with a guy digging
* Spent 6 months negotiating an NDA to get an SBOM because it was so controversial a few years back - they were claiming this is IP
* They’re at the crawl stage
* They created an OSS tool to help consume SBOMs [https://github.com/nyph-infosec/daggerboard](https://github.com/nyph-infosec/daggerboard)
* The team is creating guidance on "When to Publish a VEX Statement" o The guidance will build upon the guidance document under development by the CISA VEX Working Group
* The team is collaborating with the NYP team to determine how to use Daggerboard for this iteration
* The VEX content generation will begin once the plans for using Daggerboard are finalized


Takeaways

* Build trust:
  * collaboration mindset
  * transparency
* Incremental sophistication crawl, walk, run
* Working with 3rd party vendors grow the ecosystem

![](../assets/images/2023-06-cisa-sbom/2023-06-sbom-healthcase-poc.png)


### SBOMs in the Automotive Industry - Auto-ISAC SBOM

![](../assets/images/2023-06-cisa-sbom/2023-06-automotive-supply-chain.png)

3 Phase Approach

Phase 1

* What info is needed on an SBOM to provide analysis, sharing guidance, and security?
* What info is shared with consumers of the component?
* How are components classified in an SBOM?
* How are components identified, e.g. version, branch, fragment, supplier/author?
* What is the balance between transparency vs. liability?
* How can IP be protected in a transparent BOM?
* Should a BOM enumerate all variations?
* Who gets the SBOM and by what means?
* How can subcomponents of large libraries be distinguished from general use of the library?
* How will AutolSAC interact with and influence other SBOM projects?
* How will components be identified, tracked, and audited by the consumer of the component?
* How will software engineering and QA teams provide SBOMs?
* How will purchasing agents enforce SOM best practice and block restricted components?


Phase 2

* Unified supplier voice on SBOM adoption to OEMs
* Align with NTIA
* Practical approach with input fromOEMs
* Information Report published in 2021


Phase 3

* Will be coming out with a public statement (keeping it within the automotive industry for now)
* They believe confidentially is the heart of this
* They won’t be sharing public SBOMs
* They have created, named and transmitted SBOMs
* They will be sharing some tools they used

### Japanese METI Ministry of Economy, Trade and Industry (paused recording)

Trialling rollout across difference sectors - automotive, software and medical

![](../assets/images/2023-06-cisa-sbom/2023-06-sbom-myths-vs-facts.png)

## CISA Working Groups

### VEX Working Group

Led by: Art Manion

* Grew out of SBOM at NTIA and CISA
* One vulnerability, one status, one or more components
* “Not affected” should have justifications
  * Component not present
    * The vulnerable subcomponent is not included in the product.
  * Vulnerable code not present
    * The vulnerable subcomponent is included in the product but the vulnerable code is not present.
  * Vulnerable code not in execute path
    * The vulnerable code (likely in the subcomponent) cannot be executed due to the way it is used by the product.
  * Vulnerable code cannot be controlled by adversary
    * The vulnerable code is present and used by the product but cannot be controlled by an attacker to exploit the vulnerability.
  * Inline mitigations already exist
    * The product includes built-in protections or features that prevent exploitation of the vulnerability. These built-in protections cannot be subverted by the attacker and cannot be configured or disabled by the user.
* These mitigations completely prevent exploitation based on known attack vectors.

Visualizing a VEX document

![](../assets/images/2023-06-cisa-sbom/2023-06-vex-document-whats-included.png)


VEX implementations
* [CSAF 2.0 Profile 5: VEX](https://docs.oasis-open.org/csaf/csaf/v2.0/os/csaf-v2.0-os.html#45-profile-5-vex)
* [CycloneDX](https://cyclonedx.org/capabilities/vex/)
* [OpenVEX](https://github.com/openvex/spec)
* SPDX 3.0 Release Candidate [https://spdx.dev/news/](https://spdx.dev/news/)

### SBOM Sharing

* "Facilitating the exchange/sharing of SBOMs from legitimate sources to interested parties in ways that are fit for associated purposes."
* How to do
  * One-to-one SBOM Sharing
  * Multi-party SBOM Sharing
* Using SBOMs as a purchasing tool
* If you create content tag it with #cisasbomsharing
* Problems
  * How to share and ensure that it hasn’t been tampered with
  * How do you maintain SBOMs - 6month old SBOM is irrelevant
  * We need a central common place to store SBOMs
  * Contract language will mean a lot - that's when vendors will take this seriously

![](../assets/images/2023-06-cisa-sbom/2023-06-utility-managing-security-fix.png)

### SBOM for the Cloud

* Tracking software dependencies in SAAS
* Service Transparency
  * Its APIs too
* Avoiding using SBOM as the name - its more observability
* The group needs more expertise
* Enable full stack transparency
* Provide a comprehensive overview of SBOM for SaaS hosted on the Cloud and On-prem for identifying vulnerable components/libraries, risk factors, and fortifying the software ecosystem against potential threats.
* SaaS products
  * Hosted on-cloud (public internet)
  * Private environment for clients
  * Traditional Enterprise (self hosted and managed)
* Mission
  * Contribute to the development of an industry standard for the implementation of SBOM for the cloud
  * Develop a solution that can be applied to all types of cloud deployment models including public, private and hybrid as well as cloud services including but not limited to Infrastructure As A Service (laaS), Platform as a Service (PaaS) and Software as a Service (SaaS)
  * A global standard that can be adopted by any industry and by consumers and cloud service providers (SP) alike
  * Brand agnostic coverage of the full technology stack from the application tier to the bare metal tier

### SBOM Quality Team

* SOM Field Definitions - building up pragmatic guidance
* Format adherence (syntax)
* What is filled - (semantics)
* Pragmatic Practices - for field contents.
* Next Steps
  * Publish of pragmatic practices for minimum fields
* Working on SBOM Types - [https://www.cisa.gov/sites/default/files/2023-04/sbom-types-document-508c.pdf](https://www.cisa.gov/sites/default/files/2023-04/sbom-types-document-508c.pdf)
* Survey results (what folks want next)
  * SBOM quality
  * SBOM inventory detection types
  * Tooling that provides protection and accessibility of SOM data
  * Enterprise deployment of SOM tools at scale (creation, collection, and dissemination)
  * Data management tooling
  * Versioning: Versions of implementation formats


Looking at the table below 

* Red people disagreed on definition 
* Yellow some confusion 
* Green overall consensus


![](../assets/images/2023-06-cisa-sbom/2023-06-sbom-common-understanding.png)

![](../assets/images/2023-06-cisa-sbom/2023-06-sbom-beyond.png)

![](../assets/images/2023-06-cisa-sbom/2023-06-sbom-depth.png)


### On Ramps & Adoption

* This is the group that most folks initially join
* Recommended looking at Log4Shell Response Patterns & Learnings from Them [https://www.infoq.com/presentations/log4shell-security-response-patterns/](https://www.infoq.com/presentations/log4shell-security-response-patterns/)
* Why companies would rather not produce an SBOM
  * License violations
  * "Unfixable" issues
  * Ongoing scrutiny / accountability
  * $Other
* Maybe tolerate some form of redaction

The end of CVSS - other standards are emerging as better measures of security
* Known Exploited Vulnerabilities Catalog KVE
* Exploit Prediction Scoring System (EPSS) [https://www.first.org/epss/](https://www.first.org/epss/)
* Stakeholder-Specific Vulnerability Categorization - SSVC

![](../assets/images/2023-06-cisa-sbom/2023-06-fda-use-case.png)

## Discussion


There was a discussion portion that lasted just over an hour here are the highlights (of what I caught)


* What are the repercussions if we don’t comply? (no real answer to this)
* Not addressing active content - AV software there a stream of active content - there is a runtime SBOM coming
* HBOM - hardware bom - unfortunate name
* SBOM is a solution looking for a problem - we need to convenience these people
* Before log4shell - 8am - by 10am all products have been identified as having log4j and by 10 days all products where fixed
* We’re not including the big tech companies in the working groups
* Create case studies / wins so I can say they’re in the same industry, and I could use what they’re doing
* If this is just an SBOM solution it’ll fall flat on its face, this summer they’re issuing a white paper with potential directions, the naming problem - what we call things - its got to scale and go across all use cases
* Need to think about AI and SBOMs for models
* Even if you’re not willing to share SBOM, as a provider they should be able to attest that they are doing SCA, SAST, DAST etc.
* Move away from the word SBOM - in cloud world its observability - it's a type of introspection - the word SBOM is a victim of its own success - the value is observability - you being able to observe the building of your software
* Once you build the rails from left to right of metadata - maybe push SSDF attestation, threat data, VEX documents and have a supply chain control plane
* Moving away from CVSS to …
* Tools vendors are not ingesting yet - how asset management tools ingest these - if we had a redaction method the tools will move forward
* Think about quantum
* Setting up asynchronous communication - mailing lists will be set up
