# Get-UserHistory

## .SYNOPSIS
Get-UserHistory is a compact utility to query the Citrix Monitoring database and extract all information pertaining to a user's session history. This process provides indepth detail to a user's activity that none of the established Citrix products offer at the moment.

## .DESCRIPTION
Citrix Virtual Apps & Desktops does not offer much insight in to a user's complete session history. The main Citrix information hub, Citrix Director, only shows sessions that were initiated and terminated at a very top level view.

Ever changing business needs may require a granular view of a user's activity such as every time they disconnected/reconnected a session, what endpoint did they log in from, or how often a user worked remotely vs in the office.

Forunately, all of this information can be pieces together using the Citrix Monitoring Database but querying and joining the information together. Leveraging SQLServer Powershell module, we can remotely query the database for all of the information we require and process it server-side as to not incurr unncessary cycles. Once we have all of our data available, we can neatly package it on the client side and email the report to anyone requesting the information.

## .ACCOMPLISHMENTS/WHAT I LEARNED
Initially, the use case for this script was simply to capture user activity data. However, business requirements changed as we were tasked with also capturing events when a user connects a USB Flash Drive while working in a local office. 
  
Given that we already had the list of internal endpoints a user connected to and the lifespan of the session, all we needed to do was query the endpoints for usb flash drive event IDs.
  
The challange that was presented here was that we could potentially end up querying multiple internal endpoints and filtering out logs for the ID. This could negateiyl impact the scripts execution time which is not ideal.
  
In order to overcome this, PoshRSJOB module was implemented in order to multi-thread the querying process so we can extrapolate the information from all of the endpoints in parallell. This not only helped design an elegant script but also further enforced reusing knowledge from previous implementations.

## .AREAS OF IMPROVEMENT
A small obstacle that was presented was the timezone that was leveraged by the Citrix Monitoring database. Our team is based in US Eastern time whereas the database holds all of its information in UTC format. An area of complexity that was added was processing all of the data and converting it to Eastern time format. The process that was implemented functions as expected and we have the added benefit of not capturing data older than 90 days so the amount of data will not be enmourmaous. However, I would still prefer to optimize the conversion process from UTC to EST/EDT as I believe several cycles can still be shaved off.

## .NOTES
Script was created using Powershell 5.1. 

PoshRSJob, ActiveDirectory, and SQLServer modules are required. 






