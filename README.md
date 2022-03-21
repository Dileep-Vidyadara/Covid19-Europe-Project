# Covid19 Project based on Europe Data End2End
This project mainly focus on the Europe Covid19 Data. 
The data has been taken from EuroStat and European Centre for Disease Prevention and Control (ECDC) wesbsites.

* The main objective of this project is to use raw data to perform **ETL** (Extract, Transform and Load) using  **Azure Data Factory**.

### Types of files and data used in the project are listed below:

File Name | Description of File
-------- | ---------
1.population_by_age.tsv.gz (EuroStat/Blob) | This is a zipped file that contains the population data of countries.
2.cases_deaths.csv (ECDC) | This csv contains the number emerging Covid Cases and Deaths followed by the each day.
3.hospital_admissions.csv (ECDC) | This csv file contains the Daily Hospital Admissions, Daily ICU admissions, Weekly Hospital Admissions per 100k, Weekly ICU Admissions per 100k.
4.testing.csv (ECDC) | This csv file contains the emerging covid cases, tests being done, testing_rate and covid postive_rate on the weekly basis.
5.lookups (Blob) | Other miscellaneous  files like Calendar Lookup/dim and Country lookup/dim files used.

### The below image represents the solution architechture for this project :
![](./Slides_and_Screenshots(Media)/project_solution_architecture.png)



### Step 1: 
* I have uploaded the *population_by_age.tsv.gz (zipped_file)* into the Azure Blob Storage manually. After that I initialized the Azure Data Factory and created the **Linked Service** which connects to the Blob Storage and created **DataSet** points for the particular file. Along with that I created the **Pipeline** which has a **Copy Activity** that helps to Copy the Population Data from Blob storage to the **ADLS gen2**


![](./Slides_and_Screenshots(Media)/population_data_blob_ingestion.png)


### Step 2: 
* Now I have rest 3 datasets i.e cases_and_deaths, hospial_admissions, testing_data that are successfully uploaded in the GitHub repository under Covid19-Europe-Project
/main-csv-data-files. Now I have to connect these files to the ADF through ***https:*** Linked Service and gave my base url name to it. Since I wanted to create ingest all these 3 datasets into **ADLS gen2** at a time, I created a json file with 3 files and created parameterized datset and with the help of Lookup acivity and ForEach activity I was able to successfully ingest the data into the **ADLS gen2**.


![](./Slides_and_Screenshots(Media)/github_files_ingestion.png)


### Step 3:
* Once right after the required data ingested into the ADLS gen2 storage, I have created the dataFlows for all of the four files and creatd the required datasets for those. Now the DataFow will help us to create the transformations according to the project requirement. Once after the dataFlows build, I created the the pipelines for all of those 4 datasets which redirects to the another Container in the same ADLS storage.

*These the pictures of the dataFlows that are used for the transformations*

![](./Slides_and_Screenshots(Media)/dataflow_population_data.png)

![](./Slides_and_Screenshots(Media)/dataflow_cases_and_deaths_data.png)

![](./Slides_and_Screenshots(Media)/dataflow_hospial_admissions_data.png)

![](./Slides_and_Screenshots(Media)/dataflow_testing_data.png)


### Step 4: 
* Since we did transformation according to the project requirement, the data in the new container in ADLS storage can be used for Machine Learning and Data Science purpose. Filly one more time we are going to use ADF to load the data into the SQL database, so that Data Analysts can query  data directly from the database by using PowerBi/Tableau or any other data visualization tool for analysis purpose.

![](./Slides_and_Screenshots(Media)/PowerBi_ss.png)


![](./Slides_and_Screenshots(Media)/tests_vs_new_cases_1.png)

![](./Slides_and_Screenshots(Media)/tests_vs_new_cases_2.png)

![](./Slides_and_Screenshots(Media)/tests_map.png)

* I have also used **Schedule Triggers** at the time of Ingesting the data into the ADLS storage from different storage and also used **Event Triggers** which triggers when the data reaches into the ADLS storage.
* I basically categorized the data into 3 types. i.e ***Brozne --> Silver --> Gold*** which basically means when the data moves from bronze to gold, the quality and the value of the data improves.

* This project has been taken inspiration from one of the course in Udemy: https://www.udemy.com/course/learn-azure-data-factory-from-scratch/



![](./Slides_and_Screenshots(Media)/giphy.gif)




