# One-click update of Data on React app

The problem we need to fix is the overdependence on developers for small changes in data used for our front-end. 

The original workflow:
* Developer downloads csv file from google spreadsheet
* Developer uploads csv onto database
* Developer runs a script that converts database into json files
* Developer puts json file onto front-end code
* Developer uploads json file to S3
* Developer invalidates cache on Cloudfront. 

Imagine doing this for every single data change. 

## Proposed User Story

* Non-developer edits database from the backend app
* Non-developer clicks one button on app


### Solution

* Used json files in /public folder in React app to store static data files: Originally we stored the data files as .js files, but this meant that the files would be compiled when built and uploaded to S3.
* Created new back-end: Python Django App gives you DB-editing ability for free 
* Added custom page with button onto Django Admin page 
* Set up button to call S3 API to upload JSON into /data on S3, and set up AWS Lambda to automatically invalidate when item is added to json 
