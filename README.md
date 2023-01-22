# DELTA
## DB(D)  Endpoint(E)  Latency(L)  Testing(T)  Ammeter(A)

📣 Annoucing a new tool to calculate Cloud Database endpoint latency using SQL queries

📌 Introducing DELTA (DB Endpoint Latency Testing Ammeter). DELTA is a tool to test real-world latency against a remote database using execution of a query and calculating the network return time. The tool provides functions to test latency of Oracle, MySQL and Postgres databases.

The tool uses the oracledb, psycopg2 and pymysql packages to connect to the respective databases and execute a single query per request (you can specify multiple requests as well). The tool uses the time module to measure the time it takes to execute the query, fetch the results, and close the connection. It calculates the latency of each request and the average latency of all requests.



![Delta_Ammeter](https://user-images.githubusercontent.com/39692236/213663909-24aaf0cd-8abc-429b-811d-25303aa7063e.png)

🔧 DELTA is a cloud tool to test real-world latency against a remote database endpoint using execution of a query and calculating the network return time. 


🔧 Network tools like ping ,iperf or tcp ping can only give you network based latency which does not always translate well to an application running those queries on a remote database. 


🐍 DELTA uses Python client for Oracle, MySQL and PostgreSQL to run a query like “SELECT 1” or "SELECT 1 FROM DUAL". You can then specific the number of executions of the query and DELTA calculates the average network round-trip time for all the executions of the query on the remote database. The script also includes error handling to track failed requests. You can also include your own custom queries.

## How it works
The function 'measure_latency_oracle' uses the oracledb package to connect to the Oracle database and execute a single query per request. The function uses the time module to measure the time it takes to execute the query, fetch the results, and close the connection. The function then calculates the latency of each request and the average latency of all requests. The function opens a new connection for every request and closes it after fetching the results, which will measure the time it takes to execute the query, transfer the data over the network, and close the connection. The query parameter is passed to the function, allowing you to test the performance of the database with different queries. The same logic applies to the functions of mysql and postgres

🔌 Databases Supported :

 📌 Oracle : 

- Amazon RDS Oracle

- OCI Autonomous Database

- OCI VMDB

- OCI Exadata Cloud Service

- Oracle Database On-Premise


📌 MySQL : 

- Amazon RDS MySQL

- Amazon RDS Aurora MySQL

- OCI MySQL Database Service

- OCI MySQL Heatwave

- On-premise MySQL


📌 Postgres :

- Amazon RDS Postgres

- Amazon RDS Aurora Postgres

- On-premise Postgres

# Requirements

```
Docker Version (Minimum): 20.10.17
```

# Installation

You can install and run DELTA as a Docker container

Build from source

```
$ git clone https://github.com/shadabshaukat/DELTA.git && cd DELTA/

$ docker build -t delta . --tag delta  --pull --no-cache --force-rm

```

# Usage

The tool can be run using the command line. You will need to provide the following arguments:

    dbtype : The type of database you want to test. Can be 'oracle', 'autonomous', 'mysql' or 'postgres'
    num_of_requests : The number of requests you want to make to the database
    username : The username to connect to the database
    password : The password to connect to the database
    hostname : The hostname or IP address of the database
    portnumber : The port number of the database
    databasename : The name of the database
    "SQL QUERY" : The SQL query you want to test

## Run Autonomous Database Latency Check
### Note

- In case of Oracle Autonomous database, the connecting string can be found in OCI Console > Autonomous Database > DB Connection
- Currently only non-mTLS connections are supported. mTLS with wallet should be available soon

```
$ docker run -it delta python3 main.py \
autonomous \
1 \
admin \
YourP@ssw0rd \
'(description= (retry_count=20)(retry_delay=3)(address=(protocol=tcps)(port=1521)(host=*******.adb.ap-melbourne-1.oraclecloud.com))(connect_data=(service_name=********_testdelta_high.adb.oraclecloud.com))(security=(ssl_server_dn_match=yes)))' \
"SELECT 1 from DUAL"
```

## Run Oracle Database Latency Check

```
$ docker run -it delta python3 main.py \
  oracle \
  1 \
  hr \
  YourP@ssw0rd  \
  'orcl11g-scan.********.demovcn.oraclevcn.com:1521/orcl11g.******.demovcn.oraclevcn.com' \
  "SELECT 1 from DUAL" 
  ```

## Run Postgres Latency Check

```
$ docker run -it delta python3 main.py \
postgres \
1 \
postgres \
YourP@ssw0rd  \
database-1.******ap-southeast-2.rds.amazonaws.com \
5432 \
demo \
"SELECT 1"
```

## Run MySQL Latency Check

```
$ docker run -it delta python3 main.py \
mysql \
1 \
admin \
YourP@ssw0rd  \
mysqldemo.c******ap-southeast-2.rds.amazonaws.com \
3306 \
demo \
"SELECT 1"
```


## Check python-oracledb and Oracle Instantclient version
```
$ docker run -it delta python3 -c "import oracledb; print(oracledb.version)"
1.2.2
```

```
$ docker run -it delta ls /usr/lib/oracle
19.10
```


    


# Function Definitions
## measure_latency_oracle(user,password,dsn,num_requests,query)

This function is used to measure the latency of an Oracle database. It takes in the following parameters:

    user: The username to connect to the Oracle database.
    password: The password to connect to the Oracle database.
    dsn: The hostname and portnumber/servicename of the Oracle database.
    num_requests: The number of requests to be made to the database.
    query: The SQL query to be executed on the database.

It returns the average latency in seconds and milliseconds, the number of successful requests and any errors that occurred during the test. 

## measure_latency_autonomous(user,password,dsn,num_requests,query)

This function is used to measure the latency of an Oracle Autonomous Database with TLS (mTLS currently isn't supported). It takes in the following parameters:

    user: The username to connect to the Autonomous database.
    password: The password to connect to the Autonomous database.
    dsn: The connectivity string of the Autonomous database.
    num_requests: The number of requests to be made to the database.
    query: The SQL query to be executed on the database.

It returns the average latency in seconds and milliseconds, the number of successful requests and any errors that occurred during the test. 

## measure_latency_mysql(user,password,host,port,database,num_requests,query)

This function is used to measure the latency of a MySQL Database. It takes in the following parameters:

    user: The username to connect to the MySQL database.
    password: The password to connect to the MySQL database.
    host : Hostname or IP to connect to the MySQL database
    port : Port number of the MySQL database
    database : Database name of the MySQL instance
    num_requests: The number of requests to be made to the database.
    query: The SQL query to be executed on the database.

It returns the average latency in seconds and milliseconds, the number of successful requests and any errors that occurred during the test. 

##  measure_latency_postgres(user, password, host, port, dbname, num_requests, query)

This function is used to measure the latency of a Postgres Database. It takes in the following parameters:

    user: The username to connect to the Postgres database.
    password: The password to connect to the Postgres database.
    host : Hostname or IP to connect to the Postgres database
    port : Port number of the Postgres database
    dbname : Database name of the Postgres instance
    num_requests: The number of requests to be made to the database.
    query: The SQL query to be executed on the database.

It returns the average latency in seconds and milliseconds, the number of successful requests and any errors that occurred during the test.

 
# Contributing

    Fork it (https://github.com/your-github-username/DELTA/fork)
    Create your feature branch (git checkout -b feature/fooBar)
    Commit your changes (git commit -am 'Add some fooBar')


