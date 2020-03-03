# postman:
## Iterating through a collection using Collection Runnner and a data file

**Use Case**

Documentaion and "how-to" use Postman to loop over a collection of API calls.  Postman's own resources on the topic are found here: https://learning.postman.com/docs/postman/scripts/branching-and-looping/

**Problem**

1. Working in Getaround's fleet management too, Fleetio - I needed to remove a very large (nearly 70,000) service entry data points that were inaccurate.  
Fleetio does not support a bulk .csv import to "update" / PATCH these entries - therefore I needed to remove all of the bad data using their API, before being able to re-write the new (correct) data later on.

2. Using the software UI - it would take forever to delete these entries, even with account owner permissions.  NOTE: in order to perform this delete action - the user's API token must also have permissions to delete service entires.  The maximum bulk delete a user can perform w/in the UI is 200 entries at one time...

3. I turned to Postman's collection looping feature as a solution to this problem that needed to be solved on a tight timeline. 

**Resources**

 - Fleetio's API Documentation: https://developer.fleetio.com/docs/getting-started
 - Fleetio's API docs on Service Entries: https://developer.fleetio.com/docs/delete-service-entry
 - Additional docs on sevice entry line items: https://developer.fleetio.com/v2/docs/reference
 *NOTE: For this project I did not care about keeping particular service tasks w/in a service entry - we are going to remove and re-write the entire service w/ a new format.*
