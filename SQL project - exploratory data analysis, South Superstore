--Project: South Superstore Analysis 

--Outline:
--STEP ONE: Create tables for main data, rewards customers, and targets; upload data
--STEP TWO: Analyze data (data manipulation, joins)

--Note: 
--This data can be found online, through a "South Superstore Data" search

--STEP ONE: 
--Create tables for main data, rewards customers, and targets 
--Upload data 
create table superstore_main
(row_id integer,
 order_id character varying (250),
 order_date date,
 ship_date date, 
 ship_mode character varying (250), 
 customer_id character varying (250), 
 customer_name character varying (250), 
 segment character varying (250),
 country character varying (250), 
 city character varying (250), 
 state character varying (250),
 zip integer,
 region character varying (250),
 product_id character varying (250),
 category character varying (250),
 sub_category character varying (250),
 product_name character varying (250),
 sales numeric (7,2),
 quantity numeric (7,2),
 discount numeric (7,2),
 profit numeric (7,2),
 order_year integer)

copy superstore_main (row_id, order_id, order_date, ship_date, ship_mode, customer_id, 
					 customer_name, segment, country, city, state, zip, region, product_id,
					 category, sub_category, product_name, sales, quantity, discount, 
					 profit, order_year)
from 'C:\Users\Public\Documents\superstore_main.csv'
delimiter ','
csv header;

select * 
from superstore_main

create table superstore_target 
(sub_category character varying (250),
 order_year integer,
 sales_target numeric (7,2),
 profit_target numeric (7,2))
 
 copy superstore_target (sub_category, order_year, sales_target, profit_target)
from 'C:\Users\Public\Documents\superstore_target.csv'
delimiter ','
csv header;

select * 
from superstore_target

create table superstore_rewards
(customer character varying (250),
 customer_name character varying (250))
 
 alter table superstore_rewards
 rename column "customer_name" to "customer_name_r"
 
copy superstore_rewards (customer, customer_name)
from 'C:\Users\Public\Documents\superstore_rewards.csv'
delimiter ','
csv header;

select * 
from superstore_rewards


--STEP TWO: 
--Analyze data

--Sum of sales by state 
select  state, sum(sales) as state_sales
from superstore_main 
group by state
order by state_sales desc

--Sum of profit by state
select state, sum(profit) as state_profits
from superstore_main
group by state
order by state_profits desc
--Florida has the highest state_sales value, but negative profit! Interesting. 

--Sum of sales by sub_category and order_year 
select sub_category, order_year, sum(sales) as sub_category_sales
from superstore_main
group by sub_category, order_year
order by sub_category_sales desc
limit 15
--The sub-categories machines, phones and chairs have had relatively high sales over time

--Sum of profit by sub_category and order_year
select sub_category, order_year, sum(profit) as sub_category_profits
from superstore_main
group by sub_category, order_year
order by sub_category_profits desc
limit 15
--In contrast, the most profitable sub-categories over time seem to be Phones and Accessories

--Join rewards customers' data to the main superstore data
create temp table rewards_main_join as 
(select *
from superstore_rewards as sr
left join superstore_main as sm
on sr.customer_name_r = sm.customer_name)

--Explore the new temp table 
select * 
from rewards_main_join

--This shows profit data by sub-category for rewards customers only 
select sub_category, sum (profit) as total_profit
from rewards_main_join
group by  sub_category 
order by total_profit desc


--This uses a case statement to sort profits into arbitrary "High" and "Low" categories
select
	sub_category, 
	order_year, 
	sum(profit) as total_profit,
	case when sum(profit) > 1000 then 'High'
	else 'Low' end as profit_level
from superstore_main as sm 
group by sub_category, order_year
order by order_year 

--Data from states with highest and lowest profits, as determined above 
select *
from superstore_main as sm
where sm.state in ('Virginia', 'North Carolina')

--This lists profitable states, as determined by an arbitrary threshold
select sm.state, sum(sales) as total_sales, sum(profit) as total_profit   
from superstore_main as sm
group by sm.state
having sum(profit) > 2500
order by total_profit desc 

--Data on orders with the 'First Class' ship_mode 
select sm.order_year, sm.category, count(*) as first_class_count
from superstore_main as sm
where order_id in 
	(select order_id
	 from superstore_main
	 where ship_mode = 'First Class')
group by sm.order_year, sm.category
order by sm.order_year

select * from superstore_main
