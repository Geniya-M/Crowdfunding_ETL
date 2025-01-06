# Project 2: ETL Mini Project

* For the ETL mini project, an ETL pipeline was built using Python, Pandas, and either Python dictionary methods or regular expressions to extract and transform the data. The second option, regular expressions, was chosen.
  

## The process:

### 1. Extract the crowdfunding excel file by reading it into a Pandas DataFrame and obtain a brief summary of it's context. 

* This was achieved using the following functions and attributes:

    * pd.read_excel() : to be able to read the file

    * head(): to display the five top rows

    * info(): to get a concise summary of a DataFrame
      

### 2. Create the Category and Subcategory DataFrames

* Using the .columns function, the column names in the crowdfunding DataFrame were listed, showing that the values for category and subcategory were combined into one column titled "category & sub-category". The two were seperated into individual DataFrames doing the following:

    * str.split(): to split strings into the "category & sub-category" column based on a '/' delimiter and the parameter 'expand=True' to ensure the results include new columns titled "category" and "sub-category"

    * unique(): to obtain catorgory and subcategory unique values

    * len(): the number count of said values

    * np.arange(): to create numpy arrays from 1-9 (category_ids) and 1-24 (subcategory_ids)

    * 'for loop' function to be able to add "cat" and "subcat" to each category_id and subcategory_id (using list comprehension)

* This was then used to create and export new category and subcategory DataFrames as CSV files using the to_csv function.
  

### 3. Create the Campaign DataFrame

* The crowdfunding excel data was extracted again and used to create a campaign DataFrame which included columns that were converted into different datatypes, formates, renamed, and/or newly added. This was done doing the following:

    * rename(): to rename the "blurb", "launched_at" and "deadline" columns

    * astype(): to change "goal" and "pledged" columns' datatype to float

    * dtypes:  to check datatypes

    * pd.to_datetime(campaign_df[" "]): to convert the values in the "end_date" and "launched_date" columns to datetime object
        * dt.strftime('%Y-%m-%d'): to format the datetime objects as strings in the specified format: Year-Month-Day 
    
    * pd.merge(): to merge the campaign_df with the category_df on the "category" column and the subcategory_df on the "subcategory" column.

    * tail(): to display the last 10 rows the campaign_merged_df

    * drop(): to delete the unwanted "staff_pick", "spotlight", "category & sub-category", "category", "sub-category" columns from the campaign DataFrame

* The campaign DataFrame was then exported as a CSV file using the to_csv function.
  

### 4. Extract the Contacts Data

* There were two options offered for extracting and transforming the data from the contacts excel sheet:

    * Option 1: Python dictionary

    * Option 2: Regular expressions

* We decided to use Option 2: 
    * copy(): to create a copy of the contact_info_df DataFrame after importing it

    * str.extract(r'"contact_id":\s*(\d{4})'): to extract the four-digit contact ID number from the contact_info

    * astype('int64'): to convert the 'contact_id' column into an int64 datatype

    * str.extract(r'"____":\s*"([^"]+)"'): to extract the "name" and "email" columns from the contacts and add their values to new columns of the same names: 
        * str.extract(r'"name":\s*"([^"]+)"')

        * str.extract(r'"email":\s*"([^"]+)"')
    
    * drop(): to create a copy of the contact_info DataFrame that did not include the 'contact_info' column

    * str.split(expand=True): to create "first_name" and "last_name" columns with the first and last names from the "name" column

    * drop(): to drop the "name" column now that columns for first and last names were created

    * reordered the columns in this order: [['contact_id', 'first_name', 'last_name', 'email']]

* After doing a final check on the datatypes, a clean version of the contacts DataFrame was extracted as a CSV file and renamed "contacts_df_clean"
  

### 5. Create the Crowdfunding Database

* Using the QuickDBD application, an ERD of the four CSV files (campaign, category, subcategory, and contacts) was created, specifying the datatypes, primary keys and foreign keys 

    * The category, subcategory and contact tables have one-to-many relationships with the campaign table via "category_id" (category table), "subcategory_id" (subcategory table) and "contact_id" (contact table). While these columns are specified as foreign keys in the campaign table, they are identified as primary keys in the respective tables.

* The ERD was used to create a table schema for the CSV files, then saved as a Postgres file titled "crowdfunding_db_schema.sql".

* In SQL, a new Postgres database was created, "crowdfunding_db", where "crowdfunding_db_schema" was used to created tables for the CSV files and their respective primary and foreign keys. Each CSV file was then imported into its corresponding SQL table. 
