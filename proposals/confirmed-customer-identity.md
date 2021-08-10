# Confirmed Customer Identity

### Area

Domain Specific Taxonomy

### Working Groups

Local Retail Working Group
Local Retail and Delivery Network Working Group

### Submission Date

Tuesday, 3rd Aug, 2021

### Contributors

- Thejesh GN < thej@peppo.co.in>

### Status

Proposed Standard

### Table of Contents

| **No** | **Title** |
|--------|-----------|
|1|Current status in Beckn|
|2|Why is authenticated customer information required|
|3|What we need|
|4|Prior Art|
|5|Proposal|
|6|Corollary|


### Abstract

Customer Identification is critical for an ordering system to attach an order to a customer. Confirmed customer identifiers are required for providing support, giving loyalty points, contacting customers for any other reason, etc. 

Currently, Beckn uses a mobile number (from Billing) as the primary identification, which may or may not be customer's number.

There is no clear way to send Customer Identification in the Beckn for now. We are proposing Customer Identification as part of the standard. So it can be independent of Billing details and can be flexible enough to include various kinds of identifiers.

### Contents

#### Current status in Beckn
- Currently, `order` schema doesn't have a customer property
- The `order.fulfillment` has the customer attribute but this is where the product gets delivered. - This could be my friend's name and address if I am shipping to a friend's place. But I still own the order.
- The `order.billing` has the customer attribute, but this could be the address of the person whose credit card the customer is using. This may or may not - belong to the customer who owns the order.


#### Why is authenticated customer information required
1. Standard customer info, Every order should be owned by an authenticated customer. We need that customer information.
    - This is different from billing identity
    - This is different from end delivery address identity
    - This would be the same as the person who logged into the BAP
2. To provide any support that is required to provide related to his account
    - Access to her previous transactions with us
3. To identity the customer for loyalty, to award loyalty points regardless of which channel they orders from
4. Ability to derive the customer taste, make recommendations to them over time
5. To apply the pre-approved offers or discounts for them
6. To aggregate customer ratings regardless of through which channel he ordered from 
7. Ability for end restaurants to own and manage customer data. Do promotions etc
8. For fraud detection - mostly ratings, feedback, etc
9. Legal reasons

#### What we need
We need a customer object with identity (can start with mobile) authenticated and confirmed by the BAP. BAP takes 100% responsibility.

This is required for both order and intent.

### Prior Art
- Google Spot - mobile number
- [BBPS - Bharath Bill Pay API](https://www.npci.org.in/PDF/npci/bbp/notified-documents/BBPS%20API%20Specifications%20v13.0%20-%2005112019_0.pdf) 's use a similar key and value approach for customers. With standard keys like MOBILE, EMAIL, PAN etc
- WhatsApp Business - mobile number
- Wechat Apps and Mini Program - WeChatId and mobile number

#### Proposal
Add identifier to the person object and add `order.creator` and  `intent.creator`

It can have an array of 
    - `customer_identifier_type`  - ENUM (MOBILE, EMAIL)
    - `cusomer_identifier` - Actual Value 


#### Corollary
1. How do we allow the customer to reuse his already saved address and other information at BPP
2. How is data ownership handled? We believe the providers own customer's data regardless of which channel they order from. 

### Acknowledgments

###  Copyright Notices
    
###  License
Attribution-NoDerivatives 4.0 International