
<h1 align="center">📦Earnings Report 2Q 2025:
  
theLook eCommerce  </h1>
<div align="justify">
TheLook is a fictional eCommerce clothing platform created by the Looker team and hosted on Google Cloud. The project will assumes TheLook operates as a publicly traded company and tackled the challenge of creating a SEC-compliant earnings report for Q2 2025. For this SQL will be used to query key performance indicators (KPIs) and prepare an investor-facing earnings report for TheLook, benchmarking results against Mercado Libre’s industry standards.  <h6>***Note: Public companies are required by the SEC to disclose detailed financial statements and earnings reports each quarter for transparency with investors and the public.***</div>
  
## Table of Contents
[here](https://github.com/tinyazure/The-Look-eCommerce-Earnigs-Report/tree/main?tab=readme-ov-file#thelook-ecommerce-dataset)
  
## theLook eCommerce Dataset
The dataset contains information about customers, products, orders, logistics, web events and digital marketing campaigns. The contents of this dataset are synthetic, and are provided to industry practitioners for the purpose of product discovery, testing, and evaluation.<br>[Click for more dataset details if you have an enabled google account(gmail)](https://console.cloud.google.com/bigquery(cameo:product/bigquery-public-data/thelook-ecommerce)?project=my-gcp-data-projects).<br>
[Click and read the short instructions to enable your account(gmail)](https://cloud.google.com/bigquery/docs/sandbox#setup).<br>

The Dataset contains 8 tables:<br>  1. distribution_centers<br>  2. events<br>3. inventory_items<br>4. order_items<br>  5. orders<br>  6. products<br> 7. users       
For the porpouse of this project not all tables will be used.<br>[Click to View the Dataset if you have an enabled google account(gmail)](https://console.cloud.google.com/bigquery?p=bigquery-public-data&d=thelook_ecommerce&page=dataset&project=airy-passkey-472721-b0).<br>

## Q2 2025 Mercado Libre One Pager Earnings Report

| Q2 2025 Report|KPI's to be replicated|
|-----------------|-----------------------|
| <img src="https://github.com/tinyazure/The-Look-eCommerce-Earnigs-Report/blob/main/images/One_Pager_Report_Meli_2Q2025.jpg" width="500" height="640"> | The report is based on key performance indicators (KPIs) relevant to the eCommerce industry. For this project, the following four KPIs will be queried and analyzed using SQL and Google BigQuery Public dataset:<br><br><br><br>1. Revenue and Year-over-Year Revenue Growth<br>2. Number of Unique Buyers<br>3. Purchases per Second (calculated on a daily basis)<br>4. Deliveries completed in under 48 hours

## Q2 2025 The Look eCommerce Earnings figures and KPI´s 

1. Q2 2025 Revenue and Year-over-Year Revenue Growth:  
   * Net Revenue = $0.850 m
   * Growth YoY = +86%  
     [Query](https://github.com/tinyazure/The-Look-eCommerce-Earnigs-Report/blob/587d9738d6aea0edf28a4f027f5af86d323fdc1a/1_Revenue%20and%20Year-over-Year%20Revenue%20Growth.md)  
2. Q2 2025 Number of Unique Buyers:    
   * 11516  
     [Query](https://github.com/tinyazure/The-Look-eCommerce-Earnigs-Report/blob/main/2_Active_Customers_2Q2025.md)  
4. Q2 2025 Purchases per Day (reemplace for Second):    
   * 150  
     [Query](https://github.com/tinyazure/The-Look-eCommerce-Earnigs-Report/blob/main/3_Purchases%20per%20Second%20(calculated%20on%20daily%20basis).md)
5. Q2 2025 Deliveries completed in under 48 hours:  
    *  610  
      [Query](https://github.com/tinyazure/The-Look-eCommerce-Earnigs-Report/blob/main/4_Deliveries%20completed%20in%20under%2048%20hours.md)
   
