# Denver_Traffic_Accidents
ETL Challenge


This application takes an input of csv file and standardizes, normalizes to make the data consistent. This application then saves the data into MongoDB database and performs statistical analysis in order to provide more details about data. Then the output of statistical analysis is converted to HTML file format and results are attached to email that is sent through jobEmail.kjb job file.
Transformation: Traffic_Accidents.ktr
Following are the steps used to cleanse and upload the data in mongoDB database. Traffic_Accidents.ktr 
1) ‘CSV file input’ is used to load data from csv file.
2) Second step sorts the dataset on Incident_ID.
3) After sorting the dataset, using ‘Unique rows’ duplicate records if any are removed from the dataset.
4) OFFENSE_CODE_EXTENSION, GEO X, GEO Y attributes are removed using ‘Select Values’ step.
5)  BICYCLE_IND and PEDESTRIAN_IND fields are converted to Boolean format using ‘Select Values’ step.

6) Date format is converted to ‘MM/dd/yyyy HH:mm:ss’ format using using ‘Select Values’ step.
7) ‘Replace in string’ is used to cleanse and make values consistent in address field. In this step ‘STREET’ is replaced with ‘ST’, ‘AVENUE’ is replaced with ‘AVE’, ‘BLOCK’ is replaced with ‘BK’, ‘WEST’ is replaced with ‘W’, ‘EAST’ is replaced with ‘E’, ‘NORTH’ is replaced with ‘N’, ‘SOUTH’ is replaced with ‘S’ for all the addresses.
8) In ‘MongoDB Output’, this cleansed and standardized data is saved in MongoDB database named ‘DENVERACCIDENTSDB’ and collection name ‘ACCIDENTSCOLL’.
9) In next step for calculating distinct OFFENSE_TYPE_IDs, dataset is sorted on OFFENSE_TYPE_ID and using ‘Group by’ transformation OFFENSE_TYPE_ID is grouped which gives list of distinct OFFENSE_TYPE_IDs.
10) The list of OFFENSE_TYPE_IDs is written to a HTML file format and then the transformation attaches the HTML file with email. HTML file can be further formatted to table format for easier visualization.
11) Similarly, total number of records for each DISTRICT_ID, total number of rows, number of bicycle incidents and number of pedestrian incidents is calculated using ‘Group by’ transformation.

jobEmail.kjb
The above job sends an email using output obtained by previous ‘Transformation’ step. On success of all the jobs success step is executed. 


While using previous versions of Pentaho, which does not support big data transformations like ‘MongoDB Output’, we can use ‘JSON output’ and then import the JSON file using mongoimport command.
For example:

mongoimport --db DENVERACCIDENTSDB --collection ACCIDENTSCOLL --drop --file ~/PATH/filename.json

