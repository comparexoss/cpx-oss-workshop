# Microsoft Acik Kaynak Zirvesi - Istanbul 2018

_Delivering modern cloud native applications with ​open source technologies on Azure​_

## Overview

This workshop will guide you through migrating an application from "on-premise" to containers running in Azure Kubernetes Service.

The labs are based upon a node.js application that allows for voting on the Justice League Superheroes (with more options coming soon). Data is stored in MongoDB.

> Note: These labs are designed to run on a Linux CentOS VM running in Azure (jumpbox) along with Azure Cloud Shell. They can potentially be run locally on a Mac or Windows, but the instructions are not written towards that experience. ie - "You're on your own."

> Note: Since we are working on a jumpbox, note that Copy and Paste are a bit different when working in the terminal. You can use Shift+Ctrl+C for Copy and Shift+Ctrl+V for Paste when working in the terminal. Outside of the terminal Copy and Paste behaves as expected using Ctrl+C and Ctrl+V. 

## Lab Guides
  0. [Setup Lab environment]
  1. [Run app locally to test components]
  2. [Create Docker images for apps and push to Azure Container Registry]
  3. [Create an Azure Kubernetes Service (AKS) cluster]
  4. [Deploy application to Azure Kubernetes Service]
  5. [CI/CD Automation using Jenkins Pipeline]
  6. [Kubernetes UI Overview]
  7. [Operational Monitoring and Log Management]
  8. [Application and Infrastructure Scaling]
  9. [Update and Deploy New Version of Application]
  10. [Upgrade an Azure Kubernetes Service (AKS) cluster]
  
## Contributing

This project was forked from Microsoft Container Hackfest lab project. And it also includes additional enhancements by Comparex Turkey OSS Team.

## License

This software is covered under the MIT license. You can read the license [here][license].

This software contains code from Heroku Buildpacks, which are also covered by the MIT license.

This software contains code from [Helm][], which is covered by the Apache v2.0 license.

You can read third-party software licenses [here][Third-Party Licenses].

