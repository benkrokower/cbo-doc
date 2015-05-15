# System Requirements and Specification for CBO Web Portal System Minimum Viable Product

## Introduction

This documents provides an overview of the CBO Web Portal System Minimum Viable Product (MVP) System Requirement and Specification. 

The aim of this document is to gather, analyze and provide a proposed system specification for the CBO MVP based on the initial Scope of Work 
by defining the problem we are trying to solve, the capabilities that each of the stakeholders required, the flow of data in the system, and
the high-level product features needed.

## Scope

The primary scope of the CBO Web Portal MVP is to allow CBO to have access to the Student Backpack that is now available through P2 system.
It focuses on the CBO as the User of the System, the District as the owner of the Student Backpack, and the P2 system as the mediator for the
data transfer between the District and the CBO.

The scope of this document is to capture the system requirement that is needed to be built in order to satisfy the scope of the CBO Web Portal.

## Definition, Acronyms, and Abbreviations

In this document, we will use the following definition, acronyms and abbreviations:

- Authorized Person: Person that allowed access to a *CBO data*
- CBO: Community Based Organization
- CBO Data: a collection of information about the *CBO*, *Program*, *Student*, *Authorized Person* that is stored in *CBODB*, this does not inclide *Student Backpack*
- CBO System: a collection of CBOAPI, CBOAUTH, CBOCACHE, CBODB and CBOSITE that makes the CBO Wep Portal System 
- CBOAPI: The API service of *CBO* that is used to query the *CBO Data*
- CBOAUTH: The User Authentication service of CBO that is used to authenticate Authorized Person that belongs to a CBO
- CBOCACHE: The encrypted, temporary, non-persistent storage that temporarily store *Student Backpack*
- CBODB: The persistent storage (MongoDB) of the *CBO System* that store *CBO Data* 
- CBOSITE: CBO Site, the Angular JS based front end website application that is used by CBO Authorized Person to access student data.
- HZB: Hosted Zone Broker, a part of P2 that allows for Authentication and Searching and pulling Student Backpack.
- Program: A program run by a *CBO*, have *students* enrolled in it, and have *authorized person* authorized to access it.
- P2: A collection of systems that allows for *student backpack* data transfer between school districts and CBO.
- PRS: Privacy Rules Service, A part of P2 system that *HZB* use to gain permission of a *CBO* and other entity to access *student backpack*, and furnish it according to their permission.
- SRI: The format of the *Student Backpack* that is provided by *HZB*
- Student: A student that is enrolled in a *program*, and member of the *CBO*.
- Student Backpack: Information about student that is available from the school, transferred over *P2* by *HZB*
- Student Data: Information about student that is not part of *Student Backpack* and stored locally in *CBODB*
- System Admin: A person who have access directly to all parts of the 8CBO system*
- UA: User Agent, a browser that is used by a CBO Authorized Person to load CBOSITE and Interacting with CBOAPI service.

## Functionalities

The following are the requirements for the CBO Web Portal functionalities:

### Allow CBO Management
- The *CBO system* shall allow *System Admin* to add *CBO* directly to the *CBODB*
- The *CBO system* shall allow *Authorized Person* to edit some of the *CBO* Data
- The *CBO system* shall allow a unique subdomain name for each *CBO*
- The *CBO system* shall allow *CBO* to pull *Student Backpack* that it has right to from *HZB*

### Allow User Login and Management
- The *CBO system* shall allow *System Admin* to add initial *Authorized Person* directly to the *CBODB*
- The *CBO system* shall allow *Authorized Person* to add other *Authorized Person* for the *CBO*
- The *CBO system* shall send an invitation to a new *Authorized Person* if they are not yet in *CBODB*
- The *CBO system* shall allow *Authorized Person* to login through *CBOSITE*
- The *CBO system* shall allow access to the *Authorized Person* to a specific *CBO*
- The *CBO system* shall send e-mail notification as new *Authorized Person* added to that *Authorized Person*
- The *CBO system* shall only store encrypted password in *CBODB*
- The *CBO system* shall allow *Authorized Person* to edit their profile
- The *CBO system* shall disallow removal of the last *Authorized Person* in a *CBO*

### Display Program Management
- The *CBO system* shall allow *Authorized Person* to add and edit *Program* 
- The *CBO system* shall store *Program* in the *CBODB*
- The *CBO system* shall allow *Authorized Person* to choose the *Program* to manage if the *CBO* have multiple *Programs*

### Display Program Listing
- The *CBO System* shall have a listing of the *programs* in *CBOSITE* only and if only the *CBO* have multiple *programs*.
- The *CBO System* shall allow *Authorized Person* to add and edit *Program* in this listing

### Display Program details
- The *CBO system* shall allow *Authorized Person* to add *Student* to the *Program*
- The *CBO system* shall list, and provide an overview of all the students information enrolled on the *Program*
- The *CBO system* shall show a flag if *Student backpack* for a student is not available from *HZB* in the listing
- The *CBO system* shall allow *Authorized Person* to remove *Student* from the *Program*

### Display Student Detail
- The *CBO system* shall allow *Authorized Person* to edit *Student Data*
- The *CBO system* shall allow *Authorized Person* to link *Student* to the *Student Backpack* and store the link
- The *CBO system* shall show the list of *program* that a student is part of in a *CBO*

### Cache Student Data
- The *CBO system* shall store *student backpack* in *CBOCACHE*
- The *CBO system* shall encrypt all *student backpack* in *CBOCACHE*
- The *CBOCACHE* shall auto expire the *student backpack* stored in 24 hours after it start caching it.
- The *CBOAPI* shall check the *student backpack* from *CBOCACHE* before requesting it from *HZB*

### 

### System Error Handling
- The *CBOAPI* should handle error gracefully
- The *CBOSITE* should handle error gracefully and display informative information to the *Authorized Person*
- The *CBOAPI* should send all of the error messages to *Rollbar*

## Hardware Specification

The CBO Web Portal will run on Virtual Machines. For the MVP, the chosen Cloud infrastructure that will be used to
host these virtual machine is the Amazon Web Service (AWS). For portability between cloud infrastructures, the
CBO Web Portal will be containerized using Dockers. The Dockers will run on top of the Virtual Machines.

The following is the costing of the AWS Services needed to run the CBO Web Portal:

For Production:   
1 x Amazon EC2 Large Instance (m3.large, 7.5 GB Memory): $76.66/month, 1 year commitment   
1 x Amazon ElastiCache Small Instance (t2.small, 2GB in memory storage): $24.89/month   
1 x AWS Data Transfer Out (500 GB): $44.91/month   
1 x AWS EBS Volume (50GB): $6.00/month   
1 x Mongolab AWS M1 (Dedicated Cluster): $180.00/month   
   
For Staging:   
1 x Amazon EC2 Medium Instance (m3.medium, 3.7 GB Memory): $36.50/month, 1 year commitment   
1 x AWS EBS Volume (10GB): $2.00/month   
1 x Mongolab Sandbox: $0.00/month   
0 x ElastiCache Small Instance (shared with production): $0.00/month   
0 x AWS Data Tansfer Out (shared with production): $0.00/month   
   
Miscellaneous   
1 x GeotrustSSL Wildcard SSL Certificate: $439.00/year   
1 x Comodo Wildcard SSL Certificate for Staging: $98.00/year   
1 x Domain Name: $15.00/year   
   
Total monthly cost: $360.96   
Tota yearly cost: $552.00   
   
The CBO Web Portal system    
participant UA   
participant CBOAPI   
participant CBOCACHE   
UA -> CBOAPI: Send me Student Record with ID xxxxxx   
CBPOAPI -> CBOCACHE: Send me Student Record with ID xxxxxx   
CBOCACHE -> CBOAPI: Here is the SRI in XML for ID xxxxxx   
CBOAPI -> UA: Here is the SRI in JSON for ID xxxxxx   
