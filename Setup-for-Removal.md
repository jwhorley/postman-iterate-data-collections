# Setup
✏️ We are going to utilize Postman's Collection Runner feature --> the core function that makes
everything happen is the **postman.setNextRequest** function. 

## First use case is using a single variable from a data file w/ a single API request (DELETE), to remove a large amount of service entry data within a Fleetio account.

**Required commands for our test script:**

This first example is going to be a *single, linear loop*; we'll be deleting a large series of vehicle service entries
through the Fleetio API - which supports a {{id}} specific DELETE endpoint in their API. 

`postman.setNextRequest("Request Name");`

while

`postman.setNextRequest(null);`

will terminate the execution of the iterative loop. 

### First: Create a Collection
Do this inside of your choosen environment.
(*Postman workspace setup and global variables not covered here but can be found here*) https://www.youtube.com/watch?v=wArvaHYdw2I

Once you have your collection created, we'll need to add the API call.  We are going to use a `DELETE` call to remove data at the /service_entry endpoint within Fleetio. 

**Setup your Headers:**
Follow https://developer.fleetio.com/docs/getting-started
```
Authorization: Token [your_API_key]
Account-Token: [string from your account]
Content-type: application/json
```
URL: https://secure.fleetio.com/api/v1/service_entries/{{id}}

Links to Use Later in the Doc: 
 - https://community.postman.com/t/how-to-loop-through-array-and-use-its-values-in-a-request/6771
 - blog on conditional workflows - (w/ dog example): https://blog.postman.com/2016/03/23/conditional-workflows-in-postman/
 - MAIN: https://blog.postman.com/2014/10/28/using-csv-and-json-files-in-the-postman-collection-runner/
 and this one: https://learning.postman.com/docs/postman/scripts/branching-and-looping/
 - Ramen example: https://documenter.getpostman.com/view/1559645/RVu4GVAs?version=latest#fa28e3b1-b535-4f42-9aaf-5d52501698f1
 - Looping example: https://blog.postman.com/2018/04/11/looping-through-a-data-file-in-the-postman-collection-runner/
 - Using variables: https://learning.postman.com/docs/postman/variables-and-environments/variables/#using-data-variables
 - Building Workflows: https://learning.postman.com/docs/postman/collection-runs/building-workflows/
 - similar problem i found in Stackoverflow: https://stackoverflow.com/questions/47934314/iteration-through-json-with-multiple-api-calls-for-other-requests
 - **This is the link to explain setting an environment variable for a subsequent request:** https://community.postman.com/t/how-to-save-a-value-from-response-as-a-environment-variable/11000/5
 