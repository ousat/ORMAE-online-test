

# Real Time Vehicle Tracking APP

## Design
    ![rough flow](https://raw.githubusercontent.com/ousat/ORMAE-online-test/master/images/arch.png)

- The driver of the vehicles downloads a mobile app, registers the vehicle

- Registered vehicles send GPS data via websockets every 200 milliseconds. 
- upon no or slow connectivity , the time is increased to 500 or 1 second to upto 5 seconds


- Vehicle information is stored in Database, 
Vehicle ID and GPS position of the vehicle is stored and updated in Redis
    - vehicle id - key
    - GPS data - value

- If Vehicle is not moving for more than 10 seconds , the app sends a request to remove vehicle entry

- All the entries in Redis are active Vehicles with most recent GPS data 

- Dashboard fetches active vehicles and position from Redis and Vehicle/driver data from DB
- upon selecting a particlar vehicle, that vehicle's info is fetched from DB

## Database
    - 2 tables( collections if primary db is mongodb)
    - ![rough flow](https://raw.githubusercontent.com/ousat/ORMAE-online-test/master/images/database.png)
    


## Technologies used
1. Google Maps API / any third party GPS provider API - to get current location at the vehicle
2. websockets - for real time update 
3. Redis - for storing Active vehicle data
4. A single page dashboard application with local authentication module  
5. Tracking App for vehicle , A mobile app, installed in driver's phone
	- as soon as driver installs the app , driver details and vehicle details are updated



## Deployment strategies

- App launch/updates for vehicles are manual
- A single page dashboard application can be hosted via an EC2 instance or EKS 
- Serverless YAML file that defines Amazon Redis instance, API gateway to create endpoints and lambda functions with HTTP trigger for APIs - registering vehicle, adding/updating vehicle entry, removing vehicle entry, fetch all vehicles( pagination rule set by dashboard)

- on merge with master 
    - single page app updates the ci/cd runner on EC2 instance, if EKS new docker image is built and pushed and the EKS service pulls the updated docker image
    - serverless script is built and respective lambda functions and instances updated with new versions
    
## CI/CD

- the project will have three seperate projects
- mobile app for vehicles which will be published on google play store / apple app store. Tests and Launch in this project will be manual and not via CI/CD.

- Dashboard app , standard web tests along with unit tests

- serverless APIs -  unit tests, response tests, connectivity tests 




