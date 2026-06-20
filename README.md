# Bridebook Supplier Directory Optimisation: Data Cleaning, Enrichment & Scalable Data Integration

## 1. Project Background

### Business Context

Bridebook maintains a large supplier directory that helps couples discover wedding service providers across different categories.

However, maintaining the quality of such a directory presents several challenges:

* Duplicate supplier entries caused by multiple branches or inconsistent naming.
* Missing contact information such as websites, emails, and phone numbers.
* Inconsistent formatting across supplier names and URLs.
* Difficulties integrating newly scraped supplier data without introducing duplicates or degrading data quality.

The objective of this project was to improve the quality of Bridebook's existing supplier directory while integrating a newly scraped dataset in a scalable and repeatable manner.

Rather than focusing solely on cleaning the data, the project aimed to answer a broader business question:

### *How can Bridebook continuously improve directory quality while preserving valuable supplier information and preventing duplicate records?*

---

## Business Objectives

The project aimed to:

* Clean and standardise the existing supplier directory.
* Identify and handle duplicate suppliers intelligently.
* Enrich existing records where possible.
* Integrate newly scraped supplier data without creating duplicates.
* Preserve valuable supplier information instead of aggressively deleting records.
* Build reusable cleaning logic for future directory updates.

---

## 2. Data Overview

Two datasets were provided:

### Current Data

Bridebook's existing supplier directory containing:

* Supplier Name
* Supplier Type
* SubType
* Website
* Email
* Phone Number
* Postcode
* Site Status
* Redirect URL
* Deletion Flags

---

### Directory Scrape

A newly scraped supplier dataset containing:

* Supplier details
* Contact information
* Website URLs
* Business categories

The challenge was to merge these datasets while maintaining consistency and avoiding duplicate entries.

---

## 3. Methodology

### Step 1: Explore and Assess Data Quality

The project began by profiling the existing supplier directory.

The analysis focused on:

* Missing values
* Duplicate websites
* Duplicate emails
* Data type inconsistencies
* Category distributions
* Low-quality records

This initial assessment helped identify which fields required cleaning before any merging could occur.

---

### Step 2: Standardise Supplier Information

To create consistency across both datasets, key columns were cleaned and normalised.

This included:

* Converting text fields to lowercase.
* Removing leading/trailing whitespace.
* Standardising supplier names.
* Cleaning email formats.
* Normalising website URLs by removing:

  * http://
  * https://
  * [www](http://www).

For example:

https://www.clarks.co.uk

became

clarks.co.uk

This ensured that equivalent suppliers would be recognised correctly during matching.

---

### Step 3: Intelligent Duplicate Handling

One of the most interesting challenges involved duplicate websites.

Initially, multiple suppliers appeared to be duplicates because they shared:

* The same website
* The same email address

Examples included:

* Clarks
* Goldsmiths
* Shoezone

However, further investigation showed these represented different physical branches rather than duplicate businesses.

Deleting these entries would have caused data loss.

Instead, a new field was introduced:

### ParentBrand

This grouped suppliers belonging to the same organisation while preserving individual branches.

This approach:

* Prevented information loss.
* Maintained business hierarchy.
* Improved analytical flexibility.
* Preserved supplier coverage.

---

### Step 4: Prepare the Scraped Dataset

The newly scraped supplier directory required additional preparation before integration.

The following steps were performed:

* Renamed columns to align with Current Data.
* Standardised websites and emails.
* Cleaned phone numbers.
* Normalised supplier names.
* Standardised postcode formats.

By enforcing the same cleaning rules across both datasets, the merge process became significantly more reliable.

---

### Step 5: Merge and Integrate the Datasets

A matching strategy was designed using:

### Match Key

Priority:

1. Website
2. Supplier Name

Existing suppliers were identified first and removed from the scraped dataset.

Only genuinely new suppliers were retained.

This ensured:

* No duplicate suppliers were introduced.
* Existing information was preserved.
* New businesses could be added safely.

---

## 4. Key Findings & Business Insights

### 1. Duplicate Suppliers Were Often Legitimate Businesses

The analysis initially revealed a high number of apparent duplicates.

However, these were not necessarily data errors.

Many represented:

* Different physical stores
* Multiple branches
* Regional offices
* Franchise locations

Removing them would have reduced directory coverage.

### Business Insight

Data quality does not always mean removing duplicates.

Sometimes it means recognising business hierarchies and preserving important relationships.

The ParentBrand solution achieved this balance.

---

### 2. Website Was the Most Reliable Business Identifier

Supplier names varied considerably.

Examples:

* Clarks
* Clarks Shoes
* Clark's

However, websites remained consistent.

This made website URLs the most reliable matching key for:

* Deduplication
* Grouping
* Dataset integration

### Business Insight

Stable business identifiers should be prioritised over descriptive text fields when designing scalable matching systems.

---

### 3. Missing Information Did Not Necessarily Mean Low Quality

The scraped dataset contained many suppliers with missing:

* Phone numbers
* Emails
* Site status

However, these businesses still contained:

* Supplier names
* Postcodes
* Categories
* Websites

Automatically removing these records would have unnecessarily reduced directory size.

### Business Insight

Data completeness and data usefulness are not always the same.

A supplier with missing contact information can still provide significant value to users.

---

### 4. Directory Expansion Without Sacrificing Data Quality

The final merge introduced:

* **1,899 new suppliers**
* **23,729 total supplier records**
* **790+ ParentBrand groupings**

The integration strategy successfully expanded directory coverage while maintaining consistency and preventing duplicate entries.

### Business Impact

Bridebook gains:

* Larger supplier coverage.
* Improved directory quality.
* Better business grouping.
* Reusable cleaning logic for future updates.

---

## 5. Scalability & Future Improvements

If more time were available, the following enhancements would be implemented:

### Fuzzy Matching

Detect near duplicates such as:

* Clarks Shoes
* Clark's
* Clarks Ltd

using fuzzy string matching libraries.

---

### Automated Website Validation

Use:

* Requests
* Selenium

to automatically:

* Check website availability.
* Detect redirects.
* Flag inactive suppliers.

---

### External Data Enrichment

Integrate APIs to enrich:

* Missing phone numbers.
* Email addresses.
* Geographic regions.
* Social media information.

---

### Automated Data Pipeline

Create an end-to-end pipeline:

Scraped Data

↓

Data Cleaning

↓

Deduplication

↓

Enrichment

↓

Merged Supplier Directory

↓

Quality Validation

This would reduce manual effort and improve scalability for future supplier imports.

---

## 6. Tools & Skills Demonstrated

### Technologies

* Python
* Pandas
* NumPy
* Jupyter Notebook

### Data Engineering & Analytics Skills

* Data Cleaning
* Data Standardisation
* Data Deduplication
* Data Enrichment
* Dataset Integration
* Record Matching
* Business Rule Design
* Data Quality Assessment
* Exploratory Data Analysis
* Scalable Pipeline Design

---

## Project Outcome

This project demonstrates a practical approach to solving real-world data quality challenges by balancing data cleaning, business reasoning, and scalable integration techniques.

Instead of aggressively deleting imperfect records, the solution focuses on preserving business value, improving data reliability, and creating a repeatable framework for maintaining Bridebook's supplier directory at scale.

