# Go Grid Router Implementation
[![Build Status](https://travis-ci.org/aandryashin/ggr.svg?branch=master)](https://travis-ci.org/aandryashin/ggr)
[![Coverage](https://codecov.io/github/aandryashin/ggr/coverage.svg)](https://codecov.io/gh/aandryashin/ggr)
[![GoReport](https://goreportcard.com/badge/github.com/aandryashin/ggr)](https://goreportcard.com/report/github.com/aandryashin/ggr)
[![Release](https://img.shields.io/github/release/aandryashin/ggr.svg)](https://github.com/aandryashin/ggr/releases/latest)

This repository contains a [Go](http://golang.org/) implementation of original [Gridrouter](http://github.com/seleniumkit/gridrouter) code.

## Building
We use [govendor](https://github.com/kardianos/govendor) for dependencies management so ensure it's installed before proceeding with next steps. To build the code:

1. Checkout this source tree: ```$ git clone https://github.com/aandryashin/ggr.git```
2. Download dependencies: ```$ govendor sync```
3. Build as usually: ```$ go build```
4. Run compiled binary: ```$GOPATH/bin/ggr```

## Running
To run Gridrouter type: ```$ ggr -listen :4444 -quotaDir /path/to/quota/directory -users /path/to/.htpasswd```. See [example browsers.xml](https://github.com/aandryashin/ggr/blob/master/quota/browsers.xml) and [example .htpasswd](https://github.com/aandryashin/ggr/blob/master/.htpasswd).

## Generating users file
This implementation is using [htpasswd](https://httpd.apache.org/docs/2.4/misc/password_encryptions.html) files to store authentication data, i.e. password are normally stored in encrypted form. To create such file type:
```
$ htpasswd -bc /path/to/new.htpasswd username password
```
To add a new record to existing file:
```
$ htpasswd -b /path/to/existing.htpasswd username password
```
You certainly should have ```htpasswd``` utility installed.

## Building Docker container
To build [Docker](http://docker.com/) container install Docker and type:
 ```
 $ GOOS=linux GOARCH=amd64 CGO_ENABLED=0 go build
 $ docker build -t ggr:latest .
 ```
 This will create an image named ```ggr:latest``` in your local storage.

## Quota Reload and Graceful Restart
* To **reload quota files** just send **SIGHUP** to process or Docker container:
```
# kill -HUP <pid>
# docker kill -s HUP <container-id-or-name>
```
* To **gracefully restart** (without losing connections) send **SIGUSR2**:
```
# kill -USR2 <pid>
# docker kill -s USR2 <container-id-or-name>
```
