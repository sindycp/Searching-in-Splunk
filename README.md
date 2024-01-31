# Searching in Splunk

![Splunk Lab](https://i.imgur.com/WOQooiE.png)

## Overview

Mastering the skill of creating effective searches is essential, allowing one to swiftly and accurately reveal desired information within vast datasets. This proficiency proves particularly crucial in incident response scenarios, where the swift identification and resolution of security incidents are imperative. Furthermore, adept search techniques enhance the ability to efficiently discern patterns, trends, and anomalies within data.

In this scenario, we'll be investigating potential security issues associated with the mail server of a fictitious company, Buttercup Games. Specifically, the task involves examining any instances of failed SSH logins for the root account.

## Objective

In this exercise, Splunk Cloud will be utilized for search and investigation, allowing us to:

- Upload sample log data
- Search through indexed data using Search Processing Language (SPL)
- Assess search results
- Recognize various data sources
- Pinpoint failed SSH login(s) for the root account

## Initial Setup

### Create a Splunk Cloud Account and Activate Trial

To use Splunk Cloud, you must [create an account and sign up to get a free Splunk Cloud trial.](https://github.com/sindycp/Searching-in-Splunk-/blob/main/Splunk%20Account%20Sign-up.md).

### Data Upload

For optimal functionality, it is crucial for SIEM tools to ingest and index data. These tools gather and process data, transforming it into searchable events that can be queried, viewed, and analyzed.

To initiate querying in Splunk, the next step involves uploading data. Follow the outlined steps below to accomplish this task:

1. Download [sampledata.zip](https://github.com/sindycp/Searching-in-Splunk-/blob/main/sampledata.zip). Do not decompress the file.

2. Access Splunk Home within your Splunk Cloud free trial instance. If necessary, log in again using the credentials established

3. On the Splunk bar, locate and click on **Settings**. Then, select the **Add Data** icon.

![SplunkSettings](https://i.imgur.com/I5h4Coh.png)

4. Choose the **Upload** option.

5. Click on the **Select File** button to upload the `sampledata.zip` file and click **Open**.

6. Proceed by clicking the **Next** button to advance to **Input Settings**.

7. Within the **Host** section, opt for **Segment in path** and input **1** as the segment number.

9. Click on the **Review** button to assess the details of the upload before submission. 

![Review Details](https://i.imgur.com/FQA7yoc.png)

10. Select the **Submit** option. Upon completion of the data ingestion process by Splunk, you will receive a confirmation indicating the successful upload of the file.

## Performing a Basic Search

Pause briefly to familiarize yourself with the Splunk Cloud interface. Identify key elements such as the app panel, the quick links panel, and the Splunk bar.

![SplunkExplore](https://i.imgur.com/erTKRiA.png)

Now that the data has been successfully uploaded into Splunk, proceed to execute your inaugural query to verify the ingestion, indexing, and searchability of the data. Follow these steps:

1. Go to Splunk **Home**. (To navigate back to Splunk Home, click on the Splunk Cloud logo found on the Splunk Cloud page.)

2. Click on **Search & Reporting**. Close any pop-ups that may appear.

3. In the search bar, input your query as follows:
```SPL
index=main
```
This query term specifies the index, which serves as a data repository. In this context, the index represents a singular dataset containing events from an index named main.

4. Choose **All Time** from the time range dropdown to search for all events across the entire timeline.

5. Click the **Search** button, identifiable by the magnifying glass icon. Your search should retrieve thousands of events.

![SplunkAllTime](https://i.imgur.com/4y7izUd.png)

**Tip**: Employing brief time ranges in your searches is considered a best practice. This approach yields faster results and consumes fewer resources, enhancing overall efficiency.

## Assess the Fields

As Splunk indexes data, it associates fields with each event, incorporating them into the searchable index event data. This facilitates security analysts in efficiently searching for and pinpointing specific data. Following your initial query, scrutinize the search results and the associated fields.

For each event, the notable fields include `host`, `source`, and `sourcetype`. In the **SELECTED FIELDS** section, review these same fields.

![SplunkFields](https://i.imgur.com/fyPmvyV.png)

Review the field values by selecting the field under **SELECTED FIELDS**. Take note of the following:

- **host**: The host field designates the name of the network host where the event originated. In this search, there are five hosts:

  - `mailsv`: Buttercup Games' mail server. Examine events generated from this host.
  - `www1`: One of Buttercup Games' web applications.
  - `www2`: One of Buttercup Games' web applications.
  - `www3`: One of Buttercup Games' web applications.
  - `vendor_sales`: Information about Buttercup Games' retail sales.

![SplunkHost](https://i.imgur.com/ebozYaZ.png)
- **source**: The source field specifies the file name from which the event originates. Identify eight sources, particularly note `/mailsv/secure.log`, a log file containing information related to authentication and authorization attempts on the mail server.

![SplunkSource](https://i.imgur.com/ymMbqwv.png)

- **sourcetype**: The sourcetype determines how data is formatted. Observe three sourcetypes, and specifically examine secure-2.

![SplunkSourceType](https://i.imgur.com/sC6pP3T.png)

## Refine the Search

To investigate failed SSH logins for the root account on the mail server, you must focus the search on events specifically from the mail server.

In the **SELECTED FIELDS** section, select **host and choose **mailsv**.

Observe the addition of a new term in the search bar: 

```SPL
index=main host=mailsv
```

The search results now have narrowed down to over 9000 events generated by the mail server.

![SplunkMailSV](https://i.imgur.com/HOFW6Z0.png)

## Find a Failed Login by the Root User

Having restricted the search results to events generated by the mail server, proceed to further refine the search to identify any failed SSH logins for the root account.

1. Clear the search bar.

2. Enter the following into the search bar:
   
```SPL
index=main host=mailsv fail* root
```
This query builds upon the previous task's search, incorporating the keyword `fail*` with a wildcard to encompass terms like failure, failed, etc. Lastly, the term `root` is used to identify any events containing the term root.

3. Click on the **search** button.

![SplunkFailed](https://i.imgur.com/cKK0b7V.png)

## Analyze the Outcomes of the Search

The preceding search is expected to have produced over 300 events. Explore additional pages in the search results to view events beyond those listed on the initial page.

## Final Thoughts
In conclusion, we have delved into the exploration of basic searches using Splunk's querying language, known as Search Processing Language (SPL). This endeavor has highlighted the versatility of SPL in efficiently querying and analyzing data within Splunk. Splunk, as a notable SIEM tool, holds significance by serving as a robust platform for the storage, analysis, and reporting of data originating from various sources. Its comprehensive capabilities make it an integral component for security analysts, enabling them to navigate and extract valuable insights from diverse datasets, ultimately contributing to effective cybersecurity measures and incident response.
