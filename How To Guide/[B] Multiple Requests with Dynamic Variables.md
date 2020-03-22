# 🅱️ Multiple Requests with Dynamic Variables

This guide builds on the single request, single variable iteration [covered in my first guide, found here.](https://github.com/jwhorley/postman-iterate-data-collections/blob/master/How%20To%20Guide/%5BA%5D%20Collection%20&%20Variable%20Setup.md) 
Combining the power of Postman's variable syntax and the `postman.setNextRequest` function, you can do a lot of really powerful API-driven things automatically, and save yourself tons of time. 

### Use Case:
Like the other example, I'm going to be working in [Fleetio](https://www.fleetio.com/); I have a list of vehicles with various historical maintenance records. Some vehicles have more than one maintenance record, and no one maintenance record is the same. I'm going to create a service entry object for each corresponding vehicles and for each of the service vistis. Then I will write service line item(s) to the service entry object, and repeat this sequence for my entire maintenance list. 

```
//Simplified Text-Based Example: 

service_entry_1:
  at {{yyyy-mm-dd hh:mm}}, {{vehicle_1}} had the following service tasks performed
    service_tasks:
        {{sub_task_1}} 'oil_change' = {{service_task_1_id}}
        {{sub_task_2}} 'air_filter_replacement' = {{service_task_2_id}}
    descriptions:
        {{service_description_1}} = "Synthetic oil used, 3QT's"
        etc. 
    costs: 
        {{parts_costs_1}} = $X.00
        {{labor_costs_1}} = $Y.00
        {{parts_costs_2}} = $Z.00
        {{labor_costs_2}} = $Z.00
    tax: 
        etc.
    discounts: 
        etc.

```
Based on the published documentation for writing service data into Fleetio via their API - you need to have a **`service_entry`** id object *before* you can write any line item data to it.  Therefore, this is a perfect example of using Postman's collection runner. 
> Note: Fleetio has two versions of documentation related to service events. I'm going to refer to the v2 exclusively for anything API related on service task line items or service entries.  https://developer.fleetio.com/v2/docs/service_entries-1

### Setup your Environment Headers: 
If you don't have them from the last example, make sure you have your required authorization in place for your collection. 
For this example, here is what Fleetio requires: 
> Follow https://developer.fleetio.com/docs/getting-started

```
Authorization: Token [your_API_key]
Account-Token: [string from your account]
Content-type: application/json
```
The easiest way to do this w/in your workspace is to set these as environment variables in whatever Postman workspace you are saving this collection.

### Create a Collection:
Do this inside of your choosen environment. (Postman workspace setup and global variables not covered here but can be found here) https://www.youtube.com/watch?v=wArvaHYdw2I.  

Once you have your collection created, we'll need to add our requests. We are going to use two `POST` requests to create the service entry, and then create a service entry line item `POST` to the newly created service entry ID.
 - Here are my two URL's for each of these requests. I'm using an environment variable for my Fleetio base_url - this is not required, but **pay attention to include the {{id}} in the service_entry_line_item request**. We'll get into how this works further below when setting up the test scripts.
    - `https://{{base_url}}/api/v2/service_entries`
    - `https://{{base_url}}/api/v2/service_entries/{{id}}/service_entry_line_items`
    
### Setup your First Request Body (Create the Service Event):
For this example, I'm going to use the form-data layout vs the raw JSON format to better illustrate how I am mapping my key<>value pairs.  Using Postman's `{{variable}}` syntax, you'll need to type out the corresponding keys *(from Fleetio's documentation)*, to your data *(your csv column headers)*.  
> Note: Since these values aren't defined anywhere w/in Postman yet, they will appear red in the UI.  That is okay for now, they will be populated via the collection runner.  Orange collored variables are defined and will show the current, last, stored value if you hover over it. Here is [Postman's documentation on accessing variables.](https://learning.postman.com/docs/postman/variables-and-environments/variables/#accessing-variables)

Here is what my request body looks like. Each of these fields are not related to a specific service line item and can be written first when creating the `service_entry`.  There are more fields listed here than the minimum required by Fleetio when creating a service entry, this is just what I'm using.

<img src="/images/Postman-POST-requestbody.png" width="900" >

### Write Test Script - Parse Response Body
Now that our first request is set up, we can write the test script that runs after the request is sent in Postman.  This test can include whatever testing parameters you want.  The expected response for this example is going to be `201 = created`. 
We are going to create a variable, assign the JSON response it it and then parse that response to get the `id`.  This represents the `service_entry_id` that is required for the service entry line item creation (coming next). 
```
var jsonData = JSON.parse(responseBody);
pm.environment.set("id", jsonData["id"]);
```
Here is what my test script looks like: 
<img src="/images/Postman-TestScript_ID.png" width="900" >

### Setup your Second Request Body (Create the service line item data):
For this request, we will follow Fleetio's documentation and set up the request in the same form-data layout w/in Postman.
<img src="/images/Fleetio_serviceentrylineitem.png" width="750" >

Here is how my request looks: 
<img src="/images/Postman-POST-lineitems-request.png" width="900" >

### Write Test Script
<img src="/images/Postman-LineItemservice-Script.png" width="900" >


### Run the Collection
Now that both of the requests and test scripts are set up.  You can import your csv into the collection runner, and you're all set!.  For each of your entries, a service entry object will be created, then that ID is put into the request URL of the next request, writting new line item data to that service entry.  The collection runner will take care of the rest, running through your entire csv data set.  

**Important - this solution will always create one service entry line item, and then move on to the next service entry.** 
*So, if you have data where some service will have >= 2 line items, this example will not be sufficent for handling those use cases.*  There is another way to create the service entry line item when creating the service entry at the begining; however, it is not currently part of their online docs.  The format would use a series of additional key value pairs for as many line items as you would like to add.  Doing this purely in the request body isn't dynamic or automatic either, as the "required" setting for the fields shown above require a value for every `service_task_id`.


I'm currently working on solving a better solution, with the [help of the Postman community here](https://community.postman.com/t/iterating-a-list-of-users-in-postman/10976/13?u=j_who) - and I'll update this document once I have a workable solution.  Thanks for everyone in Postman and GitHub who's resources I used in learning this and putting together this guide of how I worked through my project!

------
#### Additional Resources: 
https://community.postman.com/t/how-to-loop-through-array-and-use-its-values-in-a-request/6771
 - Blog on conditional workflows - (w/ dog example): https://blog.postman.com/2016/03/23/conditional-workflows-in-postman/
 - Postman's Ramen example w/ great interactive data: https://documenter.getpostman.com/view/1559645/RVu4GVAs?version=latest#fa28e3b1-b535-4f42-9aaf-5d52501698f1
 - Looping example: https://blog.postman.com/2018/04/11/looping-through-a-data-file-in-the-postman-collection-runner/
 - Building Workflows: https://learning.postman.com/docs/postman/collection-runs/building-workflows/
 - Similar problem I found in Stackoverflow: https://stackoverflow.com/questions/47934314/iteration-through-json-with-multiple-api-calls-for-other-requests
 - Setting an environment variable for a subsequent request: https://community.postman.com/t/how-to-save-a-value-from-response-as-a-environment-variable/11000/5
