## Setup
✏️ We are going to utilize Postman's Collection Runner feature --> the core function that makes
everything happen is the **postman.setNextRequest** function. 

> postman.setNextRequest("Request Name");

while

> postman.setNextRequest(null);
will terminate the execution of the iterative loop. 

This first example is going to be a *single, linear loop*; we'll be deleting a large series of vehicle service entries
through the Fleetio API - which supports a {{id}} specific DELETE endpoint in their API. 

### First: Create a Collection
Do this inside of your choosen environment - (environment setup and variables not covered here but can be found here: https://www.youtube.com/watch?v=wArvaHYdw2I

