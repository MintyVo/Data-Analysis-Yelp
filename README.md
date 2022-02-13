# Profiling and Analyzing the Yelp Dataset
For this case study, I will use SQL to both answer specific questions for an organization and make inferences based on your discoveries. I will use a dataset from a US-based organization called Yelp, which provides a platform for users to provide reviews and rate their interactions with a variety of organizations – businesses, restaurants, health clubs, hospitals, local governmental offices, charitable organizations, etc. Yelp has made a portion of this data available for personal, educational, and academic purposes.
In the first part, I will answer a series of questions regarding the data to help profiling and better understanding the data. I will come up with my own inferences and analysis of the data for each particular questions in the second part. 

### This is Yelp Dataset ER Diagram
![This is Yelp Dataset ER Diagram](./Yelp.png)

## Part 1: Yelp Dataset Profiling and Understanding
1. Profile the data by finding the total number of records for each of the tables below:

i. Attribute table = 10000  
ii. Business table = 10000  
iii. Category table = 10000  
iv. Checkin table = 10000  
v. elite_years table = 10000  
vi. friend table = 10000  
vii. hours table = 10000  
viii. photo table = 10000  
ix. review table = 10000  
x. tip table = 10000  
xi. user table = 10000  

2. Find the total distinct records by either the foreign key or primary key for each table. If two foreign keys are listed in the table, please specify which foreign key.

i. Business = 10000(id)  
ii. Hours = 1562 (business_id)  
iii. Category = 2643 (business_id)  
iv. Attribute = 1115 (business_id)  
v. Review =10000(id)  
vi. Checkin = 493  
vii. Photo = 10000 (id)  
viii. Tip = 537 (user_id)  
ix. User = 10000 (id)  
x. Friend = 11(user_id)  
xi. Elite_years = 2780 (user_id)  

*Note: Primary Keys are denoted in the ER-Diagram with a yellow key icon.*

3. Are there any columns with null values in the Users table? Indicate "yes," or "no."

Answer: No

```sql
SELECT *
FROM user
WHERE id = NULL or name = NULL or review_count = NULL or yelping_since = NULL or useful = NULL or funny = NULL or cool = NULL or fans = NULL or average_stars = NULL or compliment_hot = NULL or compliment_more = NULL or compliment_profile = NULL or compliment_cute = NULL or compliment_list = NULL or compliment_note = NULL or compliment_plain = NULL or compliment_cool = NULL or compliment_funny = NULL or compliment_writer = NULL or compliment_photos = NULL;
```

4. For each table and column listed below, display the smallest (minimum), largest (maximum), and average (mean) value for the following fields:

	i. Table: Review, Column: Stars
	
		min:	1	max: 5		avg: 3.7082
		
	
	ii. Table: Business, Column: Stars
	
		min:1		max:	5	avg: 3.6549
		
	
	iii. Table: Tip, Column: Likes
	
		min:	0	max:	2	avg: 0.0144
		
	
	iv. Table: Checkin, Column: Count
	
		min:	1	max:	53	avg: 1.9414
		
	
	v. Table: User, Column: Review_count
	
		min:	0	max:	2000	avg: 24.2995

5. List the cities with the most reviews in descending order:

```sql
SELECT city, 
	sum(review_count) 
FROM business
GROUP BY city
ORDER BY 2 DESC;
```

|city             | sum (review_count) |
|-----------------|--------------------|
| Las Vegas       |              82854 |
| Phoenix         |              34503 |
| Toronto         |              24113 |
| Scottsdale      |              20614 |
| Charlotte       |              12523 |
| Henderson       |              10871 |
| Tempe           |              10504 |
| Pittsburgh      |               9798 |
| Montréal        |               9448 |
| Chandler        |               8112 |
| Mesa            |               6875 |
| Gilbert         |               6380 |
| Cleveland       |               5593 |
| Madison         |               5265 |


6. Find the distribution of star ratings to the business in the following cities:
i. Avon

```sql
SELECT name,
	stars, 
	review_count
FROM business
WHERE city = 'Avon';
```

| name                                          | stars | review_count |
|-----------------------------------------------|-------|--------------|
| Helen & Kal's                                 |   2.5 |            3 |
| Marc's                                        |   4.0 |            4 |
| Hoban Pest Control                            |   5.0 |            3 |
| Light Salon & Spa                             |   3.5 |            7 |
| Portrait Innovations                          |   1.5 |           10 |
| Winking Lizard Tavern                         |   3.5 |           31 |
| Dervish Mediterranean & Turkish Grill         |   4.5 |           31 |
| Mulligans Pub and Grill                       |   3.5 |           50 |
| Mr. Handyman of Cleveland's Northwest Suburbs |   2.5 |            3 |
| Cambria hotel & suites Avon - Cleveland       |   4.0 |           17 |


ii. Beachwood

```sql
SELECT name,
	stars, 
	review_count
FROM business
WHERE city = ‘Beachwood’;
```

| name                            | stars | review_count |
|---------------------------------|-------|--------------|
| Maltz Museum of Jewish Heritage |   3.0 |            8 |
| Charley's Grilled Subs          |   3.0 |            3 |
| Sixth & Pine                    |   4.5 |           14 |
| Beechmont Country Club          |   5.0 |            6 |
| Hyde Park Prime Steakhouse      |   4.0 |           69 |
| Origins                         |   4.5 |            3 |
| Fyodor Bridal Atelier           |   5.0 |            4 |
| College Planning Network        |   2.0 |            8 |
| Lucky Brand Jeans               |   3.5 |            3 |
| American Eagle Outfitters       |   3.5 |            3 |
| Shaker Women's Wellness         |   5.0 |            6 |
| Avis Rent A Car                 |   2.5 |            3 |
| Cleveland Acupuncture           |   5.0 |            3 |
| Studio Mz                       |   5.0 |            4 |

7. Find the top 3 users based on their total number of reviews:

```sql
SELECT id, 
	name,
	review_count
FROM user
ORDER BY review_count DESC
LIMIT 3;
```

| id                     | name   | review_count |
|------------------------|--------|--------------|
| -G7Zkl1wIWBBmD0KRy_sCw | Gerald |         2000 |
| -3s52C4zL_DHRK0ULG6qtg | Sara   |         1629 |
| -8lbUNlXVSoXqaRRiHiSNg | Yuri   |         1339 |


8. Does posing more reviews correlate with more fans?
There is no correlation between reviews and fans 

```sql
SELECT id,
	name, 
	review_count, 
	fans
FROM user
ORDER BY review_count DESC;
```

| id                     | name      | review_count | fans |
|------------------------|-----------|--------------|------|
| -G7Zkl1wIWBBmD0KRy_sCw | Gerald    |         2000 |  253 |
| -3s52C4zL_DHRK0ULG6qtg | Sara      |         1629 |   50 |
| -8lbUNlXVSoXqaRRiHiSNg | Yuri      |         1339 |   76 |
| -K2Tcgh2EKX6e6HqqIrBIQ | .Hon      |         1246 |  101 |
| -FZBTkAZEXoP7CYvRV2ZwQ | William   |         1215 |  126 |
| --2vR0DIsmQ6WfcSzKWigw | Harald    |         1153 |  311 |
| -gokwePdbXjfS0iF7NsUGA | eric      |         1116 |   16 |
| -DFCC64NXgqrxlO8aLU5rg | Roanna    |         1039 |  104 |
| -8EnCioUmDygAbsYZmTeRQ | Mimi      |          968 |  497 |
| -0IiMAZI2SsQ7VmyzJjokQ | Christine |          930 |  173 |
| -fUARDNuXAfrOn4WLSZLgA | Ed        |          904 |   38 |
| -hKniZN2OdshWLHYuj21jQ | Nicole    |          864 |   43 |
| -9da1xk7zgnnfO1uTVYGkA | Fran      |          862 |  124 |
| -B-QEUESGWHPE_889WJaeg | Mark      |          861 |  115 |
| -kLVfaJytOJY2-QdQoCcNQ | Christina |          842 |   85 |
| -kO6984fXByyZm3_6z2JYg | Dominic   |          836 |   37 |
| -lh59ko3dxChBSZ9U7LfUw | Lissa     |          834 |  120 |
| -g3XIcCb2b-BD0QBCcq2Sw | Lisa      |          813 |  159 |
| -l9giG8TSDBG1jnUBUXp5w | Alison    |          775 |   61 |
| -dw8f7FLaUmWR7bfJ_Yf0w | Sui       |          754 |   78 |
| -AaBjWJYiQxXkCMDlXfPGw | Tim       |          702 |   35 |
| -jt1ACMiZljnBFvS6RRvnA | L         |          696 |   10 |
| -IgKkE8JvYNWeGu8ze4P8Q | Angela    |          694 |  101 |
| -hxUwfo3cMnLTv-CAaP69A | Crissy    |          676 |   25 |
| -H6cTbVxeIRYR-atxdielQ | Lyn       |          675 |   45 |

*(Output limit exceeded, 25 of 10000 total rows shown)*

9. Are there more reviews with the word "love" or with the word "hate" in them?
There is more reviews with the world “love” than the one with world “hate”

```sql
SELECT count(*) AS LoveWord
FROM review
WHERE text LIKE "%love%";
```

| count (text) |
|--------------|
|         1780 |

```sql
SELECT count(*) AS HateWord
FROM review
WHERE text LIKE "%hate%";
```

| count (text) |
|--------------|
|          232 |

10. Find the top 10 users with the most fans:

```sql
SELECT id, 
	name,
 	fans
FROM user
ORDER BY fans DESC
LIMIT 10;
```

| id                     | name      | fans |
|------------------------|-----------|------|
| -9I98YbNQnLdAmcYfb324Q | Amy       |  503 |
| -8EnCioUmDygAbsYZmTeRQ | Mimi      |  497 |
| --2vR0DIsmQ6WfcSzKWigw | Harald    |  311 |
| -G7Zkl1wIWBBmD0KRy_sCw | Gerald    |  253 |
| -0IiMAZI2SsQ7VmyzJjokQ | Christine |  173 |
| -g3XIcCb2b-BD0QBCcq2Sw | Lisa      |  159 |
| -9bbDysuiWeo2VShFJJtcw | Cat       |  133 |
| -FZBTkAZEXoP7CYvRV2ZwQ | William   |  126 |
| -9da1xk7zgnnfO1uTVYGkA | Fran      |  124 |
| -lh59ko3dxChBSZ9U7LfUw | Lissa     |  120 |

## Part 2: Inferences and Analysis
1. Pick one city and category of your choice and group the businesses in that city or category by their overall star rating. Compare the businesses with 2-3 stars to the businesses with 4-5 stars and answer the following questions. Include your code.

I choose Toronto and restaurants category . This is the table I get from my SQL:

| star      | name          | city    | category    | hours                | review_count |
|-----------|---------------|---------|-------------|----------------------|--------------|
| below 2   | 99 Cent Sushi | Toronto | Restaurants | Saturday|11:00-23:00 |            5 |
| 2-3 stars | Pizzaiolo     | Toronto | Restaurants | Saturday|10:00-4:00  |           34 |
| 3-4 stars | Edulis        | Toronto | Restaurants | Saturday|18:00-23:00 |           89 |
| 4-5 stars | Sushi Osaka   | Toronto | Restaurants | Saturday|11:00-23:00 |            8 |

i. Do the two groups you chose to analyze have a different distribution of hours?  
Yes.

ii. Do the two groups you chose to analyze have a different number of reviews?  
Yes.

iii. Are you able to infer anything from the location data provided between these two groups? Explain.  
No.

```sql
SELECT CASE WHEN stars > 4.0 THEN '4-5 stars'
            WHEN stars > 3.0 THEN '3-4 stars'
            WHEN stars > 2.0 THEN '2-3 stars'
            ELSE 'below 2' END AS star,     
    business.name,
    business.city,
    category.category,
    hours.hours,
    business.review_count,
FROM (business inner join category ON business.id = category.business_id) INNER JOIN hours ON hours.business_id = category.business_id
WHERE business.city = 'Toronto' AND category = "Restaurants"
GROUP BY business.stars;
```

2. Group business based on the ones that are open and the ones that are closed. What differences can you find between the ones that are still open and the ones that are closed? List at least two differences and the SQL code you used to arrive at your answer.

|  AVG(b.stars) | SUM(b.review_count) | AVG(b.review_count) | COUNT(r.useful) + COUNT(r.funny) | is_open |
|---------------|---------------------|---------------------|----------------------------------|---------|
|           2.0 |                   4 |                 4.0 |                                2 |       0 |
| 2.96153846154 |                 504 |       38.7692307692 |                               26 |       1 |

i. Difference 1:  
The group which are still open has more stars

ii. Difference 2:  
The group which are closed has more reviews 

```sql
SELECT AVG(b.stars),
	  SUM(b.review_count),
	  AVG(b.review_count),
	  COUNT(r.useful) + COUNT(r.funny),
	   is_open
FROM business b INNER JOIN review r ON b.id  =  r.id
GROUP BY b.is_open;
```

3. For this last part of your analysis, you are going to choose the type of analysis you want to conduct on the Yelp dataset and are going to prepare the data for analysis.

Ideas for analysis include: Parsing out keywords and business attributes for sentiment analysis, clustering businesses to find commonalities or anomalies between them, predicting the overall star rating for a business, predicting the number of fans a user will have, and so on. These are just a few examples to get you started, so feel free to be creative and come up with your own problem you want to solve. Provide answers, in-line, to all of the following:

i. Indicate the type of analysis you chose to do:  
I would like to study the preference among different types of food on Yelp..

ii. Write 1-2 brief paragraphs on the type of data you will need for your analysis and why you chose that data:  

I choose several popular types of food which are Chinese, Korean, Vietnamese, Japanese, Italian, French, and Mexican. I analyze their star ratings and numbers of reviews so I can get some insights on which type of food is more popular on Yelp  

iii. Output of your finished dataset:

| category   | Number_Of_Restaurants | AVG(b.stars) | AVG(b.review_count) |
|------------|-----------------------|--------------|---------------------|
| Korean     |                     2 |         4.25 |                31.5 |
| French     |                     2 |          4.0 |               128.5 |
| Japanese   |                     5 |          3.8 |                30.4 |
| Italian    |                     2 |          3.5 |                74.0 |
| Mexican    |                     7 |          3.5 |       46.7142857143 |
| Vietnamese |                     1 |          3.5 |                62.0 |
| Chinese    |                     4 |        3.125 |               199.0 |

iv. Provide the SQL code you used to create your final dataset:

```sql
SELECT c.category,
	COUNT(b.name) AS Number_Of_Restaurants,
	AVG(b.stars),
	AVG(b.review_count),
FROM business b INNER JOIN category c ON c.business_id = b.id 
WHERE c.category IN ("Chinese", "Mexican", "French", "Italian", "Korean", "Japanese", "Vietnamese") 
GROUP BY c.category 
ORDER BY AVG(b.stars) DESC;
```