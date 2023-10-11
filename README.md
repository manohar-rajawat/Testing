# Salesforce Data Integration with EAB Advisor Data

## Introduction

In this documentation, we will explore a real-world case of data integration in Salesforce. The problem is related to updating advisor data for students in Salesforce using data from the EAB (Education Advisory Board) website. The manual process of updating this data is time-consuming and prone to errors. To solve this, an Apex class, `EABAdvisorFetchingBatchJob`, has been created to automate the data retrieval and update process, saving time and improving data accuracy.

## Problem Statement

The university faced several challenges related to advisor data:

1. **Manual Data Retrieval**: Advisor data from EAB had to be manually downloaded from the website in Excel format and then parsed through a Python script to fit Salesforce's data structure. This process took around 2 hours of manual effort monthly.

2. **Data Inconsistency**: Advisor changes in the EAB portal were not always reflected in the student portal of the university. This resulted in students having the contact information of their previous advisors, causing confusion and inefficiency.

## Solution Overview

The solution to these problems involves the creation of an Apex class, `EABAdvisorFetchingBatchJob`, which runs as a daily batch job. This class fetches advisor data from EAB using APIs and updates the Salesforce records accordingly. This approach not only eliminates manual work but also ensures that the Salesforce records are consistently up to date.

## Technical Details

### Apex Class Structure

The `EABAdvisorFetchingBatchJob` class is implemented as a Salesforce Batchable class and is designed to run daily. It utilizes Salesforce Batch Processing and supports making callouts to external systems, which is necessary for fetching data from the EAB API.

Here's a breakdown of the main components of the class:

#### 1. `start` Method

- The `start` method defines the initial query locator. It identifies the current academic term and retrieves a list of relevant students for data update.

- If there's no active term, the batch job is skipped for the day, ensuring it only runs during active terms.

#### 2. `execute` Method

- The `execute` method is responsible for processing the retrieved student data.

- It sends API requests to the EAB website to fetch the student's EAB profile, including advisor data.

- It maintains sets and maps to organize data efficiently, such as a set of Net IDs for students and related contacts.

- It creates and updates relationships between students and advisors in Salesforce, ensuring the data is consistent.

#### 3. `finish` Method

- The `finish` method handles any post-processing tasks, although in this case, it appears to be empty.

### Key Functionalities

1. **Data Retrieval**: The class retrieves student and advisor data from the EAB website through API calls.

2. **Data Transformation**: The EAB data is transformed into Salesforce-compatible format.

3. **Data Update**: Student-advisor relationships in Salesforce are created or updated based on the EAB data.

4. **Error Handling**: The class handles errors and exceptions gracefully.

5. **Batch Processing**: The class operates as a batch job, which can be scheduled to run daily.

6. **API Callouts**: It uses the `AllowsCallouts` interface to make external API requests.

7. **Data Consistency**: The class ensures that advisor data in Salesforce is always up to date, eliminating manual data entry and associated errors.

### Integration with Slack

- The code references a `SlackAPICallout` class, which might be used for logging or alerting purposes via Slack notifications.

### Salesforce Objects Used

- `hed__Term__c`: To determine the active academic term.

- `Career_Summary__c`: Contains information about students and their academic status.

- `hed__Relationship__c`: Stores the relationships between students and advisors.

- `Contact`: Represents the contact records of students and advisors.

## Conclusion

The `EABAdvisorFetchingBatchJob` class exemplifies an effective solution to automate data integration between Salesforce and an external data source (EAB). It eliminates manual efforts, ensures data accuracy, and improves the consistency of advisor information. This approach can be a valuable model for solving similar integration challenges in Salesforce or other CRM systems.
