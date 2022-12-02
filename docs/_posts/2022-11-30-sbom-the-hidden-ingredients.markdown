---
layout: post
title:  "SBOM - The Hidden Ingredients"
date:   2022-11-30 23:14:05 +0000
categories: supply-chain
tag: sbom supply-chain owasp automation devsecops
---

With [78% of code in the codebase being open source](https://www.synopsys.com/software-integrity/resources/analyst-reports/open-source-security-risk-analysis.html)
the contents of software in respect to the packages, libraries and what assembled the software is a black box. SBOMs are
being hailed as the solution to this visibility problem, the established analogy being they're the ingredient label on
software. But what if the SBOM didn't tell the full story and the software contained some hidden ingredients?


## Automation

In this blog post I'm going to go through some automation using various SBOM tools where the generation will happen at
different stages of the build process. We'll analyse the SBOMs and compare the results to an SCA scan of the docker
image.

- SBOM generation - [OWASP CycloneDX Maven Plugin](https://github.com/CycloneDX/cyclonedx-maven-plugin)
- SBOM generation (Docker Image) - [Anchore Syft (SBOM Action)](https://github.com/anchore/sbom-action)
- SBOM consumption / analysis - [OWASP Dependency Track](https://docs.dependencytrack.org/getting-started/deploy-docker/)
- SCA - [OWASP Dependency Check](https://jeremylong.github.io/DependencyCheck/)


## Generating SBOMs

Working with OWASP [Security Shepherd](https://github.com/OWASP/SecurityShepherd) I've gone ahead and set up SBOM 
generation at two stages. The first SBOM is generated during the Maven using the `pom.xml`. The second is being
produced when the docker images are being built and the application being packaged into them.

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

Then the following maven [build](https://github.com/OWASP/SecurityShepherd/blob/6aae0a513726204e63202c1d1b4a7d675b5242b9/.github/workflows/release.yml#L29)

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


### Analysing SBOMs

We'll push the results into [Dependency Track](https://docs.dependencytrack.org/getting-started/deploy-docker/)
and gain the much needed visibility. This is an OWASP project, so you get all that open source goodness and the project itself is really polished and
easy to set up and use. What's great is the project is `built using a thin server architecture and an API-first design`
so it's super easy to set up automation around the project. Big shout out to the project leaders Steve Springett and
Niklas Düster for an awesome job!


Docker compose is an easy way of getting up and running and the maintainers have provided a compose file

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


