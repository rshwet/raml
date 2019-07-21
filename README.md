# raml
Design a RESTful API using RAML

===============================================================================================
                                    RAML SPECIFICATIONS
===============================================================================================

Designed a RESTful API using RAML for a single resource, customers and that allows processing of CURD operation using the API.

The main api file is retailapi.raml. RAML version 1.0 is used for the api implementation. Media type used for the api is application/json.

This API will allow:

 1. Listing of customers
 2. Creation of new customer
 3. Selection of customer based on criteria - customer Id
 4. Update of selected customer
 5. Deletion of selected customer

An independent Customer data type is defined in Customer.raml under api data types to promote reusability. This data type is included and reference in Retail api file. Mocking data examples have been stored under examples folder in .json format. These are been included for mock testing. 

The customer resource has following properties:
 1. customer id
 2. customer first name
 3. customer last name
 4. customer address
          4.1 address line 1
          4.2 address line 2 ( optional and nullable)
          4.3 city
          4.4 state
          4.5 country
          4.6 zip

Complete project has been implemented and tested mulesoft data center and API manager.

===============================================================================================
                                          USECASE 1
===============================================================================================

USECASE: A consumer may periodically (every 5 minutes) consume the API to enable it (the consumer) to maintain a copy of the provider API's customers (the API represents the system of record)

To enable the consumer, consume the API every 5 minutes, the request is being retriggered by invalidating the client cache. To achieve this Cache-control parameter in header is set to max-age of 300 seconds. After every 300 seconds the cache will be set as stale, triggering a re-request for the data. Thus, ensuring a periodic refresh of data.

Other way of achieving this could have been the client thread, re-requesting data from the api in a loop after a wait of 300 seconds. But that would be a little dirty approach of having a seperate thread running in the background just to retrigger data refresh.

===============================================================================================
                                          USECASE 2
===============================================================================================

USECASE: A mobile application used by customer service representatives that uses the API to retrieve and update the customers details

Using their app, the customer service representatives can enter the id number associated with each customer and retrive customer details. Once the customer details have been retrived, the executive can then update and delete the customer information. 

Similarly, we can implement searching of customer using mobile number or email-id, which will allow the customer service representative to locate the customer record in the system and retrive the same, especially when customer id is not known. Once the data is retrived, a similar put method implemetation can be written. But I have limited the scope to the defined usecase.

To ensure, efficient network utilization, status code returned on successful updation is 204 - which indicates, request has succeeded with no body content. An update request does not require customer data in response, as it has the same data that has been persisted. A confirmation of successful execution of update will be sufficient.

To keep track of the resource that has been successfully updated, Etag property of header is used, which carries the etag for the resource involved in the transaction.

This usecase has a scope of implementation of basic authentication logic using securitySchemes, to ensure only valid user is able to update or delete customer data. But this I believed is a bit out of scope for now, especially when the client side project scope is not known.

===============================================================================================
                                          USECASE 3
===============================================================================================

USECASE: Simple extension of the API to support future resources such as orders and products 

The existing model can be easily extended to implement product and order resources. For product and order resources, we will have to define product and order datatypes under apidatatypes. Also, the .json files with examples can be stored under examples folder. Once all these files are defined,  they can be included to retailapi.raml and relevant methods for api can be implemented.

We can then associate customer with orders and orders with products by creating subresources.

===============================================================================================
                                     DESIGN DECISIONS
===============================================================================================

1. This has been my first encounter with RAML and Mulesoft. It has been a great learning experience until now. I had to read and listen to a lot of text and videos to understand RAML and its implementation style. Also, with upgraded version of mulesoft, following how to utilize anypoint platform was confusing. The first major decision to make was, if i should implement API raml using mulesoft or notepad. But a little bit of extra struggle to begin with into understanding anypoint platform helped a big way.

2. Then case the design decision, if I should expand the usecase more than the what has been stated, but then I decided to stick to plan, and deliver in time, than expand the scope. First achieve whats required and build on it if time permits. One such decision was adding mobile number and email address and having api for usecase 2 with mobile number and email id search criteria and update. 

3.  Adopted RAML implementation best practices as far as possible with consideration of time constraint. 

4.  For usecase 1, it was quite non-intutive to think of cache staleness as one of the approaches for periodical refresh of data. It took to me time to first come up with a possible approach and then identify which exact parameters would do the needful and how can i represent those parameters into the .raml file. 

5. For usecase 2, implementation of authentication is very important part, but decided to park it for future scope, as the client side requirement and implementation is not clear. Incase if the client performs role management at its end, and allows access to update and delete customer information to only those eligible, authentication on API is not necessary.

6. I have implementated only selective error code. 

7. I was looking for an opportunity to implement query parameters in existing .raml, but could not find relevant scenario.
