---
authors:
- younjin

categories:
- Talk Abstraction
- Pivotal

date: 2018-06-03T17:16:22Z
draft: false
short: |
  All talk abstractions 
title: Abstracts
---

# Abstracts

Updated at 03 June 2018 
All talks are available. 

## Netflix MSA and Pivotal 

In this talk, we will explore why Netflix started their MSA journey on cloud. There are several key points to build up Netflix MSA, which is not only about tools, but also culture and platform. After this session, audiences will get an overview of "How replicate Netflix MSA" for Enterprise organizations. 

* Why Netflix Changed 
* MSA Expectations & Considerations 
* Organizational Changes 
* Full Cycle Developers 
* Netflix OSS tools 
* Built to Adapt 
* Conclusion 


## Spring Cloud Data Flow with Demo 

Based on Fred Melo's talk (@fredmelo_br), this session describes what is Spring Cloud Data Flow, what's the benefits using it, and how it works. For many years, Enterprises were used ESB or ETL for their data moving part. But it's not enought to handle real-time data process and various tool for different analytic problems. In this talk, audiences will get an idea how they create "Data Microservices" for flexible data pipelining. 

* Traditional Data Moving and it's problems 
* Intorduing Spring Cloud Data Flow 
* Fraud Detection demo 
* How it can be used in your architecture 


## Project Riff for Open Source FaaS 

FaaS (Function as a Service) is becoming a new way of build services. Beyond Micro Services, it offers Micro Service as a Function. And it's based on "event-driven" pattern as native.  There are some famous FaaS such as AWS Lambda, but public offerings are having limits to use sometimes. It will be better soon, but Enterprise need another way to use FaaS for various reasons such as security, long running processes (LRP), event driven with other legacy services. [Project Riff](http://projectriff) has designed for this requirements. And it will be a part of Pivotal Cloud Foundry as PFS(Pivotal Function Services) for Enterprise use, but others can use it as an open source.  

* Introducing Project Riff 
* Set a Riff Environment on Kubernetes 
* Deploy a Function to Riff 
* How it works 


## Introducing Spring Cloud 

[Spring](http://spring.io) is very popular framework in Java world. Recently, Spring team announced Spring Cloud Project based on Spring Framework, and Spring Boot, to leverage Cloud Based application implements. Cloud is extreme environment that changes very often. Cloud Environemt is different with Legacy system, so there's different way to build and deploy applications. After this session, audiences will get core concepts about how to make Cloud Native Applications with Spring Cloud Project. 

* API Gateway 
* Availability Zone aware Load Balancing 
* Externalize Configurations 
* Service Discovery 
* Distributed Tracking 
* Circuit Breaker 
* Global Locks 
* Distributed Messaging 



## Dive Deep into Cloud Foundry 

[Cloud Foundry](http://cloudfoundry.org) is a Platform to support Full Cycle Developers in terms of Delivery. With Cloud Foundry, Developers don't have to remember where's the databases are located, message brokers are placed, and how your application is load-balanced. All you need to do is using 'cf' command line interface, to prepare Backed-services for applications, Deploy new / exsisting applications, and bind them together. In this session, we will explore from Cloud Foundry Over View, and Dive Deep into its Architectures. Audiences will get an idea about "why Cloud Foundry is important for Developers". 

* Introducing Cloud Foundry 
* Technical Overview 
* Archicture Dive Deep 
* How to integrate with 3rd party services 


## Deploying software with BOSH 

[BOSH](http://bosh.io) is a tool from Google's BORG to software lifecycle management. Not like Ansible, Chef, Terraform, and similar tools, BOSH manages how to deploy software, and it's health managements, and providing self-healing mechanism base on Cloud. In this session, audiences will get how BOSH works, and how to implement software/cluster deployment with BOSH. 

* Introducing BOSH 
* How it works
* Manage Stemcell, Release, Jobs, Deployments, and Canary Deployments 
* In depth Topics 


## Netflix OSS Dive Deep 

[Netflix OSS](http://netflix.github.io) introducing various type of tools, that are optimized for Amazon Web Services. There are many kinds of tools, so we will explore one by one in detail. 



# Hands on sessions 

To make better understands based on each talks, hands-on sessions available. Each sessions designed for 3hours to half-day, and laptop preparations. 

## Spring Cloud Hands-on 

Audiences will deploy Spring Cloud tools and sample applications on their Laptop for Desktop. 

## Concourse Hands-on 

Audiences will create CI Pipeline on their Laptop/Desktop. 

## BOSH Hands-on 

Audiences will deploy ElastiCache cluster on Amazon Web Services or Google Cloud Platform. 

## Project Riff Hands-on 

Audiences will deploy Project Riff on their Laptop and also Deploy NodeJS/Spring Cloud Functions to Riff. 

## Cloud Foundry Developer Hands-on 

Developers will experience how they can deploy Java/PHP/Go/Python/Ruby/dotNET/Docker Image/Binary/Static Files to Cloud Foundry. And also explore how to prepare backed-services for those applications. 

## Cloud Foundry Operations Hands-on 

Operators will learn how to manage Cloud Foundry with BOSH, and how to create Tiles for Cloud Foundry Marketplace. And for DevOps, there's a special session for how to make Service Brokers for Cloud Foundry as well. 

## GreenPlum Hands-on 

In this session, audiences will deploy Greenplum on their desktop/laptop. And Link it to another tools such as Tableru, to visualize their data analytics. And also explore how MPP works, and how it's different with OLTP databases. If there's any request for production level deployments, Amazon Web Services could be used for this session. 

## Netflix OSS Hands-on 

Netflix OSS is quite hard to use natively. In this session, audiences will experience how to use Netflix OSS on their laptop. Base on the experience, Some of audience will learn how to use it on their production services. 

## From LAMP Stack, to MSA 

In this session, audience will deploy Single LAMP stack with WordPress. And based on service scale requirement, evaluate the LAMP stack to distributed service. 


## HPC Cluster 101 

In this session, audiences will get an Idea how to make large scale Batch Procssing service. BOINC, Queue Managers like torque, and Compute node designs for MPI or just Map/Reduce clusters to compute such as Rendering, Molecule calcualtions, and GPGPU workloads. 

## In-Memory-Cache-Grid Hands-on with Gemfire 

In this session, audiences will explore how to create WAN level clusters to manage in memory key-value store that leverages high-speed data references. IMDG is one of key tools to manage RDMBS overloads, by having memory stores as trasactional buffer or cache. Ticketing systems, Product selling, and Digital Marketing expampels will be used. 


## Spring Cloud Data Flow hands-on 

Data Pipeline is one of critical part nowadays. Spring Cloud Data Flow provides pipelining capability to eliminate Data Monolith. In this session, audiences will get logs from Applicaitons, and send the logs to Spark Micro Batch Processing and Save those data to IMDG and MPP based DW. After this Hands-on, audiences will get an idea of Data MicroServices, and how to use it. 


# My Bio 

It's been more than 16 years that I've been worked in IT industry. Started as Kenel & Driver engineer, than system engineer, and now working as DevOps and Technologist. Designed global service systems and its flow with AWS since 2009, and involved as DevOps engineer to build Korea Telecom's public cloud at 2011. Worked for 1st open source based hosting company in Korea as researcher, to solve long and short term issues. Also worked as AWS Solutions Architecture and helped more than 500 companies to move their applications to AWS Cloud. Now working at Pivotal as Principal Technologist, to write articles, speaking at events, creat demo, and developer engagements in Korea and Japan. 