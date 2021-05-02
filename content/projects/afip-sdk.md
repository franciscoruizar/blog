---
author: "Francisco Ruiz"
title: "Afip SDK"
date: "Mar 10, 2021 9:42 AM"
tags: ["Java", "SOAP", "AFIP", "SDK", "Ports and Adapters"]
ShowToc: false
ShowBreadCrumbs: false
---

## ℹ️ Introduction

Library to connect to the AFIP Web Services

## 🏁 How To Start

1. Install Java 11
2. Clone this repository: `https://github.com/franciscoruizar/afip-sdk`.
3. Generate a certificate and private keys for
   AFIP: [Read more](https://github.com/franciscoruizar/afip-sdk/blob/main/docs/certificates.md)
4. Execute some [Gradle lifecycle tasks](https://docs.gradle.org/current/userguide/java_plugin.html#lifecycle_tasks) in
   order to check everything is OK:
    1. Create [the project JAR](https://docs.gradle.org/current/userguide/java_plugin.html#sec:jar): `make build`
    2. Run the tests and plugins verification tasks: `make run-tests`

## ☝️ How to update dependencies

* Gradle ([releases](https://gradle.org/releases/)): `./gradlew wrapper --gradle-version=WANTED_VERSION --distribution-type=bin`
