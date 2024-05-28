# Sales Insights-Data Analysis Using Power BI and SQL

*PART I — Problem Statement*

AtliQ hardware is a company in India which supplies computer hardware and peripheral devices across India only. They have many stores across India such as surge stores, Nomad stores etc. The head office of the company is situated in Delhi.

## Scenario —

The sales manager of the company is facing many challenges. He is facing issues in tracking sales in dynamically growing market. He is having issues with the insights of his business.

In order to this he has some of the regional managers in North, south and central India working for the company. So, he calls them and ask about the insights he wants to know. They tell him about the sales in last quarter and the growth in that quarter.

So, the problem is that the conversations that are happening are verbal. Hence, the regional managers are sugar coating the facts and the manager of the company does not get the clear picture of the facts. Even after knowing that the sales are declining, he cannot do anything because he does not have the clear picture of the sales. Asking for the records the regional manager provides him with excel files. But by this he cannot figure out small things.

All what the manager wants is a view of the weakest area the company need to focus to increase the sales and improvise the declination. He is interested in simple, understandable and digestive insight. So, he is more interested in a dashboard which he can go and look at the real data because data speaks the truth. All he wants is a simple data visualization tool which he can access on daily basis.

## Data Cleaning and ETL (Extract, Transform, Load):
In this process, we are work on data cleaning and ETL.

 Step 1: Connect the MySQL database with the PowerBI desktop.
 
 Step 2: Loading data into the Power BI deskstop.
 This step load all the tables and created in the data base. This load option will connect with the SQL and pull all the records into power BI environment.
         
 In that model view looking up for model which form the star schema.
         
 ![Screenshot (4)](https://user-images.githubusercontent.com/118357991/233265427-94285bbf-79e6-446f-bd72-dc7c220e0680.png)

 Setp 3: Transform data with the help of Power Query
 
 Perform filtration in market’s table: In the tables perform when we click on the transform data option, we are directed to Power query editor. Power query editor is where we perform out ETL.and then we can perform data transformation i.e. Data Cleaning, Data Wrangling, Data Munging. we need to filter the rows where the values are null and 
 filtering the data and deselecting the blank option. 

 Perform filtration in Transaction’s table: In the table perform when we check the query in the MySQL to filter some negative values and also 0 values that appears in the table, the desired output is received.and we will perform the similar filtration in PowerBI. we have deselecting the values, don’t want in the table. The result after filtration. the zero values represent some garbage values which is not possible so we need to clean that data.

Convert USD into INR in the transaction’s table: the AtliQ Hardware only works in India so the USD values are not possible. we need to convert those USD values into INR by using some formulas. Add new column - Conditional column - normalized currency where sales amount will be in INR

In power query editore finding the total values having USD as currency.

     `=Table.AddColumn(#"Filtered Rows", "norm_sales_amount",each if [currency] = "USD" then [sales_amount]*75 else [sales_amount]`
 
 using this correct formula of the conversion,and converted the USD currency into INR.
  In MySQL Workbench find that there are duplicates of USD and INR
 
     `SELECT count(*) from sales.transactions where sales.transactions.currency="INR\r";` 
     150000 - can't removed as it is large amount

     `SELECT count(*) from sales.transactions where sales.transactions.currency="INR";` 
     279 - we can remove it as it is small record and can be considered as bad data

     `SELECT count(*) from sales.transactions where sales.transactions.currency="USD\r";` 
 
     `SELECT count(*) from sales.transactions where sales.transactions.currency="USD";`

     `SELECT * from sales.transactions where sales.transactions.currency='USD\r' or sales.transactions.currency='USD';`

we can see that it is duplicate and for analysis its better to delete anyone of them so lets delete USD and keep USD/r. finally we will keep data with INR/r and USD/r-

Hence, by using such tools and technology one can make data driven decisions which helps to increase the sales of the company.

So, in this project we will help a company make its own sales related dashboard using PowerBI.

*PART II — Data Discovery*

## Project planning using AIMS grid –

AIMS grid: It is a project management tool which consists of four components to it.

1) Purpose (what to do exactly)

2) Stack holders (who will be involved)

3) End result (what do you want to achieve)

4) Success criteria (cost optimization and time save)

In our case the end result will be the dashboard created and success criteria will be bumping up the sales using cost optimization and save the time of the manager of the company.
![Screenshot 2024-05-28 183442](https://github.com/mdilshad7478/Sales-Insights---Data-Analysis-Using-Power-BI-and-SQL/assets/157358118/b892c544-8e04-4799-9b8c-ba1a12c25851)

- #### Flowchart of project execution -

  ![1_khhcniAryBdmmfJt0Zk0Lg](https://user-images.githubusercontent.com/118357991/231545034-7f6cc437-5683-44f1-92df-a671540ccae9.jpg)

*PART III — Data Analysis using SQL*

Completed the Data discovery and then used mySQL for data analysis.

SQL database dump is in db_dump.sql file above. Download db_dump.sql file to your local computer

- Importing Data to MySQL workbench

![Screenshot (2)](https://user-images.githubusercontent.com/118357991/233262007-c36f58cd-df19-42b5-b9cb-4d72c0ef64a4.png)

![Screenshot (3)](https://user-images.githubusercontent.com/118357991/233262064-b1fb8f0f-8c16-402d-adac-07784b81a2fe.png)


The import of data is done from an already existing MySQL file. This file has to be loaded into MySQL workbench for further data analysis. 
- Analysis of data by looking into different tables and reflecting garbage values

   We can see that garbage value that the table market cantains certain values which are incorrect.
   
     `SELECT * FROM sales.market;`
     
   And then we can check that transacation table we can see that ceratin negative value in amount which is not possible. and we can see that certain transactions are      in USD. Hence, filtration of that is also needed by converting into INR.
     
     `SELECT * FROM sales.transactions;`
     
 - Analysis of different SQL statement on data base
     
   1.To find of all customers records
    `SELECT * FROM sales.customers;`
      
   2.To find total number of customers 
     
     `SELECT count(*) From sales.customers;`

   3.To find transactions for Chennai market (market code for chennai is Mark001
     
      `SELECT * FROM sales.transactions where market_code='Mark001';`

   4.To find distrinct product codes that were sold in chennai
     
      `SELECT distinct product_code FROM sales.transactions where market_code='Mark001';`
    5.To find transactions for Chennai market (market code for chennai is Mark002
     
      `SELECT * FROM sales.transactions where market_code='Mark002';`

   6.To find distrinct product codes that were sold in mumbai
     
      `SELECT distinct product_code FROM sales.transactions where market_code='Mark002';`
      
   7.To find transactions where currency is US dollars
     
      `SELECT * from sales.transactions where currency="USD";` 
      
   8.To find transactions in 2020 join by date table
     
      `SELECT sales.transactions.*, sales.date.* FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date where sales.date.year=2020;`
     8.To find transactions in 2019 join by date table
     
      `SELECT sales.transactions.*, sales.date.* FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date where sales.date.year=2019;`  

   9.To find total revenue in year 2020,
     
      `SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date where sales.date.year=2020 and sales.transactions.currency="INR\r" or sales.transactions.currency="USD\r";` 
      
   10.To find total revenue in year 2019,
     
      `SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date where sales.date.year=2019 and sales.transactions.currency="INR\r" or sales.transactions.currency="USD\r";`
     11.To find total revenue in year 2020, January Month,
     
      `SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date where sales.date.year=2020 and sales.date.month_name="January" and (sales.transactions.currency="INR\r" or sales.transactions.currency="USD\r");` 

   12.To find total revenue in year 2020, February Month,
     
      `SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date where sales.date.year=2020 and sales.date.month_name="February" and (sales.transactions.currency="INR\r" or sales.transactions.currency="USD\r");` 

   13.To find total revenue in year 2019, January Month,
     
      `SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date where sales.date.year=2020 and sales.date.month_name="January" and (sales.transactions.currency="INR\r" or sales.transactions.currency="USD\r");` 

   14.To find total revenue in year 2019, February Month,
   
      `SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date where sales.date.year=2020 and sales.date.month_name="February" and (sales.transactions.currency="INR\r" or sales.transactions.currency="USD\r");` 

   15.To find total revenue in year 2020 in Chennai
     
      `SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date where sales.date.year=2020 and sales.transactions.market_code="Mark001";` 

   16.To find total revenue in year 2020 in Mumbai
     
      `SELECT SUM(sales.transactions.sales_amount) FROM sales.transactions INNER JOIN sales.date ON sales.transactions.order_date=sales.date.date where sales.date.year=2020 and sales.transactions.market_code="Mark002";` 

Similarly, if we want different of any other particular city the market code of that city is used on the mysql workbench.

*PART IV — Data Cleaning and ETL (extract transform load)*
- Step 1: We are going to connect MySQL with the PowerBI dektop
- Step 2: Loading the data into the PowerBI desktop
    Here, we are going to load all the tables we have created in the data base. This load option will connect with the SQL and pull all the records into power BI environment.

[Looking up for model which forms the star schema]![Screenshot 2024-05-28 204304](https://github.com/rppradhan08/sales-insight/assets/157358118/c4a77b41-a6d7-4aa0-b18f-272787b6227e)

