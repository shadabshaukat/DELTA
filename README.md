# DELTA
## DB(D)  Endpoint(E)  Latency(L)  Testing(T)  Ammeter(A)

📣 Excited to announce a new tool to calculate Cloud Database endpoint latency using SQL. Launching DELTA (DB Endpoint Latency Testing Ammeter)


![Delta_Ammeter](https://user-images.githubusercontent.com/39692236/213663909-24aaf0cd-8abc-429b-811d-25303aa7063e.png)

🔧 DELTA is a cloud tool to test real-world latency against a remote database endpoint using execution of a query and calculating the network return time. 


🔧 Network tools like ping ,iperf or tcp ping can only give you network based latency which does not always translate well to an application running those queries on a remote database. 


🐍 DELTA uses Python client for Oracle, MySQL and PostgreSQL to run a query like “SELECT 1” or "SELECT 1 FROM DUAL". You can then specific the number of executions of the query and DELTA calculates the average network round-trip time for all the executions of the query on the remote database. The script also includes error handling to track failed requests. You can also include your own custom queries.

## How it works
The function 'measure_latency_oracle' uses the cx_Oracle package to connect to the Oracle database and execute a single query per request. The function uses the time module to measure the time it takes to execute the query, fetch the results, and close the connection. The function then calculates the latency of each request and the average latency of all requests. The function opens a new connection for every request and closes it after fetching the results, which will measure the time it takes to execute the query, transfer the data over the network, and close the connection. The query parameter is passed to the function, allowing you to test the performance of the database with different queries. The same logic applies to the functions of mysql and postgres

🔌 Databases Supported :

 Oracle : 

- Amazon RDS Oracle

- OCI Autonomous Database

- OCI VMDB

- OCI Exadata Cloud Service

- Oracle Database On-Premise


MySQL : 

- Amazon RDS MySQL

- Amazon RDS Aurora MySQL

- OCI MySQL Database Service

- OCI MySQL Heatwave


Postgres :

- Amazon RDS Postgres

- Amazon RDS Aurora Postgrea


# Deployment

$ git clone https://github.com/shadabshaukat/DELTA.git

$ cd DELTA

$ sudo pip3 install -r requirements.txt

# Test Oracle Latency 
  ## Autonomous Oracle DB ##
  $ python3 main.py oracle <num_of_requests> <username> <password> 'hostname:portnumber/servicename' "SQL QUERY"
  
$ python3 main.py oracle 10 admin YourP@ssw0rd1234#  '(description= (retry_count=20)(retry_delay=3)(address=(protocol=tcps)(port=1521)(host=******.adb.ap-melbourne-1.oraclecloud.com))(connect_data=(service_name=*********_testdelta_high.adb.oraclecloud.com))(security=(ssl_server_dn_match=yes)))' "SELECT 1 from DUAL"
  
  <img width="1507" alt="Screen Shot 2023-01-20 at 8 52 45 pm" src="https://user-images.githubusercontent.com/39692236/213666821-45660d3e-5539-4bec-be36-9bdc9ae8360c.png">

  
  ## Non-Autonomous Oracle DB ##
  $ python3 main.py oracle <num_of_requests> <username> <password> 'connectingstring' "SQL QUERY"
  
$ python3 main.py oracle 10 admin YourP@ssw0rd '10.10.1.10:1521/orcldev' "SELECT 1 from DUAL"
  
  <img width="1504" alt="Screen Shot 2023-01-20 at 8 50 53 pm" src="https://user-images.githubusercontent.com/39692236/213666852-c61c8f26-b12d-4c00-9a2b-75401b67d517.png">


# Test MySQL Latency
  $ python3 main.py mysql <num_of_requests> <username> <password> <host> <port> <databasename> "SQL QUERY"
  
$ python3 main.py mysql 10 admin YourP@ssw0rd mysqldemo.********.ap-southeast-2.rds.amazonaws.com 3306 demo "SELECT 1"
  
  
<img width="1509" alt="Screen Shot 2023-01-20 at 8 49 25 pm" src="https://user-images.githubusercontent.com/39692236/213666881-79be4f8b-de7d-47b6-84ed-6a57c6f48941.png">

# Test Postgres Latency
  $ python3 main.py postgres <num_of_requests> <username> <password> <host> <port> <databasename> "SQL QUERY"
  
$ python3 main.py postgres 1 postgres YourP@ssw0rd1234#_ database-1.********.ap-southeast-2.rds.amazonaws.com 5432 demo "SELECT 1"
  
  <img width="1498" alt="Screen Shot 2023-01-21 at 3 01 07 am" src="https://user-images.githubusercontent.com/39692236/213746779-bdb07df3-f2f2-4d7e-8679-89db5dd4779d.png">


