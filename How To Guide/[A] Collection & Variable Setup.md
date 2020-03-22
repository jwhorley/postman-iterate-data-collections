# 🅰️ Single Request w/ Single Variable Example
### Create Collection and Define Variables for the Request
We are going to utilize Postman's Collection Runner feature --> the core function that makes
everything happen is the **`postman.setNextRequest`** function. 

Our first use case is using a single variable from a data file w/ a single API request (DELETE), to remove a large amount of service entry data within a Fleetio account. Our CSV import will have the ID's of each of the objects we want to remove in a single column with a clear header row


### Setup Your Data: 
To follow along with this example, I'm going to use real ID's (which have been removed from my test Fleetio account), so they won't work if you are trying this with your own account-token and API key.  Here's what the csv data looks like - use it as a guide to format your data and modify the request parameters to fit your use case. 

[CSV IMAGE]

### Create a Collection:
Do this inside of your choosen environment.
(*Postman workspace setup and global variables not covered here but can be found here*) https://www.youtube.com/watch?v=wArvaHYdw2I

Once you have your collection created, we'll need to add the API call.  For this example, we are going to use a `DELETE` call to remove data at the /service_entry endpoint within Fleetio. 
This helpful [Postman Blog Post](https://blog.postman.com/2014/10/28/using-csv-and-json-files-in-the-postman-collection-runner/) has downloadable examples of both csv and json data you can use to practice along with additional sceenshots from Postman. 

### Setup your Environment Headers:
Follow https://developer.fleetio.com/docs/getting-started
```
Authorization: Token [your_API_key]
Account-Token: [string from your account]
Content-type: application/json
```

The easiest way to do this w/in your workspace is to set these as environment variables in whatever Postman workspace you are saving this collection. [Postman has great docs](https://learning.postman.com/docs/postman/variables-and-environments/variables/#variable-scopes) on the hierarchy of variables and the variable scope of each. 
Understanding variable scope is key to making sure your collection run works properly (which we'll get to next).

<img src="/images/var-scope.jpg" width="350" >

### Create the Delete Request:
We're going to use a single request - in this example a `DELETE` request for our collection runner.  If you need to create something using a `POST` your steps will be the same, just with a different request.

Here is what my request headers look like with the environment headers shown in `{{ }}'s`.
Note: Our delete request URL parameters require the value of the service event - for now you must put that data variable (shown in red) into the URL as shown below. 
```
https://secure.fleetio.com/api/v1/service_entries/{{id}}
```
Later our data file will reference this `{{id}}` when running through each iteration.

<img src="/images/Postman_Headers_example.png" width="900" >


### Setup a Test Script:
The test script is run **after** the request is sent; this is where we can test for expected responses and write to the console log to help with any troubleshooting or review of our results after we've run the collection.

For our test script, we expect a 204 "no content" response since we are deleting data.  Add this function into your test script to test that response: 

```
// Test for successful command
pm.test("Log results of the request:", function () { pm.response.to.have.status(204); });
```

You can also add additional messages to the console log using data from the request or response.  Here I'm writting to the console the id of the service entry that was just removed: 
```
console.log("Removed Service Entry :" + data.id);
```
Postman provides *a ton* of really great examples and documentation for writing test scripts: 
 - [Documenter: Intro to Writing Gests w/ Examples Link](https://documenter.getpostman.com/view/1559645/RzZFCGFR?version=latest&_ga=2.201325480.747007528.1584880174-1277279863.1583418097)
 - [DannyDainton All Things Postman GitHub Link](https://github.com/DannyDainton/All-Things-Postman/blob/master/Examples/07_creatingOurFirstTest.md) 🙏

Since we have a *single, linear loop* over a single reqeust the collection runner can just repeat that single request for however many rows we have in our CSV.  If `postman.setNextRequest()` is absent in a request, the collection runner defaults to linear execution and moves to the next request - which is okay for this use case.  Read more about branching and looping in the [Postman Learning Center here.](https://learning.postman.com/docs/postman/scripts/branching-and-looping/)

> Note: While looping over one request continously, considering wrapping `setNextRequest` in logic to ensure it doesn't run indefinitely, otherwise you'll have to force close the runner. 

If you had more than one request in your collection or wanted to run these in a sepecific order the `postman.setNextRequest("Request Name")` function is what you would want to use.

```
//tells postman the next request to action, "Remove Service"
postman.setNextRequest("Remove Service");

//will terminate the execution of the iterative loop. 
postman.setNextRequest(null);

```
Here is how my test script looks with all of the pieces together:

[POSTMAN SCRIPT IMAGE]

### Load the Collection Runner:
Details on starting a collection run can be found in Postman's [Learning Center article, here.](https://learning.postman.com/docs/postman/collection-runs/starting-a-collection-run/). 



Links to Use Later in the Doc: 
 - https://community.postman.com/t/how-to-loop-through-array-and-use-its-values-in-a-request/6771
 - blog on conditional workflows - (w/ dog example): https://blog.postman.com/2016/03/23/conditional-workflows-in-postman/
 - Ramen example: https://documenter.getpostman.com/view/1559645/RVu4GVAs?version=latest#fa28e3b1-b535-4f42-9aaf-5d52501698f1
 - Looping example: https://blog.postman.com/2018/04/11/looping-through-a-data-file-in-the-postman-collection-runner/
 - Using variables: https://learning.postman.com/docs/postman/variables-and-environments/variables/#using-data-variables
 - Building Workflows: https://learning.postman.com/docs/postman/collection-runs/building-workflows/
 - similar problem i found in Stackoverflow: https://stackoverflow.com/questions/47934314/iteration-through-json-with-multiple-api-calls-for-other-requests
 - **This is the link to explain setting an environment variable for a subsequent request:** https://community.postman.com/t/how-to-save-a-value-from-response-as-a-environment-variable/11000/5
 
