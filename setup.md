
We are going to utilize Postman's Collection Runner feature --> the core function that makes
everything happen is the postman.setNextRequest function. 

> postman.setNextRequest("Request Name");

while

> postman.setNextRequest(null);
will terminate the execution of the iterative loop. 

1. This first example is going to be a single, linear loop: deleting a large series of vehicle service entries
through the Fleetio API - which supports a {{id}} specific DELETE endpoint in their API. 
