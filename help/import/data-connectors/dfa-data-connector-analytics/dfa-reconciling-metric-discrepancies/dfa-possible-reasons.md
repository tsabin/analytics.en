---
description: Lists a number of reasons why data discrepancies can occur between Adobe Analytics and DFA reports.
keywords: DFA
seo-description: Lists a number of reasons why data discrepancies can occur between Adobe Analytics and DFA reports.
seo-title: Possible Reasons for Discrepancies
solution: Analytics
title: Possible Reasons for Discrepancies
topic: Data connectors
uuid: fd414cc6-d568-491e-9b1c-5d493dcfbe45
index: y
internal: n
snippet: y
---

# Possible Reasons for Discrepancies{#possible-reasons-for-discrepancies}

Lists a number of reasons why data discrepancies can occur between Adobe Analytics and DFA reports.

## Users on Safari browser and other browsers that block 3rd-party cookies {#section-4b0dc429a54a4744a33976b0bb2d1b2a}

3rd party cookie acceptance is typically the largest cause of discrepancy between Adobe Analytics and DFA. Safari and some other browsers block third-party cookies by default. This means that by default, Safari accepts the first-party cookie used by most Analytics implementations, but rejects the third-party cookie used by DFA.

A sample of data from our Analytics 15 beta customers showed less than 0.5% of users typically reject first-party cookies, while 5-12% reject third-party cookies (many of these rejections are likely due to default browser settings).

This discrepancy can result in large difference in the data collected by Analytics and DFA.

## Why might impressions reported in DFA be higher than impressions reported in Adobe Analytics? {#section-db0ad070a65a4985bcc589b2d0d30b90}

* DFA sends data to Adobe data collection servers in a nightly batch, so impression data in Analytics can be up to 2 days behind the DFA reports. 
* Adobe uses SAINT classifications to classify imported DFA tracking codes into various levels of aggregations (campaign name, placement name, ad name, etc.) If the discrepancy appears when running a classification report, perform a simple test to see if the classifications have not yet caught up with imported metrics:

    * In the classification report, locate a line item called “None”. 
    * Do a breakdown of this line item by the same variable, using the raw DFA ID report (such as Campaign ID). 
    * In this report, take note of any unclassified DFA tracking codes which are in the form DFA:XXXXX:XXXXX ... 
    * If many of these tracking codes exist, investigate the nightly SAINT classification process.

## Why might DFA clicks be higher than Adobe Analytics click-throughs? {#section-2fce4608ed044bdc9cf812cb719d5d35}

* DFA records a click before the visitor lands on the customer website. Analytics records Click-Throughs after the landing page loads and executes the Adobe JavaScript beacon. Typically, discrepancies are had because either the visitor is not arriving to the landing page after DFA tracks a click, or the *`s.maxDelay`* timer is being hit. 
* Ensure all placements and creatives in the Floodlight Configuration include the clickThroughParam in the landing page URL (for example “?CID=1”). Failing to set this parameter will cause the Adobe Analytics JavaScript to miss any click-throughs which happen after the first hit of the visit. 
* Ensure all placements and creatives have a landing page which is tagged with the JavaScript and has the DFA Integrate module, and that the Floodlight Configuration ID on that landing page matches the Floodlight configuration ID of the served ads. Often, discrepancies come because the landing page for ads are set to third party site, or the served ads. 
* If you use Rich Media ads or Flash (swf) ads, make sure that whenever the DFA clicktracker is hit, that the visitor's browser is also being redirected to the Landing page with the `clickThroughParam` included in the query string. Failure to redirect the browser will not cause a click-through to be recorded. 
* Timeouts represent instances where DFA data might have been available but the JavaScript did not receive a response in time. When a visitor arrives on the landing page, Adobe JavaScript requests the visitor's information from DFA's fls.doubleclick.net service. The *`s.maxDelay`* parameter determines how long the JavaScript waits for the Floodlight Service (FLS) data. If *`s.maxDelay`* is too high, visitors can leave the site before Adobe collects the hit data; meaning that no click data is recorded. If *`s.maxDelay`* is set too low, the visitor's Internet connection cannot retrieve the FLS data in time; meaning that the hit is sent to Adobe without DFA click information. See [Tuning s.maxDelay](../../dfa-data-connector-analytics/dfa-integration/dfa-tuning-s-maxlelay.md#concept-6deb28eee18e414db220d6009d449f0d) for a further discussion on this subject. 

* Bot traffic might inflate the DFA click numbers. Bots may have functionality to click on an ad but may not have the complexity to execute an Analytics beacon or fire the synchronous script tag to load the Floodlight servers request data. If these bots are not removed from the Clicks figure, this might be a source of discrepancy. 
* Visitors which leave the page prior to *`s.maxDelay`* expiring, and DFA data returning, will be lost; no DFA or visitor data will be collected for them. 
* Analytics attempts to identify and remove duplicate Click-throughs so they are counted only once per campaign per visit. DFA counts visitors who click “Back” and pass through the ad redirect multiple times as additional ACM Clicks, while Analytics does not count these as multiple Click-throughs. 
* DFA Floodlight tags do not rely on JavaScript enabled, while Analytics does. Because of this, there might be some cases where DFA records a hit when Analytics does not. To identify if this might be a problem, use the Analytics JavaScript report in the Visitor Profile menu.

## Why might DFA post-impression activities be higher than Adobe Analytics view-throughs? {#section-5daa91039c404df48b6a3447c20406f7}

* Analytics attempts to identify and remove duplicate Click-throughs so they are counted only once per campaign per visit. DFA counts visitors who click “Back” and pass through the ad redirect multiple times as additional ACM Clicks, while Analytics does not count these as multiple Click-throughs. 
* DFA Floodlight tags do not rely on JavaScript disabled, while Analytics does. Because of this, there might be some cases where DFA records a hit when Analytics does not. 
* DFA counts Post-impression activities when using Floodlight tags, which can be placed on the client Web site. Analytics counts View-throughs after the JavaScript beacon (image request) executes. Code placement on the Web page can determine if an aborted page load counts as a Post-impression activity or a View-through.

## What if discrepancies are far outside an acceptable range and the possible reasons above do not apply? {#section-ca50eb75dd5d4d0396f4668b44d7547c}

Consult your Integration Consultant, or Adobe Client Care, to document the discrepancies and report them to the Data Connectors engineering team. To expedite your request, have 2 - 3 days of data comparing the metrics in question (at a campaign code level). In your request, identify all actions you have already taken to reconcile the discrepancy. 
