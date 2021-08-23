# Supporting subscribers who have national presence

## Area
Core Api

## Status
*Proposed Standard*

## Working Groups

Core-Proposal Admin

## Submission Date
04/Aug/2021

## Contributors
* Venkatraman Mahadevan
* Ravi Prakash


## Table of Contents

| **No** | **Title** |
|--------|-----------|
|1|Current Limitations|
|2|Solution Approach|
|3|Impact Analysis|

## Abstract
Subscribers who have national presence should be allowed to register with null city_id so that their keys etc don't need to be **replicated/regenerated** for each city. 


## Contents

### Current Limitations
A bpp can onboard multiple providers operating from different provider-locations across the country.Forcing the bpp(subscriber) to indicate cities they operate from would be impractical as it depends on the locations of the providers being onboarded with a bpp. 

### Solution Approach 
* Subscribers must be allowed to register with city_id as null .
* Subscriber id must be unique in the registry so that if you lookup by a subscriber_id we should not get multiple records with different/duplicate keys, The subscriber entity must be only one which also publishes the subscriber's public keys. 

`In other words, Subscribers will not be allowed to subscribe for multiple cities with the same subscriber id`

* Beckn Registry can however expose a /register_location api where a subscriber can register the Location (beckn schema object) that allows specification of arbitrary regions where the subscriber provides service. (This would depend on region around specific provider locations in the city_id in which a provider_location resides)

`` Implementation Note: if a provider location is registered as a lat,lng with a delivery radius, then one can precompute (min lat,min lng, max lat , max lng and persist in the registry . these bounds are upper bounds that kind of ensure that all point within these boundaries are within the radius specified.  This would aid in fast lookups later.``

* Registry api /lookup 
    1. A new object attribute "location" may be passed input. (If this is passed, the city_id attribute is ignored is city_id can be passed inside the location object)
    1. if queried for specific city_id, would need to return 
        * subscribers registered with matching city_id 
        * subscribers with national presence registered with city_id as null having locations registered at just the city level matching the specific city_id. 
        * subscribers with national presence registered with city_id as null and does not have any provider locations registered for that subscriber. 
     
    1. if queried with location object representing a specific point (lat,lng) (user's location) then first find the city_id in which this location resides. Then return:
        * subscribers registered with matching city_id, 
        * subscribers with national presence registered with city_id as null having locations registered at just the city level matching the specific city_id .
        * subscribers having locations registered near the user's lat lng that may service the user's location.



### Impact Analysis
Registry Api new (/register_location)
Registry Api change (/lookup)

The suggested changes would be backwards compatible. To use the new features, there would need to be a code change by BAP participants (to pass the user's location) and BG (to use this user location information)


## Acknowledgments

##  Copyright Notices
    
##  License
Attribution-NoDerivatives 4.0 International