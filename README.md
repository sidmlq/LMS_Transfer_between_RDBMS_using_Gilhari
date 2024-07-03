# Library management system using Gilhari microservice
## Overview ##
This project is an example of how Gilhari microservice framework can be used to transfer (JSON) data between two relational databases of different kinds (MySQL and Postgres).
The project shows:
* Reverse-engineering of a JSON object model (and its object relational mapping) from the first relational database having an existing schema (by using the JDX ORM).
* Populating the first database DB1 with x number of JSON objects by configuring and using the Gilhari microservice framework. (Optional if DB1 already has the data that needs to be transferred)
* Using a Java/Python/GUI program to retrieve the existing data from the first database (DB1 of kind MySQL) using one instance of the Gilhari microservice and then to transfer the data to a second database (DB2 of kind Postgres) using another instance of the Gilhari microservice configured for the second database.

## Description ##

* The project simulates a library management system. On the "server" side, there is a MySQL source database with tables with data of Books, Authors, Members and Loans. On the "client" side, there is a Postgres destination database with tables for each member to store each member's loan history. 
* There are two gilhari instances listening on two different ports (one each for server and client). The first instance uses GET requests to stream data from the MySQL database. The second instance then uses a POST request to send the retrieved data to the second database.
* The database agnostic property of Gilhari allows for switching between the two databases very simple and straightforward.

## Setting up Gilhari ##

* Download Gilhari SDK from [SoftwareTree](https://www.softwaretree.com/v1/products/gilhari/gilhari_introduction.php)

* Read the documentation shipped with the SDK for more information about Gilhari

## Configuring the project ##

* It is recommended to use Java JDK-8 (install from official sources) while using JDX related tasks with Gilhari.

* Set environment variables for jdx, databases. Install Docker engine. 

* Clone the repository and place your local repository in the ```examples``` folder of your local installation of Gilhari SDK.

## Running the project
* ```reveng``` contains files to Reverse Engineer java container classes and declare the Object Relational Mapping (ORM) specification file.
    1. Refer to the README for more details.
    2. Change the Database URL and driver in the .config file as required.
        > You may consider using an existing database that you are comfortable with to use for your requirements. 

* ```sourcedb``` contains scripts to compile Java source files, build Docker image and run it to have the first Gilhari instance listen at ```localhost:8081```.
    1. In ```sourcedb/src/com/mycompany/gilhari5```, create a class file Author.java as shown to create a JDX_JSONObject (derived from Software Tree's JDX). These files are as created by ```reveng/JDXReverseEngineer```. 
    2. The ```compile.cmd``` compiles all the added Java files in the above directory.
    >Note: Add the references and names to the java files in ```sources.txt``` appropriately.
    >Optionally to the run this application as a standalone- in the lib/ directory, add the requirements as .jar files (here, a jdx-json package and jxclasses.jar, found in the libs/ directory of the Gilhari SDK installation).  
    3. Classname mapping file and .jdx file as mentioned in ```gilhari5_source_mysql_local.config``` can be found in ```sourcedb/config```. You may edit attributes and relations in the ORM specification file as required. Add the appropriate JDBC driver for your database in the same ```./config``` directory.
    4. Create a docker file as shown and run ```build.cmd``` to build the docker image. Then use ```run_docker_app.cmd``` to run the image.
    5. To run curl commands as in ```curlPopulate.cmd``` and ```curlStreamData.cmd```, open a new terminal window, navigate to the directory and then run the command files. There should be corresponding activity on the earlier command terminal window below the confirmation message of Gilhari listening at a port (set here to 8081).
    6. Postman can also be used to perfrom REST API calls. Refer to Gilhari_API manual as shipped with the SDK for more details. 

* ```docs``` contains the ER diagram of the database which makes the details in the OR-Mapping file clearer. 

[!NOTE]
Gilhari is a product of Software Tree, LLC. Use is permitted only with a valid license as issued by Software Tree, LLC. FOr more information, please refer to Software Tree's [website](https://www.softwaretree.com)