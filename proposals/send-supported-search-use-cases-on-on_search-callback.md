# Send supported search use cases on on_search callback

### Area

Domain Specific Taxonomy

### Working Groups

Local Retail Working Group
Local Retail and Delivery Network Working Group

### Submission Date

Tuesday, 27th July, 2021

### Contributors

### Status

Proposed Standard

### Table of Contents

| **No** | **Title** |
|--------|-----------|
|1|Current limitations|
|2|Solution approach|
|3|List of search use case codes|
|4|Use case example|
|5|Impact analysis|

### Abstract

BPPs can support different types of searches as per their business use cases. Currently there is no mechanism by which a BPP can declare to the BAP regarding the types of searches it supports. Defining a process by which the BPP can transmit to the BAP the search cases it supports will enable better interoperability between the BAPs and BPPs.

### Contents

#### Current Limitations

Currently a BPP might for example only support search by provider id based on its business use case. But a BAP in the network may fire searches based on item name and the BPP will never return a result. 

#### Solution Approach

If a BPP does not support a search use case that is being sent, in the on_search call back it can optionally send an error object with type **POLICY_ERROR** with code **SEARCH_ERROR_009** and message **Unsupported search use case. Please find the supported use cases:<list of supported use case codes separated by commas>**.

#### List Of Search Use Case Codes

| **Code** | **Use Case** |
|----------|--------------|
|UCS001|Search by provider id|
|UCS002|Search by item name|
|UCS003|Search by provider name|
|UCS004|Search by item availability time|
|UCS005|Search by fulfilment end location|

#### Use case example

If a BPP does not support search by provider id then it can send back the on_search callback as below :
```
{
    "context": {
        "domain": "nic2004:52110",
        "country": "IND",
        "city": "std:080",
        "action": "on_search",
        "core_version": "0.9.1",
        "bap_id": "https://mock_bap.com/",
        "bap_uri": "https://mock_bap.com/beckn/",
        "bpp_id": "https://mock_bpp.com/",
        "bpp_uri": "https://mock_bpp.com/beckn/",
        "transaction_id": "1209849124",
        "message_id": "12341242343",
        "timestamp": "2021-03-23T10:00:40.065Z"
    },
    "error": {
        "type": "POLICY-ERROR",
        "code": "SEARCH_ERROR_009",
        "message": "Unsupported search use case. Please find the supported use cases:UCS002,UCS005"
  }
```

Here this means the BPP only supports search by item name and search by fulfilment end location currently.

#### Impact Analysis

Implementing these changes will not break any existing implementations. This can be an optional feature that a BPP can implement. Similarly a BAP can choose if it wants to retry the search based on the supported search use case.

### Acknowledgments

###  Copyright Notices
    
###  License

Attribution-NoDerivatives 4.0 International