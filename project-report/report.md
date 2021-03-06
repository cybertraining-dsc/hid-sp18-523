# Managing item sales transactions and predicting quantity using REST API

| Ritesh Tandon, Arijit Sinha
| ritandon@iu.edu, arisinha@iu.edu
| Indiana University Bloomington
| hid: hid-sp18-523,  hid-sp18-520
| github: [:cloud:](https://github.com/cloudmesh-community/hid-sp18-523/tree/master/project-report/report.md)
| code: [:cloud:](https://github.com/cloudmesh-community/hid-sp18-523/tree/master/project-code)

---

Keywords: docker, Scikit, Open API, REST, Swagger, Linear Regression

---

## Abstract

In current world, data is getting generated and stored with different 
storage systems. We need to use this data for better understanding, 
analysis and estimate future scenarios with certain probability. 
There are many algorithms which have been developed and implemented for 
providing better accuracy on future scenarios. This project comprises
of two parts on one hand it lets users manage items sales transactions
 and on the other hand it lets users predict sales quantity of given
 item of given outlet. Thus helping corporate store manage their inventory 
 stock to meet their retail outlet demands.

## Introduction

Bigmart is big retail outlet chain in Europe. This project aims to help
manage their inventory stock to fulfill demands  of their retail outlets
using state of the art cutting edge cloud based technology. System exposes 
inventory related information through OPEN API that can be deployed on cloud.
This allows system to scale seamlessly if organization expands to open new 
outlets or if sales transaction grows exponentially. On premise applications
running in outlet store can access these API enpoint regardless of language
they are written in. This API also provides future sales prediction of specific 
item in given outlet based on historical transactions. This prediction information
helps organization to manage their stock efficiently eventually reducing loss that 
occur either through over stocking or losing sales opportunity because of non 
availability of items.


## Scope of work

To develop Open API that allows corporate users to upload historical sales 
transactions data for all outlets in batch mode using file upload.
Store outlet users should also be able to insert invidividual 
item sales transactions for given store outlet. This Open API will allow
store outlet users to query and view item details by passing item idendifier 
and store outlet identifier. It should have feature that allows store outlet 
users to query and view sales information  by passing item idendifier and 
store outlet identifier. In addition it should  allows corporate users to upload 
details of items they wish to get predictive quantity.


## Technology Stack

API is defined with Open API specification. Swagger codegen is used to 
build sever side code stubs and client side SDK. Swagger UI is used to
test API endpoints. Python flask micro web framework. Python pandas
, numpy is used for data cleansing and pre processing. Python sklearn
preprocessing package is used for treating and imputing missing values.
Python linear_model package is used to create linear model for prediction.
Python Jupyter notebook is used for data exploration and data visualization.

We have acquired Bigmart dataset from Kaggle. 
We have 14204 instances and 13 attributes in dataset, which will be
spitted into Training and Test Data set. This dataset has following
attributes

* Item Fat Content
* Item Identifier
* Item MRP
* Item Outlet Sales
* Item Type
* Item Visibility
* Item Weight
* Outlet Establishment Year
* Outlet Identifier
* Outlet Location Type
* Outlet Size
* Outlet Type
* source

[@kaggleds].

## Dataset Details

* test dataset  - https://github.com/cloudmesh-community/hid-sp18-523/blob/master/project-code/test.csv
* train dataset - https://github.com/cloudmesh-community/hid-sp18-523/blob/master/project-code/train.csv

It has following 12 attributes with continuous and categorical values

* Item Fat Content has 5 unique values
* Item Identifier has 1559 unique values
* Item MRP has 8052 unique values
* Item Outlet Sales has 3494 unique values
* Item Type has 16 unique values
* Item Visibility has 13006 unique values
* Item Weight has 416 unique values
* Outlet Establishment Year has 9unique values
* Outlet Identifier has 10 unique values
* Outlet Location Type has 3 unique values
* Outlet Size has 4 unique values
* Outlet Type has 4 unique values

train dataset is used by corporate users to store historical sales 
transactions of items of different outlet stores which can then be 
queried by outlet users to view their transactions. This dataset is 
also used by implementation of prediction operation to train the 
model.

test dataset is used by corporate users to pass it to prediction
endpoint of API in order to get estimated future sales quantity.

## Data Visualization

The histogram in +@fig:HistogramofImportantAttributes shows 
distribution of data of different variables from dataset.
These are mportant feature influencing prediction of item
sales quantity exposed through prediction API enspoint.

![Histogram of Important Attributes](images/HistrogramofImpAttributes.png){#fig:HistogramofImportantAttributes}

In +@fig:CorrelationMatrixbetweenVariables plot shows 
corelation between variables in dataset.
The graph displays features which are higly correlated in 
darker color.

![Correlation Matrix between Variables](images/Correlation.png){#fig:CorrelationMatrixbetweenVariables}


## Data Preprocessing

Not all historical outlet store data ( train.csv ) transactions 
has information information of all required attributes. We had to
treat these missing values in order for predicting item sales quantity.
We observed that there were 2439 entries of item weight that were 
missing. This needs to be filled in order to improve accuracy of
prediction. We decided to fill missing values with mean as that improved
accuracy of our prediction mode. We have also observed that there were
4016 records that were missing values of outlet size. Since outlet size
is categorical data, we decided to replace these values with mode. Outlet
size with most number of records is used to fill in these missing values.


## Azure ML Studio

Azure ML studio provides the GUI interface for creating the Machine
Learning Train models and Predictions. It provides a provision to
integrate with Azure Cloud and expose Web Service

## Train Model with Azure

Created on Azure ML Studio, 3 Learning Algorithms used

* Boosted Decision Tree
* Linear Regression
* HyperTuned Boosted Decision Tree

As seen in results  +@fig:RMSEComparisionResults and +@fig:HypertunedAlgorithmRMSEComparision 
regarding RMSE result scores, Hyper-tuned Boosted Decision 
Tree has provided better results.

![RMSE Comparision Results](images/RMSEComparison.png){#fig:RMSEComparisionResults}

![Hypertuned Algorithm RMSE Comparision](images/RMSEComparisionBetweenHypertune.png){#fig:HypertunedAlgorithmRMSEComparision}

## Predictive Model

Update Trained model with Test dataset for predicting Item
Outlet Sales data. Verified and updated data cleaning process which
we have implemented for Train dataset. After converting categorical
data with indicators, we can apply the trained model.

Created predictive model using the above Hyper-tuned Boosted Decision
Tree.

From the score function, have extracted only 2 columns

* Item Identifer
* Item Outlet Sales

In +@fig:OutputofPredictionfromModels shows WebService
input and WebService Output using the selected model on
the dataset with predicted value.

![Output of Prediction from Models](images/PredictionOutputfromModel.png){#fig:OutputofPredictionfromModels}

## Web Service Deployment

Once the Prediction model has been executed successfully, it can be
deployed as web service from Azure ML Studio.

+@fig:AzureMLStudioWebserviceDeploy shows the generated
API key, which will be used for Azure Cloud deployment.

![Azure MLStudio Webservice Deploy](images/Webservicedeploy.png){#fig:AzureMLStudioWebserviceDeploy}

It will provide an option to Test web service locally

* Click on Test button enabled at the bottom of the screen
* Download the CSV file from the tool to test the Web API with
    prediction model.

## Azure Cloud deployment

Once Web Service is created locally, It creates a hyper link
with name of the web service. On clicking this hyperlink 
service dashboard opens up that let the develop set up and 
configure web service on Azure cloud for consumption as 
shown in  +@fig:WebServiceConfigrationDetails. 

It provides test tab on the dashboard where we can provide
inputs and get the prediction values given by model after 
clicking on Test Request response button.

This step will assure that, the web services are working as expected.

![WebService Configration Details](images/Webserviceconf.png){#fig:WebServiceConfigrationDetails}

After clicking consume tab from Dashboard, It will display option for
Response Request Web Template link.

Copy the request response link generated on the page.

Once clicked on the link, it will redirect to Azure cloud configuration
using Response Request Web application.

Azure ML Request, Response Service Web App In Azure cloud, it need to
created as

* Create the request response service web app
* Create Resource Group
* Add Model Management services
* Click on the URL link generated under resource group
* Update the Settings with API POST URL
* Update the API key generated from Web service from Azure ML studio.


Entered the values used to for testing locally, from the 
+@fig:WebServiceResult, we see that amount should matched
which confirms service is functioning as expected for
predicting values.

![WebService Result](images/WebService.png){#fig:WebServiceResult}

Batch Mode for Web Service Execution Download the CSV generated from
Azure ML Studio

* Open the CSV, it will be open with Web API built in
* Use Sample data link on the API
* Select the range of columns and provide as input to API
* Select the cell from where the prediction values needs to be
  displayed.
* Click on Prediction button. It will generate the prediction values
  for all the selected Input entries with Item Identifiers.

+@fig:BuiltAPIonCSVFile shows the API got created on the CSV format
file, for generating the predicted values using the features from
the dataset. These predicted values matches with the values 
generated from the local or webservice created.

![Built API on CSV File](images/csvscreenshot.png){#fig:BuiltAPIonCSVFile}

## Drawback of using Azure ML Studio

Azure ML studio provides easy to use drag and drop objects for
all steps (from model intialization to deployment) such as 
data preprocessing, training model, fitting model, evaluating 
model, tuning model and prediction. It is every easy to use and
handy tool for data scientists and data engineer. However, it has 
a drawback; developers are not able to generate the code that is 
generated at the backend for further experimentation and testing.

## Azure with Notebook Instance

Notebook instance of Azure ML Studio was used to execute python
code file for benchmarking. Code containing data preprocessing, 
training model, fitting model, evaluating model, tuning model and 
prediction steps was executed on Azure notebook instance. 
It took 15.2 ms for overall batch prediction using notebook. 
Computation time using model saved on disk was recorded  as 
1.95 mili seconds as shown.

### Code

```python
Predict on original Test Set using Random forest model with Hypertune

%%time
overallprediction=clfgrdmof.predict(testr[predictors])
CPU times: user 15 ms, sys: 761 µs, total: 15.7 ms
Wall time: 15.2 ms

print(overallprediction)
[1648.57907925 1413.2015807  700.49019691  ... 11922.98505294 3487.44068317
 1308.81770146]

import pickle
filename = 'finalized_model.pkl'
pickle.dump(clfgrdmof, open(filename, 'wb'))

 
# load the model from disk
loaded_model = pickle.load(open(filename, 'rb'))
Test1 = loaded_model.predict(testr[predictors])

%%time
print(Test1)
[1648.57907925 1413.2015807 700.49019691  ... 1922.98505294 3487.44068317
 1308.81770146]
CPU times: user 1.68 ms, sys: 277 ms, total: 1.96 ms
Wall time: 1.95 ms
```

## AWS with Notebook Instance

AWS Sagemaker service was used to benchmark performance of
model on AWS cloud. Python notebook file containing code for
data preprocessing, training model, fitting model, evaluating 
model, tuning model and prediction was executed on AWS notebook
instance. 
Execution time of prediction was captured for comparison.
It took 8.2 ms for overall batch prediction using notebook. 
Computation time using model saved on disk was recorded  as 
371 micro secs.

### Code

```python
Predict on original Test Set using Random forest model with Hypertune

%%time
overallprediction=clfgrdmof.predict(testr[predictors])
CPU times: user 12 ms, sys: 0 ns, total: 12 ms
Wall time: 8.2 ms

print(overallprediction)
[1610.61752003 1401.48146969  578.3748237  ... 1854.50378336 3521.16741962
 1286.86433971]

import pickle
filename = 'finalized_model.pkl'
pickle.dump(clfgrdmof, open(filename, 'wb'))

 
# load the model from disk
loaded_model = pickle.load(open(filename, 'rb'))
Test1 = loaded_model.predict(testr[predictors])

%%time
print(Test1)
[1610.61752003 1401.48146969  578.3748237  ... 1854.50378336 3521.16741962
 1286.86433971]
CPU times: user 0 ns, sys: 0 ns, total: 0 ns
Wall time: 371 µs
```

## LocalMachine Instance

Python notebook file was executed on local instance for comparing 
performance with cloud. Data preprocessing, training, fitting and 
prediction steps were performed on local python instance.
Start time and end time was recorded and execution time was calculated.
It was observed that execution and prediction took more time compared
to execution time on cloud. It took approximately 10 minutes on local
instance.

### Local Execution - Prediction Time

```python
Local Start Time: 
2018-12-08 01:39:40.654383
Local End Time: 
2018-12-08 01:39:40.684909
[1710.00908356 1425.18637329  566.05070462 ... 1825.71847362 3565.54132359
 1278.2714367 ]
[1710.00908356 1425.18637329  566.05070462 ... 1825.71847362 3565.54132359
 1278.2714367 ]
[1710.00908356 1425.18637329  566.05070462 ... 1825.71847362 3565.54132359
 1278.2714367 ]
```

## Open API

REST Open API was created using Swagger codegen for storing and managing 
sales transactions that occurs in different outlet stores.  Other than
managing transactions, API also predicts item sales quantity for given batch
of items using linear regression method.

## Open API endpoints

![API Enpoints](images/api-endpoints.png){#fig:APIRndpoints}



#### Open API Specification

```
swagger: "2.0"
info:
 version: "0.0.1"
 title: "Item Sales API"
 description: "This API is designed to provide historical data of sales 
 transactions that occured in mutiple outlets spread across Europe of 
 big retail giant. API provides sales information of given items and 
 oulets to consumers. This eventually helps them plan their inventory 
 and place order quantity. API also predicts sales of items based on their 
 weight, fat content, visibility, price and type"
 termsOfService: "http://swagger.io/terms/"
 contact:
  name: "Ritesh Tandon, IU, MS Data Science - 2018"
 license:
  name: "Apache"
host: "localhost:8080"
basePath: "/cloudmesh"
schemes:
 - "http"
consumes :
 - "application/json"
produces :
 - "application/json"
paths:
 /prediction:
   get:
    summary : "Predicts sales of items based on their weight, fat content, 
    visibility, price and type. This API endpoint returns predicted sales 
    quantity in batch mode."
    description : "Predicts sales of items based on their weight, fat content, 
    visibility, price and type. This API endpoint returns predicted sales 
    quantity in batch mode."
    produces:
     - "application/json"
    responses:
     "200":
       description: "Successfully fetched predicted sales of items based on 
       their weight, fat content, visibility, price and type in batch mode "

       schema:
         $ref: "#/definitions/Prediction"


 /item/{Item_Id}:
    get:
      operationId: item.getitemsales
      summary: "This API endpoint returns sales transactions of an itemid that 
      is passed as path parameter. It returns item id, outlet store and sales."
      description: "This API endpoint returns sales transactions of an itemid 
      that is passed as path parameter. It returns item id, outlet store and 
      sales."
      parameters:
        - name: Item_Id
          in: path
          description: Item Id
          type: string
          required: True
      responses:
        200:
          description: Successfully fetched Sale of given item id in all 
          outlet store.
          schema:
            $ref: "#/definitions/ItemSale"
 /sale:
    get:
      operationId: sale.getitemoutletsales
      summary: "This API endpoint returns sales quantity of an itemid for 
      given outlet store that is passed as query string parameter."
      description: "This API endpoint returns sales quantity of an itemid 
      for given outlet store that is passed as query string parameter."
      parameters:
        - name: item_id
          in: query
          description: Item Id passed in query string
          type: string
          required: True
        - name: outlet_code
          in: query
          description: outlet code passed in query string
          type: string
          required: True
      responses:
        200:
          description: Successfully fetched Sale of given item id in given 
          outlet
          schema:
            $ref: "#/definitions/ItemOutletSale"
 /item:
    get:
      operationId: item.getitemdetails
      summary: "This API endpoint returns all details of an itemid for given 
      outlet store that is passed as query string parameter."
      description: "This API endpoint returns all details of an itemid for 
      given outlet store that is passed as query string parameter."
      parameters:
        - name: item_id
          in: query
          description: Item Id passed in query string
          type: string
          required: True
        - name: outlet_id
          in: query
          description: Outlet Identifier passed in query string
          type: string
          required: True
      responses:
        200:
          description: Successfully fetched Sale of given item id in given 
          outlet
          schema:
            $ref: "#/definitions/ItemDetail"
    put:
      operationId: item.additem
      summary: "This API endpoint add item details such as such as weight, 
      MRP, visibility index, outlet type, outlet identifier, year, sales 
      quantity etc."
      description: "This API endpoint add item details such as such as weight, 
      MRP, visibility index, outlet type, outlet identifier, year, sales 
      quantity etc."
      parameters:
        - name: Item_Identifier
          in: query
          description: Item Id passed in query string
          type: string
          required: True
        - name: Item_Weight
          in: query
          description: item weight passed in query string
          type: string
          required: True
        - name: Item_Fat_Content
          in: query
          description: Item fat content passed in query string
          type: string
          required: True
        - name: Item_Visibility
          in: query
          description: outlet visibility passed in query string
          type: string
          required: True
        - name: Item_Type
          in: query
          description: Item type passed in query string
          type: string
          required: True
        - name: Item_MRP
          in: query
          description: Item MRP passed in query string
          type: string
          required: True
        - name: Outlet_Identifier
          in: query
          description: Outlet Identfier passed in query string
          type: string
          required: True
        - name: Outlet_Establishment_Year
          in: query
          description: outlet year passed in query string
          type: string
          required: True
        - name: Outlet_Size
          in: query
          description: outlet size passed in query string
          type: string
          required: True
        - name: Outlet_Location_Type
          in: query
          description: outlet location type passed in query string
          type: string
          required: True
        - name: Outlet_Type
          in: query
          description: outlet type passed in query string
          type: string
          required: True
        - name: Item_Outlet_Sales
          in: query
          description: item outlet sales passed in query string
          type: string
          required: True
      responses:
        200:
          description: Successfully added item details
          schema:
            $ref: "#/definitions/Item"
 /data/train:
    post:
      operationId: train.uploadtrainfile
      summary: "This API endpoint can be used to upload train csv file. 
      This csv file is used to train linear regression model for prediction."
      description: "This API endpoint can be used to upload train csv file. 
      This csv file is used to train linear regression model for prediction."
      consumes:
        - multipart/form-data
      parameters:
        - name: uptrainfile
          in: formData
          description: file to upload
          type: file
          required: True
      responses:
        200:
          description: Successfully uploaded file
          schema:
            $ref: "#/definitions/DataTrain"
 /data/test:
    post:
      operationId: test.uploadtestfile
      summary: "This API endpoint can be used to upload test csv file. 
      prediction quantity of items listed in this csv file can be obtained 
      by calling api prediction endpoint."
      description: "This API endpoint can be used to upload test csv file. 
      prediction quantity of items listed in this csv file can be obtained 
      by calling api prediction endpoint."
      consumes:
        - multipart/form-data
      parameters:
        - name: uptestfile
          in: formData
          description: file to upload
          type: file
          required: True
      responses:
        200:
          description: Successfully uploaded file
          schema:
            $ref: "#/definitions/DataTest"
definitions:
 Prediction:
  type: "object"
  required :
   - "model"
  properties:
   model:
    type: "string"
 ItemSale:
  type: "object"
  required :
   - "model"
  properties:
   model:
    type: "string"
 ItemOutletSale:
  type: "object"
  required :
   - "model"
  properties:
   model:
    type: "string"
 DataTrain:
  type: "string"
  format: binary
  required :
   - "model"
  properties:
   model:
    type: "string"
 DataTest:
  type: "string"
  format: binary
  required :
   - "model"
  properties:
   model:
    type: "string"
 Item:
  type: "string"
  format: binary
  required :
   - "model"
  properties:
   model:
    type: "string"
 ItemDetail:
  type: "object"
  required :
   - "model"
  properties:
   model:
    type: "string"


```


## Code Reproducing steps

Environment - Ubuntu , python


#### Testing Service Locally

step 1 - git clone https://github.com/cloudmesh-community/hid-sp18-523/tree/master/project-code

step 2 - cd project-code

step 3 - Run command

```
make service
```
This will download swagger codegen client jar. Move default_controller.py, 
dataset files ( train.csv and test.csv ) in respective folder.

step 3 - Run command

```
make run
```

This will run the REST API service. 
Keep this terminal window open. Do not close it.

step 4 - Open new terminal 

step 5 - cd project-code

step 6 - Run command to load train data using /data/train
         API end point

```
~/project-code$ curl -X POST --header 'Content-Type: multipart/form-data' 
--header 'Accept: application/json' {"type":"formData"} 
-F 'uptrainfile=@train.csv' 'http://localhost:8080/cloudmesh/data/train'
```

successfull result indicate that file has been uploaded 
successfully

```
~/project-code$ curl -X POST --header 
'Content-Type: multipart/form-data' 
--header 'Accept: application/json' {"type":"formData"} 
-F 'uptrainfile=@train.csv' 'http://localhost:8080/cloudmesh/data/train'
curl: (52) Empty reply from server
{
  "model": [
    "successful!!"
  ]
}
```

step 7 - Insert new item with all its details using /item PUT endpoint 

```
curl -X PUT --header 'Content-Type: application/json' --header 
'Accept: application/json' 'http://localhost:8080/cloudmesh/item
?Item_Identifier=RRR05&Item_Weight=5&Item_Fat_Content=LOW
&Item_Visibility=0.01&Item_Type=Dairy&Item_MRP=72&Outlet_Identifier=OUT111
&Outlet_Establishment_Year=2018&Outlet_Size=Medium
&Outlet_Location_Type=Tier%201&Outlet_Type=Supermarket&Item_Outlet_Sales=2400'
```

successfull result indicate that new item details have been inserted with
all details. This data can further be used for querying and predictive
modeling.

```
{
  "model": [
    "successful!!"
  ]
}
```

step 8 - Get item sale information by passing item identifier as path
parameter to Open API endpoint

```
~/project-code$ curl -H "Content-Type: application/json" 
http://localhost:8080/cloudmesh/item/FDA15
```

successfull result will show item sale quantity along with store 
identifier

```
~/project-code$ curl -H "Content-Type: application/json" 
http://localhost:8080/cloudmesh/item/FDA15
{
  "model": [
    {
      "Item_Outlet_Sales": {
        "FDA15": 5976.2208
      }, 
      "Outlet_Identifier": {
        "FDA15": "OUT017"
      }
    }
  ]
}

```

step 9 - Get all details of item for given outlet by passing 
item id and outlet id as query string to /item
API end point

```
~/project-code$ curl -H "Content-Type: application/json" 
'http://localhost:8080/cloudmesh/item?item_id=FDA15&outlet_id=OUT049'
```

successfull result will display details of given
item and outlet store

```
~/project-code$ curl -H "Content-Type: application/json" 
'http://localhost:8080/cloudmesh/item?item_id=FDA15&outlet_id=OUT049'
{
  "model": [
    "{\"Item_Identifier\":{\"[\"FDA15\",\"OUT049\"]\":\"FDA15\"},
    \"Item_Weight\":{\"[\"FDA15\",\"OUT049\"]\":9.3},
    \"Item_Fat_Content\":{\"[\"FDA15\",\"OUT049\"]\":\"Low Fat\"},
    \"Item_Visibility\":{\"[\"FDA15\",\"OUT049\"]\":0.016047301},
    \"Item_Type\":{\"[\"FDA15\",\"OUT049\"]\":\"Dairy\"},
    \"Item_MRP\":{\"[\"FDA15\",\"OUT049\"]\":249.8092},
    \"Outlet_Identifier\":{\"[\"FDA15\",\"OUT049\"]\":\"OUT049\"},
    \"Outlet_Establishment_Year\":{\"[\"FDA15\",\"OUT049\"]\":1999},
    \"Outlet_Size\":{\"[\"FDA15\",\"OUT049\"]\":\"Medium\"},
    \"Outlet_Location_Type\":{\"[\"FDA15\",\"OUT049\"]\":\"Tier 1\"},
    \"Outlet_Type\":{\"[\"FDA15\",\"OUT049\"]\":\"Supermarket Type1\"},
    \"Item_Outlet_Sales\":{\"[\"FDA15\",\"OUT049\"]\":3735.138}}"
  ]
}
```


step 10 - Get Sale of item by calling /sale API endpoint for given outlet 
by passing item id and outlet code as query string parameter

```
~/project-code$ curl -H "Content-Type: application/json" 
'http://localhost:8080/cloudmesh/sale?item_id=FDA15&outlet_code=OUT049'
```

successfull result will just display sale quantity information 

```
~/project-code$ curl 
-H "Content-Type: application/json" 
'http://localhost:8080/cloudmesh/sale?item_id=FDA15&outlet_code=OUT049'
{
  "model": [
    [
      3735.138
    ]
  ]
}

```

step 11 - Run command to load test data using /data/test
         API end point
```
~/project-code$ curl -X POST --header 
'Content-Type: multipart/form-data' --header 
'Accept: application/json' {"type":"formData"} 
-F 'uptestfile=@test.csv' 'http://localhost:8080/cloudmesh/data/test'
```

successfull message result will indicate that test file has been 
uploaded 


```
~/project-code$ curl -X POST --header 
'Content-Type: multipart/form-data' --header 
'Accept: application/json' {"type":"formData"} 
-F 'uptestfile=@test.csv' 'http://localhost:8080/cloudmesh/data/test'
curl: (7) Couldn't connect to server
{
  "model": [
    "successful!!"
  ]
}
```


step 12 - Run command to get prediction of sales quantity of items
listed in test data file by calling /prediction API end point


```
~/project-code$ curl -H 
"Content-Type: application/json" http://localhost:8080/cloudmesh/prediction
```

successfull result will display predictive quantity of test data items
using linear regression method.

```
~/project-code$ curl 
-H "Content-Type: application/json" http://localhost:8080/cloudmesh/prediction
{
  "model": [
    [
      1857.1406639820989, 
      1563.2871665661592, 
      1834.231027551627, 
      2581.450544775922, 
      5159.461138379451, 
      1919.533023840407, 
      627.6994262925587, 
      2803.1335223421875, 
      1508.992328153954, 
      3072.745089319022, 
      1999.6589439699842, 
      1327.040653005452, 
      1816.3873392549926, 
      2056.702717158843, 
      912.9796569607081, 
      2537.4069529934063, 
      3082.8856104726055, 
      2763.159286304049, 
      3204.078913003401, 
      1110.070131269718, 
      2810.2681164220576, 
      3846.3743818872504, 
      826.7525706134691, 
      342.6092963525098, 
      3006.7573564539343, 
      1414.1059565447613, 
      924.7275399636796, 
      2529.0310769705984, 
      3823.1790049697015, 
      2022.4847250844673
    ]
  ]
}

```


## Performance Comparison

Performance comparision was done for executing python
notebook on three different environment . Two different 
clouds ( Azure and AWS ) and local.


Environment  | Description       |  Elapsed Time
-------------|-------------------|----------------
Azure Cloud  | Notebook Instance |  15.2 ms
AWS Cloud    | Notebook Instance |  8.2 ms
Local        | Python Instance   |  0.03 sec

From above comparison we note that AWS cloud is faster 
giving performance of 0.0082 seconds whereas prediction on 
Azure has took 0.0152 seconds. lowest being the local environment 
with 0.03 second.

## Conclusion

Linear Regression, Boosted Decision and Hypertuned Boosted Decision
models were implemented in python notebook code.  

Best model was decided based on accuracy and that was used for prediction
of sales quantity of items across store outlets which helps in inventory
and stock management.

Model was  deployed on Azure Cloud, AWS Cloud and Local machine for 
predicting prices. Docker image was created for reproducing. 
Performance was benchmarked on different cloud and local environment. 
On comparison we observed that prediction performance
was best on AWS cloud. 


## Acknowledgement 

Authors would like to thank Dr. Gregor von Laszewski for his support
and suggestions to write this paper.

## Workbreakdown

Arijit and Ritesh worked on data processing,Data exploration and designing 
the Machine learning Algorithms. We both have brainstromed on visualizing 
data. Arijit worked on Azure cloud and creating web service and Ritesh 
worked on creating REST Open API using linear regression methodand tested 
on local machine through Swagger UI. We both worked on comparing performance 
python notebook code that was executed on two clouds for benchmark benchmarks. 

