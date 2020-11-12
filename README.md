# Roche home assigment

This is the resolution for technical test provided.

Both Product and Order have been implemented

### Running the application:

A docker-compose file has been created to execute the application. So having docker installed,
just type: `docker-compose up`
This command will start a PostgreSQL on port 5432 and the application on 8080, so both ports must 
be available.  

### Build, test and run (locally)

The application uses gradle. 

Build: On Linux/OSX you can use `./gradlew build` to build the application (will run the tests)

Run: First you need to set database connection. There's a local profile prepared with its own application-local.properties
that can be modified to use a local connection. To run selecting this profile you neeed to export
`export spring_profiles_active=local` 
and then run  `./gradlew bootRun`


Swagger documentation is generated on http://localhost:8080/swagger-ui.html

However, you can use also the following scripts generated by postman:

Create a product:

`
curl --location --request POST 'http://localhost:8080/products/v1/product' 
--header 'Content-Type: application/json' 
--data-raw '{
    "sku" : "12345",
    "name" : "SecondProduct",
    "price" : 11.50,
    "creationDate": "2020-03-30T19:37:30.30301"
}'
`

Retrieve a list of all products: 

`curl --location --request GET 'http://localhost:8080/products/v1/products' 
`

Update a product:

`curl --location --request PATCH 'http://localhost:8080/products/v1/product' 
--header 'Content-Type: application/json' 
--data-raw '{
    "sku" : "12345",
    "price" : 12.50
}'
`

Delete a product (soft delete, the product will not be listed but still exist on database):

`
curl --location --request DELETE 'http://localhost:8080/products/v1/product/12345'
`

Place an Order (all products must previously exist. On json, only sku is considered when creating order)

`
curl --location --request POST 'http://localhost:8080/orders/v1/order' 
--header 'Content-Type: application/json' 
--data-raw '{
    "id" : 1,
    "buyerEmail" : "john@doe.ch",
    "placedTime": "2021-10-30T19:37:30.30301",
    "products" : [
        { "sku" : "12344" }, {"sku" : "12346" }
    ]
}'
`

Retrieve all orders within a given time period (considered 2 dates) Total order amount is presented for each order

`curl --location --request GET 'http://localhost:8080/orders/v1/orders?startMoment=2020-10-01T00:01:30.165&endMoment=2020-10-31T14:01:30.165'`
