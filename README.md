# Built an end-to-end ETL pipeline to capture and process real-time books data

## Tools Used: Python, Spark, AWS, Redshift, Airflow, PostgreSQL  

**Overview**  
Data is captured in real time from the goodreads API using the Goodreads Python wrapper. The data collected from the goodreads API is stored on local disk and is timely moved to the Landing Bucket on AWS S3. ETL jobs are scheduled in airflow to run every 10 minutes.  

### ETL Flow  
**1)** Data Collected from the API is moved to landing zone s3 buckets. 
**2)** ETL job has s3 module which copies data from landing zone to working zone.  
**3)** Spark job is triggered which reads the data from working zone and apply transformation. Dataset is repartitioned and moved to the Processed Zone.  
**4)** Warehouse module of ETL jobs picks up data from processed zone and stages it into the Redshift staging tables.  
**5)** Using the Redshift staging tables and UPSERT, operation is performed on the Data Warehouse tables to update the dataset.  
**6)** ETL job execution is completed once the Data Warehouse is updated.  
**7)** Airflow DAG runs the data quality check on all Warehouse tables once the ETL job execution is completed.  
**8)** Airflow DAG has Analytics queries configured in a Custom Designed Operator. These queries are run and again a Data Quality Check is done on some selected Analytics Table.  
**9)** Dag execution ends after these Data Quality check.  

#### Goodreads Pipeline DAG  
![image](https://user-images.githubusercontent.com/68136798/113649404-fada6200-9653-11eb-92da-a539c282c827.png)
  
#### DAG View  
![image](https://user-images.githubusercontent.com/68136798/113649490-23625c00-9654-11eb-8d57-b97728b64200.png)
  
#### DAG Tree View  
![image](https://user-images.githubusercontent.com/68136798/113649609-586eae80-9654-11eb-96c7-a74de7480e08.png)
  
#### DAG Gantt View  
![image](https://user-images.githubusercontent.com/68136798/113649689-7a683100-9654-11eb-9766-b92aca1fe026.png)
  
#### Data Loaded to Warehouse  
![image](https://user-images.githubusercontent.com/68136798/113649857-d29f3300-9654-11eb-9c53-ec5e1306b5aa.png)
  
