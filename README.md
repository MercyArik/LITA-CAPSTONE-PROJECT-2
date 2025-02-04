## Customer Segmentation for a Subscription Service
### Project Summary
This project analyzes customer data for a subscription service to identify key customer segments, understand subscription patterns, and analyze trends in cancellations and renewals. The goal is to provide actionable insights into customer behavior and subscription types. The final deliverable is a Power BI dashboard presenting the analysis results.
### Objectives
- Identify Customer Segments
- Classify customers based on subscription types, duration, and region to gain insights into different customer groups.
- Understand Subscription Patterns
- Analyze subscription trends, including the average duration of subscriptions and the most popular subscription types, to determine customer preferences.
- Analyze Cancellations and Renewals
- Investigate cancellation rates by region, subscription type, and duration, identifying factors that influence cancellations and renewals.
- Monitor Revenue by Subscription Type
- Calculate total revenue generated from each subscription type to determine which plans are most profitable.
- Develop Interactive Data Visualizations
- Create a Power BI dashboard with interactive elements to allow stakeholders to explore and analyze customer segments, trends, and behaviors dynamically.
- Provide Actionable Insights
- Deliver insights that can guide decision-making for customer retention strategies, targeted marketing, and product offerings based on customer segment analysis.
### Project Structure
The project consists of three main components: Excel Data Exploration, SQL Query Analysis, and Power BI Dashboard.
###### Folders and Files
- https://github.com/MercyArik/Customer-Segmentation-for-a-Subscription-Service/blob/main/Customerrawdata.xlsx /data: Contains raw sales data used for this analysis.
- https://github.com/MercyArik/Customer-Segmentation-for-a-Subscription-Service/blob/main/CustomerReports.xlsx/Excel Reports: Contains Excel summary tables and initial analysis.
- https://github.com/MercyArik/Customer-Segmentation-for-a-Subscription-Service/blob/main/Customerdata.png/PowerBI: Contains Power BI files for the dashboard.
### Tools Used
The following applications was used for data cleaning, analysis, manipulation and visualizations:
- Microsoft Excel: Data cleaning, exploration and initial metrics calculation.
- SQL Server DB: for running SQL queries
- PowerBI: for interactive data visualization dashboard
- Github to document my portfolio

### Data Cleaning & Preprocessing
The preprocessing stage took the following shape:
1. Data loading and inspection
2. Removing duplicates and blanks (null vales)
3. Data cleaning and formatting
4. Transformation of the variables and datatypes
### Exploratory Data Analysis 
The EDA will be use to explore the data while providing answer to the following questions about subscription pattern such as;
- Retrieve the total number of customers from each region.
- Find the most popular subscription type by the number of customers.
- Find customers who canceled their subscription within 6 months.
- Calculate the average subscription duration for all customers.
- Find customers with subscriptions longer than 12 months.
- Calculate total revenue by subscription type.
- Find the top 3 regions by subscription cancellations.
- Find the total number of active and canceled subscriptions.
### Data analysis
Descriptive Statistics: Initial insights, like average sales, top-selling products, and monthly trends using
Pivot Tables, Excel formula and SQL queries.
```Excel
=DATEDIF(E2,F2,"d")
=AVERAGE(I:I)
=RANK.EQ(COUNTIF(D:D,D2), COUNTIF(D:D,D:D),0)
```
##### Pivot Tables
![PIVOT1](https://github.com/user-attachments/assets/12b70964-b8a4-40b2-901f-482169fe23a3)
![PIVOT2](https://github.com/user-attachments/assets/bd7d357d-648b-4cb4-9b4f-7950392320c7)
![PIVOT3](https://github.com/user-attachments/assets/18d16f2f-de6d-473f-b7fb-204948ac10ff)
![PIVOT4](https://github.com/user-attachments/assets/3a822297-24aa-4cd1-b4aa-244bde4a35cc)
### Key Findings and Insights from Pivot Tables

1. **Popular Subscription Type**
   - The **Basic** subscription type is the most popular among users.

2. **Average Subscription Duration**
   - The average subscription duration is **365.35 days**, indicating that no subscription exceeds one year.

3. **Revenue Insights**
   - The **highest revenue** recorded is **$33,776,735** from the Basic subscription type.

4. **Revenue by Region**
   - **East Region**: Highest revenue
   - **North Region**: Second highest revenue
   - **South Region**: Third highest revenue
   - **West Region**: Lowest subscription revenue

These insights can help guide future marketing strategies and product offerings by focusing on the most popular subscription type and regions with the highest revenue.
#### SQL Queries
```
select * from CustomerData$


--- to delete empty columns---
alter table customerdata$
drop column f11, f12, f13, f14, f15, f16

--- to delete duplicates---

WITH DuplicateRows AS (
select
ROW_NUMBER() OVER (PARTITION BY CustomerName, Region, SubscriptionType, SubscriptionStart, SubscriptionEnd, Canceled, Revenue, SubscriptionDuration,Canceled_Count 
ORDER BY CustomerID) AS RowNum
FROM Customerdata$
)
DELETE FROM DuplicateRows
WHERE RowNum > 1;


---retrieve the total number of customers from each region---
SELECT Region, COUNT(CustomerID) AS Number_of_Customers FROM customerdata$ GROUP BY Region

--Find the most popular subscription type by the number of customers---

select top 1
subscriptionType,
count (customerID) as Number_of_customers
from customerdata$
group by subscriptionType
order by Number_of_customers desc;


---Find customers who canceled their subscription within 6 months---

select 
customerID,
subscriptionstart,
subscriptionend
from customerdata$
where SubscriptionEnd is not null
and datediff(month, subscriptionstart, subscriptionend) <=6;


---calculate the average subscription duration for all customers---

select avg(datediff(day, subscriptionstart,
coalesce(subscriptionend,
getdate()))) as Avg_Subscription_Days,
avg(datediff(month, subscriptionstart,
coalesce(subscriptionend,
getdate()))) as Avg_Subscription_Months,
avg(datediff(Year, subscriptionstart,
coalesce(subscriptionend,
getdate()))) as Avg_Subscription_Year
from customerdata$;

---Find customers with subscriptions longer than 12 months---

select 
customerID,
canceled
subscriptionstart,
subscriptionend,
datediff(month, subscriptionstart, subscriptionend) as months_subscribed
from customerdata$
where subscriptionend is not null
and datediff(month, subscriptionstart, subscriptionend) >12
order by months_subscribed;

---Calculate total revenue by subscription type---

select 
Subscriptiontype,
sum(revenue) as Total_Revenue
from customerdata$
group by subscriptiontype
order by Total_Revenue desc;


---Find the top 3 regions by subscription cancellations---
select top 3
Region,
count(canceled) as Cancellation_count
from customerdata$
where canceled ='1'
group by region
order by Cancellation_count desc;


---Find the total number of active and canceled subscriptions---

select
Sum(case when Canceled=1 then 1 end) as TotalCanceledSubscriptions,
Sum(case when Canceled=0 then 1 end) as TotalActiveSubscriptions
from customerdata$;
```
### Data Visualization
The following is the visual generated using the SQL queries in PowerBI

#### PowerBI Dashboard
![CustData](https://github.com/user-attachments/assets/0cf0df65-2bee-4241-bd08-a44503b37170)


### Key Findings & Insights
##### Summary of Results: 
- With 17,000 (Seventeen Thousand) subscribers using the Basic type of subscription, the most popular subscription type is Basic.
- The top three regions by cancellation count is the North, South and West respectively.
- The total active subscription  is higher than the number of canceled subscription (Active:18612, Canceled:15175)
##### Implications: 
- Popularity of Basic Subscription: The Basic subscription is clearly the most popular among users, indicating a strong market preference. This could suggest that the features or pricing of the Basic plan align well with subscriber needs.

- Cancellation Trends by Region: The highest cancellation rates in the North, South, and West regions might indicate localized issues or dissatisfaction. Understanding the specific reasons behind cancellations in these areas could be critical.

- Overall Subscriber Growth: With more active subscriptions than cancellations, the service is experiencing net growth, which is a positive indicator of overall health. However, the high cancellation count still warrants attention to improve retention.
### Recommendations
- Enhance Basic Subscription Features: Since the Basic subscription is the most popular, consider enhancing its features or value proposition to retain subscribers. Collect feedback to identify desired improvements.

- Investigate Regional Cancellations: Conduct targeted surveys or focus groups in the North, South, and West regions to gather insights into the reasons for cancellations. This will help tailor solutions to specific issues faced by users in those areas.

- Retention Strategies: Develop and implement retention strategies, such as personalized communication, loyalty rewards, or special promotions aimed at subscribers in regions with high cancellation rates.

- Monitor Subscription Metrics: Regularly track subscription growth and cancellation rates to identify trends over time. This data will help in making informed decisions and adjusting strategies proactively.

- Explore Upsell Opportunities: For existing Basic subscribers, consider targeted marketing for higher-tier subscriptions, highlighting features that may appeal to users looking for more value.
### Project Summary
By addressing the issues of cancellation while enhancing the popular Basic subscription, you can strengthen subscriber loyalty and ensure sustained growth.
### References
Data Source(s): Data authored by The Incubator Hub

