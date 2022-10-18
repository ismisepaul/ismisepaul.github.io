---
layout: post
title:  "SupplyChainSecurityCon @ Open Source Summit Europe 2022"
date:   2022-09-23 18:31:05 +0000
categories: supply-chain
tag: conferences sbom vex supply-chain slsa provenance attestation
---
Last week I attended the Linux Foundation's [Open Source Summit Europe](https://events.linuxfoundation.org/open-source-summit-europe/) here in my hometown of Dublin where I mainly camped out at the [SupplyChainSecurityCon](https://events.linuxfoundation.org/open-source-summit-europe/about/supplychainsecuritycon/) event. The talks focused on SBOMs, SLSA, VEX, provenance, attestation and signing. He's what I picked up at the event over the 4 days. 


![image]({{site.baseurl}}/assets/images/2022-09_convention-centre.jpg)


I think everyone had a slide with [this XKCD](https://xkcd.com/2347) to drill home the problem the industry is facing.

## The Keynotes
There were lots of great announcements at the keynotes like Europe setting up its own chapter of the Linux Foundation, Meta handing over PyTorch to the Linux Foundation, lots of great work around sustainability and Linus pushing to merge Rust into the Linux Kernel. Among the keynotes were two security focused talks from Christopher Robinson aka CRob and Paul Vixie.


![image]({{site.baseurl}}/assets/images/2022-09_linus_opensourcesummit_europe.jpg)


Christopher’s talk “The Future of Open Source is Trust” focused on as open source software has evolved so has the threat landscape and highlighted the the great work being done by the OpenSSF which has the following initiatives

- Best Practices for Open Source Developers
- Securing Critical Projects
- Supply Chain Integrity 
- Identifying Security Threats in open source projects
- Security Tooling
- Vulnerability disclosures

Paul Vixie’s talk was a great history lesson entitled “ Revisiting Total Cost of Ownership for External Dependencies” where he spoke about how a patch was distributed which contained DNS back in the 80s by posting it to the Usenet and retrieved by an FTP server. People would just download the patch which eventually became abandonware “Then everything got big!” Anyone with a network device needed DNS. People just copied the code and made their own changes divorced from the upstream and all embedded systems are running a fork of a fork of a fork. Paul admits to writing bugs in DNS but also addressing them in the 1990s. “It’d be nice to have an internet when we were building one”. His key takeaways for today's problems is that dependencies don’t just go one deep, orphaned dependencies are a huge cost and people should be monitoring their supply chains. We’re all in this together so let's organize ourselves better where he’s working with the OpenSSF to build tools to make traceability simpler. He reckons once people start sharing SBOMs that'll take a “decent whack” against the problem and shirk it by half. But we currently we don’t have a plan…

## SupplyChainSecurityCon

The problem with supply chains was almost always communicated through the SLSA diagram below, with the incentive to do something about the problem coming from the EO14028, and the solution being implementing SLSA and producing SBOMs. The SBOM analogy everyone reached for was it's basically the ingredients label used to communicate what is contained in the food you’re buying - with VEX being the allergy advice. 

![image]({{site.baseurl}}/assets/images/2022-09_slsa_supply_chain.png)


### Terminology 

The following terms were spoken about a lot so its handy to have some definitions

SLSA - Supply chain Levels for Software Artifacts (SLSA) “salsa”. It’s a security framework, a check-list of standards and controls to prevent tampering, improve integrity, and secure packages and infrastructure in your projects, businesses or enterprises. It’s how you get from safe enough to being as resilient as possible, at any link in the chain.

SBOM - Software Bill of Materials is a nested inventory, a list of ingredients that make up software components.

VEX - Vulnerability Exploitability eXchange is a form of a security advisory with the goal allowing a software supplier or other parties to assert the status of specific vulnerabilities in a particular product. Allow both suppliers and users to focus on vulnerabilities that pose the most immediate risk, while not investing time in searching for or patching vulnerabilities that are not exploitable and therefore have no impact.


![image]({{site.baseurl}}/assets/images/2022-09_ingredients.png)


[Provenance](https://www.tiktok.com/@chainguard_dev/video/7133203786927050027?is_from_webapp=v1&item_id=7133203786927050027) - is metadata about how an artifact was built, including the build process, top-level source, and dependencies.

Attestation - is an authenticated statement (metadata) about a software artifact or collection of software artifacts. A key feature of SLSA Level 2, which requires organizations to protect against software tampering and add minimal build integrity guarantees. It is also found in the NIST Secure Software Development Framework and ISACA’s Certified Information Security Auditor training.

### What’s in a Name? Vulnerabilities, SBOMs, and the Challenge of Software Identity - Justin Murphy, Department of Homeland Security (DHS), Cybersecurity & Infrastructure Security Agency (CISA)

As this was a talk from DHS the room was packed. Justin spoke about the challenge of software identity and about the problem of discovering if you’re at risk when using a 3rd party where he highlighted four ways of achieving this and the problem with each one
1) Security Advisories - scale, vendor hasn’t issued one, information needed is not mentioned
2) Ask the maintainer - will you get a response
3) Own investigation - time consuming, expensive, where to start?
4) SBOM - Do you have it, does it have enough information, are there tools available to process it

Justin said that at a minimum vendors should be declaring their top level dependencies. He spoke about how it took Executive Order 14028 to get organizations to take this seriously. One way they’re tackling this is using a machine-readable way of issuing security advisories with [CSAF](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=csaf). He highlighted the problem where there are many ways of identifying a package - CPE, PURL, SWID tags, SWIHD and gitBOM and there needs to be a working and agreed upon way. He said SBOMs are coming, so vendors should be making plans. The overall problem is the number of vulnerabilities is increasing and there’s a lack of comprehensive global identification solutions that impedes the full benefits of SBOM and VEX. Help out here https://www.cisa.gov/sbom 


### Trusted-SBOM: On the Critical Importance of Verifying SBOMs - Haoxiang Zhang, Huawei & Ahmed E. Hassan, Queen's University

This talk was about the quality of SBOMs as both Haoxiang and Ahmed felt that if the industry produces SBOMs that are of low quality then it won’t be adopted. Low quality includes both false-postives and false-negatives something that plagues the application security tool ecosystem. There were two measures they devized: correctness of SBOM and completeness of SBOM.

> Correctness = # of packages found in software / # of packages declared in the SBOM
> Completeness = # of files in software for which provenance can be determined based on SBOM / # of files in binary

They took a set of open source software and measured how correct the SBOMs were and the results weren’t great. This was a small set and they outlined the need to have a comprehensive study. They also highlighted the need for a community effort to achieve trusted SBOMs which included an agreed upon definition of SBOM quality metrics under correctness and completeness. There’s also a need for tools to measure the quality where organizations can quality stamp a vendor's SBOMs “like how farms are verified organic”.


### Composing the Ultimate SBOM - Ivana Atanasova & Velichka Atanasova, VMware

Ivana and Velichka made the case that the ultimate SBOM is one that’s composed from many “micro” SBOMs. That is each of the packages within your dependency graph has an SBOM and you combine these “micro” SBOMs into a single SBOM that describes your package and its dependencies. They spoke about the dependency graph nightmare and the current tooling that is out there to work with SBOMS

- Tern
- Fossology
- Scancode
- Spdx tools
- Oss review toolkit
- Salus - at build
- Pkgconf bomtool
- K8s BOM

They highlighted that 90% of organizations have started their SBOM journey and the problem with not having a standard for those SBOMs

- Risk that data is not comprehensive
- Prevents interoperability
- Hard to use from edition auctions
- Hard data exchange 


They listed the current standards

- SPDX - Linux Foundation - official approved ISO standard
- CycloneDX - OWASP


They highlighted the 3 things you need to generate an ultimate SBOM

- SBOM Data - what your software contains
- Standardized Formats
- Tool

They said the most comprehensive SBOM is from source or at build time but you can’t always rely on getting them. Could an SBOM exist for each of the components like having micro SBOMs that describe each component but that might end up with 100s or 1000s+ of SBOMS. The good thing is that you can stitch them together into a single SBOM. SBOM composer takes micro SBOMs and combines them.

They closed by saying there’s both a technical challenge and a getting together challenge, every actor in the supply chain needs to generate their own SBOM.

Links https://automatecompliance.org/ 
Tools https://github.com/act-project/TAC
SBOM and SLSA https://slsa.dev/blog/2022/05/slsa-sbom 


### VEXing Open Source Security: Vulnerability Data for Everything - Andrew Martin, ControlPlane & Andres Vega

Andrew and Andres explain that VEX is a standard for communicating the vulnerability risk of exploitation for software and is machine readable. It grew out of the NTIA multistakeholder process and the framing working group. Started in 2020 and continued into 2022 facilitated by CISA

They outlined 3 solutions for addressing vulnerable code?

- [DARPAs Cyber Grand Challenge 2016](https://www.darpa.mil/program/cyber-grand-challenge)
- People doing the work for you from RHEL, Google, Assured Open Source, Alpha Omega
- Vendors releasing exploitability indicators

They argue that an SBOM should be static and should be definitive. But a VEX document should be dynamic and exists fleetingly at the moment of creation. Should they both co-exists? They say that shipping Vex with SBOM makes no sense as its static vs temporal data where a VEX needs to be updated with new information as it emerges around certain vulnerabilities. Both formats should be signed but VEX must be API-retrievable. SBOM is trusted only when done at build time where at package and reverse engineering there’s less accuracy. 

The outline the current issues 

- SBOM currently represents any bill of materials e.g. a single binary, compiled application and libraries, a package or tarball, a container image with other binaries, packages and arbitrary files. False positives slowly erode trust and false negatives instantly obliterate trust. 
- How is the SBOM derived - at compile / build time, package time, observation time? How do you scan for software in a download container image or library, do you trust the mainterer to disclose vulnerabilities, do you trust the packer to proxy the original SBOM or VEX.
- VEX needs to be shared through an API with vendor continuous analysis along with independent analysis. Can you trust a VEX or SBOM. Also you need a secure metadata distribution and storage.


### Attesting Practically: Exploring the Glue Behind Secure Runtime Environments - Jim Bugwadia, Nirmata

Jim presented on how you can improve your supply chain security using attestation. He spoke on how metadata are artifacts that provide information on artifacts or build environments such as provenance, SBOMs, VEX etc.

He defined attestation as signed statements from a trusted entity about the build environment and artifacts. It conveys that the build was executed in a trust environment and the artifacts comply with organizational and regulatory requirements. He highlighted tools from Sigstore that could be used to handle signing, verification and provenance - cosign, fulcio and rekor. 

He closed with the following points
- Signing and verification of images is a good start
- Attestation provide additional verifiable data (SLSA Level 3+)
- Cosign and [Kyverno](https://kyverno.io/) make it easy to sign and verify attestations
- VEX aims to solve major issues with vulnerability management and fits well with the attestation workflow

### How to Identify and Avoid Cracks and Bumps in your Digital Infrastructure? By Considering the Health of Open Source in your Intake Process - Johan Linåker, RISE Research Institutes of Sweden

Johan raised something I didn’t really think about until attending his talk and that is how do you measure the health of open source projects. His motivation was to help organizations to adopt open source as some view it as sub par or insecure especially those in the public sector. His framework which is based on [CHAOSS](https://chaoss.community/metrics/) and consists of 107 metrics over 15 themes and includes things like how active is the communication on the project, how much conflict is there, how much diversity, what is the license, how capable are the contributors, the security, governance, processes etc. He plans on releasing this framework and future work includes 

- Validating, prioritizing and further characterizing the questions in the framework. 
- Gathering metrics and data sources from practitioner
- Investigating application and possibility to automate the framework


### Improving Package Repository Security – From White Papers to Practice - Jussi Kukkonen, Google

Jussi spoke about how difficult it is to get everyone on the same page which makes securing open source supply chains that more difficult. The concept he spoke about was [The Update Framework (TUF)](https://theupdateframework.io/) used by Google, Amazon, DataDog and Sigatore and how it addresses the security of software update systems. Within the open source world he was involved in [PEP458](https://peps.python.org/pep-0458/) and [PEP480](https://peps.python.org/pep-0480/) which features concepts from TUF. However, these were documented in 2013 and 2014 where it took almost 10 years to get PEP458 into PyPI and he reckons PEP480 simply won’t happen. In reflection he said that the TUF specification should have considered repository operations and not only focus on the client and also should have involved those who are developing in the process - don’t just focus on attacks but more on workflows and processes.


With the assistance of his colleague he also gave a live demo of where they signed releases with Sigstore and showed how to achieve tamper proof builds with SLSA.


### Tracking Attackers in Open Source Supply Chain Attacks: The New Frontier - Tzachi ( Zack) Zorenshtain, Checkmarx

Tzachi outlined a problem with package repositories but also the mindset of the security community when it comes to vulnerable code vs malicious code. He started by outlining a couple of high profile cases in npm and PyPI such as UAParser.js, coa, rc (all contained the same malicious code and occurred because of account takeovers being sold on an underground Russian marketplace), he also highlighted a big phishing campaign in August 2022 against PyPI contributors. Unlike the malicious attacks against contributes there also exists protestware like Brandons’s node-ipc, where he decided to add code to wipe people’s computers if they reside in Russia or Belarus. Another problem he raised is the speed at which attackers punch holes in defences such as MFA where EvilProxy added “Phishing as a Service” that can bypass MFA in Github, NPM and PyPI. He argues that no one is taking responsibility to stop these attackers. His team at Checkmarx along with Sentinel Labs have collaborated and tried bring attention, even going so far as to create websites around them

- https://red-lili.info/
- https://cuteboi.info

He also spoke about typosquatting and how successful it really is and with a tool called “Package Lab” you can simply steal the reputation of any other package on any registry which he calls “star jacking” where a malicious package called pampyio did just this where it replicated the code of pampy and made it look like it had the exact same reputation. 

He closed by saying that we’ve been working with vulnerable code for years now but with malicious code we’re still deleting it as soon as its discovered. We need to recognize these are different problems, we should be analyzing and not wiping. He draws distinctions between vulnerable vs malicious

- You can never be okay with having malicious open source code
- You can manage vulnerabilities, but you don’t manage malicious source code

His summary 

- Great power comes great responsibility
- Our software, our responsibility
- Next year will be completely different examples
- Don't take code from strangers

### Tools 

#### BoF: State of SBOM Tools

I joined this discussion thinking it was a talk but in fact it was a room full of SBOM pros. There were lots of ideas thrown around that I’ve tried to capture here. The idea of the discussion was to come up with a good bottom up approach to creating SBOMs - something that you can integrate into your build pipeline. For when the CISO or legal is like I need an SBOM now! 

Why do we need SBOMs
- EO-14028
- Find Log4j
- To find “libraska” (xkcd-2347)
- Evaluation of Stack / Hygiene
  - License Compliance
  - Optimizing engineering stack
  - Risk management
- AI/ML - model and data set
- Integrity check for release artifacts - rather than hash every artifact

Existing Tools

The room listed off some SBOM tools which are captured in the table below. It was funny that we’d spent a long time going over the list when someone shouted up there’s a Github with this info https://github.com/awesomeSBOM/awesome-sbom 

Type: Produce, Consume, Transform, Share

<table>
  <tr>
   <td>Name
   </td>
   <td>Type
   </td>
   <td>Description
   </td>
  </tr>
  <tr>
   <td>tern
   </td>
   <td>Produce, consume
   </td>
   <td>Generate SBOM for contiaers and filesystem
   </td>
  </tr>
  <tr>
   <td>spdx-sbom-generator 
   </td>
   <td>produce
   </td>
   <td>Generate SBOM for projects
   </td>
  </tr>
  <tr>
   <td>syft
   </td>
   <td>Produce, transform
   </td>
   <td>Genreates SBOM for containers and filesystems
   </td>
  </tr>
  <tr>
   <td>oss-review-toolkit
   </td>
   <td>Produce, consume
   </td>
   <td>Generates SBOM for source code, projects, and files
   </td>
  </tr>
  <tr>
   <td>bom
   </td>
   <td>produce
   </td>
   <td>K8s SBOM generator
   </td>
  </tr>
  <tr>
   <td>tejolote
   </td>
   <td>produce
   </td>
   <td>Consumes SBOMs and creates attestations
   </td>
  </tr>
  <tr>
   <td>bom-tool
   </td>
   <td>produces
   </td>
   <td>C & C++
   </td>
  </tr>
  <tr>
   <td>sbom-composer
   </td>
   <td>transform
   </td>
   <td>Combines SBOMs
   </td>
  </tr>
  <tr>
   <td>sbom-tool
   </td>
   <td>produce
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>github/trustsource
   </td>
   <td>produce
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td>cyclonedx
   </td>
   <td>produce
   </td>
   <td>Cyclonedx format
   </td>
  </tr>
  <tr>
   <td>fossology
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
</table>

Gaps in functionality
- Bom query language
  - Retrieve data
  - Graph view
- Interoperability between formats
- Semantics for different artifacts
- Names are different for different ecosystems
- Lacking tools to read SBOMs. Need to parse and query them
- Relation in the SBOM to whatever its describing - a certification mechanism - the SBOM is in fact associated with the binary
- Disparate ecosystems - incentives for those ecosystems e.g. PyPi, NPM to produce SBOMs
- Reality is not captured by SBOM
- Every SBOM tool will create a different SBOM so consumer tools are hindered - what is the relationship between the packages because every tool encodes it differently 

Other tools of note

#### Gitsign – Keyless Git Commit Signing - Billy Lynch, Chainguard Inc
[Gitsign](https://github.com/sigstore/gitsign) - Keyless git commit that tackles the A in SLSA where submit unauthorized changes to the code. Developed by Sigstore and uses an IODC provider to sign each git commit. Question was does this break the git ethos that you don’t need an internet connection to use git - yes it kind of does. 

#### From Kubernetes With  Open Tools For Open, Secure Supply Chains - Adolfo García Veytia, Chainguard
[Tejote](https://github.com/puerco/tejolote) - A highly configurable build executor and observer designed to generate signed SLSA provenance attestations about build runs.

#### Do You Know What's in the Software You Run? Introducing GitBOM - Nell Shamrell-Harrington, Microsof
[gitbom](https://github.com/git-bom/spec) - inspired how git works to produce a SBOM produced at build time
