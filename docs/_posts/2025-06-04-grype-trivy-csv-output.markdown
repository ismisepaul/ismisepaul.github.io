---
layout: post
title:  "Outputting CSV findings with Grype & Trivy"
date:   2025-06-04 10:02:12 +0000
categories: sca security-tools
tag: trivy grype sca
---

Both [Grype](https://github.com/anchore/grype) and [Trivy](https://github.com/aquasecurity/trivy) are popular
software composition analysis (SCA) tools used predominantly to report on CVEs within container images. However, they 
can be used in other ways. This post will outline how to scan for CVEs (+GHSAs) and output those results to CSV. Both 
tools support templates as a way to output the scanning results, this is what we'll leverage to output results to CSV. 
Assumes MacOS.


# Preparing Images

Pull down the image you want to scan (example [Ubuntu](https://hub.docker.com/_/ubuntu))

```shell
docker pull ubuntu
```

Get the SHA value of the local image you want to scan (example ece images)

```shell
docker images | ggrep ubuntu
```

Example output (SHA == `9d45648b4030`)

```
ubuntu           latest            9d45648b4030   7 days ago      101MB
```


# Grype

[Grype](https://github.com/anchore/grype) is a vulnerability scanner maintained by Anchore. It can be used to scan 
container images and filesystems. Grype can scan from podman, Docker, image archives, OCI, Singularity, local directories 
and files, SBOMs, and directly from registries. Anchore have developed a separate tool called 
[Syft](https://github.com/anchore/syft) that generates SBOMs where these can then be scanned with Grype.

## CSV Template
Using the template below will allow you to output the results in a CSV format
- Create a new folder called `grype` on your filesystem e.g. `$HOME/security-tools/grype`
- Copy the template below and save it as `csv.tmpl` to the newly created folder.

{% raw %}
```
"Package","Version Installed","Vulnerability ID","Severity"
{{- range .Matches}}
"{{.Artifact.Name}}","{{.Artifact.Version}}","{{.Vulnerability.ID}}","{{.Vulnerability.Severity}}"
{{- end}}
```
{% endraw %}

## Scanning Images

### [Option 1] Installed Grype

#### Install
```shell
brew tap anchore/grype
brew install grype
```

#### Run

SHA `9d45648b4030` is that of the Ubuntu image - see "Preparing Images" above

```shell
grype -o template -t $HOME/security-tools/grype/csv.tmpl 9d45648b4030  
```

### [Option 2] Using Docker

```shell
docker run --rm -it \
  -v $HOME/security-tools/grype:/opt \
  anchore/grype:latest  \
  -o template -t /opt/csv.tmpl ubuntu
```


# Trivy

Trivy is a vulnerability scanner maintained by Aqua Security. It can be used to scan software artifacts and also 
deployments. Trivy can be used to scan container images, filesystems, Git Repository (remote), Virtual Machine Image, 
Kubernetes. The tool can be used to detect OS packages and software dependencies in use (SBOM), known vulnerabilities 
(CVEs), IaC issues and misconfigurations, sensitive information and secrets, and software licenses.

## CSV Template

This will allow you to output the results in a CSV format
- Create a new folder called `trivy` on your filesystem e.g. `$HOME/security-tools/trivy`
- Copy the template below and save it as `csv.tmpl` to the newly created folder.

{% raw %}
```
"Package","Version Installed","Vulnerability ID","Severity"
{{- range . }}
{{- range .Vulnerabilities }}
"{{ .PkgName }}","{{ .InstalledVersion }}","{{ .VulnerabilityID }}","{{ .Severity }}"
{{- end }}
{{- end }}
```
{% endraw %}

## Docker Context

Get current docker context

```shell
docker context list
```

Example output (note the name with the asterisk * is the current context)
```
NAME               DESCRIPTION                                          DOCKER ENDPOINT                                 
default            Current DOCKER_HOST based configuration              unix:///var/run/docker.sock
desktop-linux *    Docker Desktop                                       unix:///$HOME/.docker/run/docker.sock
multipass-docker   Used to connect to docker runnin on a multipass vm   ssh://ubuntu@192.168.69.3
```

## Scanning Images

### [Option 1] Installed Trivy

#### Install

```shell
brew install trivy
```

When running ensure to set `--docker-host` with the docker endpoint output outlined in the “Docker Context” section above

SHA `9d45648b4030` is that of the Ubuntu image - see "Preparing Images" above

```shell
trivy --docker-host unix:///$HOME/.docker/run/docker.sock \
  image --format template --template @$HOME/security-tools/trivy/csv.tmpl  
```


### [Option 2] Using Docker

When running ensure to set `--docker-host` with the docker endpoint output outlined in the “Docker Context” section above

Trivy recommends to mount a cache directory as a volume

```shell
docker run --rm -it  \
  -v $HOME/Library/Caches:/root/.cache/ \
  -v $HOME/security-tools/trivy:/opt \
  aquasec/trivy:latest \
  image --format template --template @/opt/csv.tmpl \
  --detection-priority comprehensive \
  ubuntu
```
