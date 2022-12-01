---
layout: post
title:  "Automating SBOM Ingestion"
date:   2022-11-30 23:14:05 +0000
categories: supply-chain
tag: sbom supply-chain owasp automation devsecops
---
So you've generated a couple of SBOMs, or you've got access to SBOMs from software you're using - now what? 


A good start would be to set up [Dependency Track](https://docs.dependencytrack.org/getting-started/deploy-docker/) 
locally. This is an OWASP project, so you get all that open source goodness and the project itself is really polished and 
easy to set up and use. What's great is the project is `built using a thin server architecture and an API-first design` 
so it's super easy to set up automation around the project. Big shout out to the project leaders Steve Springett and 
Niklas Düster for an awesome job!

## Install
Docker compose is an easy way of getting up and running and the maintainers have provided a compose file at 

```bash
# Downloads the latest Docker Compose file
curl -LO https://dependencytrack.org/docker-compose.yml

# Starts the stack using Docker Compose
docker-compose up -d
```

Once you log in [http://localhost:8080](http://localhost:8080) with `admin` `admin` and update your password you'll need 
to go ahead and set up an API key. Out of the box there's one set up already under 
`Administration -> Access Management -> Teams -> Automation` where you'll just have to add the permission 
`PROJECT_CREATION_UPLOAD`

![](../assets/images/2022-11-30-dependency-track-api-setup.png)

## SBOM Generation

As an example of how to set up SBOM generation I'm going to use work I've been doing on the OWASP project 
[Security Shepherd](https://github.com/OWASP/SecurityShepherd). The automation is generating SBOMs at 
different phases of the software development pipeline. Firstly at the build through the `pom.xml` and then generating an 
SBOM when the software is being packaged into docker.

### Maven

To generate an SBOM against the `pom.xml` I've used another of Steve's projects the 
[CycloneDX maven plugin ](https://github.com/CycloneDX/cyclonedx-maven-plugin). You simply import it into the pom like so

```xml
<plugin>
    <groupId>org.cyclonedx</groupId>
    <artifactId>cyclonedx-maven-plugin</artifactId>
    <version>2.7.1</version>
</plugin>
```

Then the following maven command is the run when a [release](https://github.com/OWASP/SecurityShepherd/blob/6aae0a513726204e63202c1d1b4a7d675b5242b9/.github/workflows/release.yml#L29) 
is triggered of Security Shepherd 

```bash
mvn clean install -Pdocker -DskipTests -B -DexcludeTestProject=true cyclonedx:makeBom
```

### Docker

For the Docker images I've decided to go the Anchore's [Syft](https://github.com/anchore/syft) where they conveniently 
offer it as a [Github Action](https://github.com/anchore/sbom-action). 

SBOMs are being [generated](https://github.com/OWASP/SecurityShepherd/blob/6aae0a513726204e63202c1d1b4a7d675b5242b9/.github/workflows/release.yml#L37) from these three Docker image
1) Tomcat
2) MariaDb
3) MongoDb

Assuming you've got some automation in place to create SBOMs you'll need to pull them from that location and feed them
into Dependency Track. 

 
