1) Your task is to determine the number of unique users who accessed the website.

<details>
<summary>Click to expand the Answer!</summary>
  
```sql
select count(distinct user_id) from users;
```
</details>


|count(distinct user_id)|
|-----------------------|
|500|

2) Your task is to calculate the average number of cookies all users have on the platform?
<details>
<summary>Click to expand the Answer!</summary>

```sql
with cte as(
select user_id,count(cookie_id) as cc  from users
group by user_id
) 
select round(avg(cc)) average_cookies_per_user
from cte ;
```
</details>

|average_cookies_per_user | 
|-------------------------|
|4|

3) Your role is to derive the unique number of visits by all users for each month?
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select month(event_time) Month,
count(distinct visit_id) unique_visit_count
from events
where event_type=1
group by Month;
```
</details>

|Month|unique_visit_count|
|---------------|---------|
|1	|876|
|2	|1488|
|3	|916|
|4	|248|
|5	|36|


4) Your role is to derive the unique number of visits by all users for each month.

<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select ef.event_name,count(e.visit_id) event_count
from events e inner join event_identifier ef 
on e.event_type=ef.event_type
group by 1;
```
</details>

|event_name|event_count|
|---------------|---------|
|Page View |20928|
|	Add to Cart |8451|
|	Purchase |1777|
| Ad Impression|876|
|Ad Click |702|

5) Your task is to determine the percentage of visits that view the checkout page but do not have a purchase event.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
with cte as
(
select
sum(case when event_type= 1 and page_id=12 then 1 else 0 end) as checkout,
sum(case when event_type= 3 then 1 else 0 end) as purchase
from events
)
select 
(1-purchase/checkout)*100 as percentage_checkout_view_with_no_purchase
from cte;
```
</details>

| percentage_checkout_view_with_no_purchase |
|---------------|
| 15.5017 |

6) You are entrusted with the task of identifying the top 3 pages on the platform that have garnered the most views.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select ph.page_name,count(*)
from events e inner join page_hierarchy ph
on e.page_id=ph.page_id
where e.event_type=1
and ph.product_id is not null
group by ph.page_name 
order by count(*) desc limit 3;
```
</details>

|page_name|count(*)|
|---------------|---------|
|Crab |	1564 |
| Oyster |1568 |
| Russian Caviar | 1563 |

7) Your task is to analyse the number of views and cart adds for each product category.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select ph.product_category,
sum(case when e.event_type=1 then 1 else 0 end) page_views,
sum(case when e.event_type=2 then 1 else 0 end) cart_add
from events e inner join page_hierarchy ph
on e.page_id=ph.page_id
where ph.product_id is not null
group by 1 order by 1 desc;
```
</details>

| product_category | page_views | cart_add |
|-------------------|------------|----------|
| Shellfish         | 6204       | 3792     |
| Luxury            | 3032       | 1870     |
| Fish              | 4633       | 2789     |

8) What is the percentage of visits which have a purchase event?

<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select round(sum(case when event_type="3" then
1 else 0 end)/count(distinct visit_id),3)*100 as purchase_percentage
from events;
```
</details>

| purchase_percentage |
|---------------------|
| 49.900 |

9) Your task is to identify the product category that hosts the maximum number of products.

<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select product_category,count(distinct product_id)
from page_hierarchy
where product_category is not null
group by product_category
order by 2 desc limit 1;
```
</details>

| product_category | count(distinct product_id) |
|-------------------|-----------------------------|
| Shellfish         | 4                           |

10) Your task is to analyse user behaviour concerning product views and purchases.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select u.user_id,
sum(case when e.event_type=1 then 1 else 0 end) product_views,
sum(case when e.event_type=3 then 1 else 0 end) purchase
from events e inner join users u
on e.cookie_id=u.cookie_id
group by 1;
```
</details>



11) Your task is to identify the most viewed and least viewed product on the website.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select ph.page_name, ph.product_category,count(*) views
from events e inner join page_hierarchy ph
on e.page_id=ph.page_id
where e.event_type=1
and ph.product_id is not null
group by 1,2 
order by 3 desc limit 1;
```
</details>

| page_name | product_category | views |
|-----------|-------------------|-------|
| Oyster    | Shellfish        | 1568  |

<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select ph.page_name,ph.product_category,count(*) views
from events e inner join page_hierarchy ph
on e.page_id=ph.page_id
where e.event_type=1
and ph.product_id is not null
group by 1,2
order by 3 asc limit 1;
```
</details>

| page_name | product_category | views |
|-----------|-------------------|-------|
| Black Truffle    | Luxury        | 1469  |

12) Your task is to identify the number of page views each campaign generated.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select pci.campaign_name,count(e.visit_id) page_views
from events e inner join page_hierarchy ph
on e.page_id=ph.page_id
inner join campaign_identifier pci 
on pci.product_id=ph.product_id
where e.event_type=1
group by pci.campaign_name
order by 2 desc;
```
</details>

| campaign_name                           | page_views |
|-----------------------------------------|------------|
| "Half Off - Treat Your Shellf(ish)"     | 4636       |
| "BOGOF - Fishing For Compliments"       | 4633       |
| "25% Off - Living The Lux Life"          | 3032       |

13) Your task is to calculate the total number of unique visits for each campaign.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select pci.campaign_name,
count(distinct e.cookie_id) unique_visitors
from events e inner join page_hierarchy ph
on e.page_id=ph.page_id
inner join campaign_identifier pci 
on pci.product_id=ph.product_id
group by pci.campaign_name;
```
</details>

| campaign_name                           | unique_visitors |
|-----------------------------------------|-----------------|
| "25% Off - Living The Lux Life"          | 1523            |
| "BOGOF - Fishing For Compliments"       | 1678            |
| "Half Off - Treat Your Shellf(ish)"     | 1662            |

14) Your task is to calculate the number of events recorded on each day in the "events" table.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select date(event_time) event_date,
count(visit_id) total_events
from events group by 1;
```
</details>

| event_date | total_events |
|------------|--------------|
| 2020-02-04 | 486          |
| 2020-01-18 | 314          |
| 2020-02-21 | 662          |
| 2020-02-22 | 591          |
| 2020-02-01 | 244          |
| 2020-01-25 | 272          |
| 2020-02-09 | 479          |
... (and the rest of the data)

15) Your task is to calculate the total number of Ad Impression and Ad Click generated by all users per month.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select month(event_time) as "Month" ,
sum(case when event_type=4 then 1 else 0 end) as Ad_Impression_Count,
sum(case when event_type=5 then 1 else 0 end) as Ad_Click_Count
from events
group by month(event_time) order by 1;
```
</details>

| Month | Ad_Impression_Count | Ad_Click_Count |
|-------|----------------------|----------------|
| 1     | 216                  | 173            |
| 2     | 371                  | 291            |
| 3     | 214                  | 178            |
| 4     | 63                   | 48             |
| 5     | 12                   | 12             |

16) Your task is to determine the number of sessions each user had on the website.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select user_id,
count(distinct visit_id) session_count
from users u inner join events e 
on u.cookie_id=e.cookie_id
group by 1 order by 2 desc;
```
</details>

17) Your task is to identify the number of users who have made a purchase and order them in descending order based on their order counts.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
select user_id,count(visit_id) no_of_time_purchase
from users u inner join events e 
on u.cookie_id=e.cookie_id
where event_type=3
group by 1 order by 2 desc;
```
</details>

18) Your task is to analyse the user behaviour in terms of visiting the pages and identifying the page that each user visits the most.
<details>
<summary>Click to expand the Answer!</summary>
 
```sql
with cte as(
select u.user_id,ph.page_name,count(*),
dense_rank() over(partition by u.user_id order by count(*) desc) as k
from users u inner join events e on u.cookie_id=e.cookie_id 
inner join page_hierarchy ph on e.page_id=ph.page_id
where ph.product_id is not null
group by 1,2
)
select * from cte where k=1;
```
</details>
