## Using Postman's Collection Runner:
**Iterating through collections using CSV data, using dynamic variables**

**Use Case**

Documentaion and "how-to" use Postman to loop over a collection of API calls.  Postman's own resources on the topic are found here [Postman Scripts Brancing & Loopin](https://learning.postman.com/docs/postman/scripts/branching-and-looping/).  Postman also provides an excellent Discourse forum for the [Postman community of users](https://community.postman.com/); I found this resource to be extremely helpful while learning how to use these features. 

**This guide will show you how to set up the collection runner in Postman with two use cases:** 
 - 🅰️ Setup and run a collection with a single request and a single URL variable
 - 🅱️ Setup and run a collection with two requests and use data from the first request in the request body of the second

**Problem**

**1)** Working on a project using the fleet management product: Fleetio - I needed to remove a very large (nearly 70,000) data set representing innaccurate service data, and ultimately replace it with new data.

> NOTE: Fleetio *does not* support a bulk .csv import to "update" or PATCH these service entries - therefore I needed to remove all of the bad data using their API, before being able to re-write the new (correct) data later on.

**2)** Using the software UI - it would take forever to delete these entries, even with account owner permissions.  The maximum bulk delete a user can perform w/in the UI is 200 entries at one time... so using the API is the clear choice here.

> NOTE: Fleetio settings <> in order to perform a delete action - the user's API token must also have permissions to delete the corresponding data. 

**3)** I turned to Postman's collection looping feature as a solution to this problem that needed to be solved on a tight timeline. 

**Resources**

 - Fleetio's API Documentation: https://developer.fleetio.com/docs/getting-started
 - Fleetio's API docs on Service Entries: https://developer.fleetio.com/docs/delete-service-entry
 - Additional docs on sevice entry line items: https://developer.fleetio.com/v2/docs/reference
 
 > NOTE: For this project I did not care about keeping particular service tasks w/in a service entry - we are going to remove and re-write the entire service w/ a new format.
