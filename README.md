# Food_Ratings_Analysis
Challenge 12
## Part 1: Database and Jupyter Notebook
- In Git Bash, CD to the 'Resource' folder and mongoimport --db uk_food --collection establishments --file establishments.json --jsonArray --drop.
- Import dependencies, create an instance of MongoClient, list the database, assign the uk_food database to a variable name, and list the collections in the uk_food database.
- Using find_one() to find and display one document in the establishments collection.
## Part 2: Update the Database
- Create a dictionary for the new restaurant data and insert the new restaurant into the collection using insert_one().
- Find the BusinessTypeID for "Restaurant/Cafe/Canteen" and return only the `BusinessTypeID` and `BusinessType` fields by defining the projection to return only the BusinessTypeID and BusinessType fields.
- Update the new restaurant using the update_one().
- Find the documents that have LocalAuthorityName as 'Dover' using the count_documents(() and delete all documents where LocalAuthorityName is "Dover" using delete_many().
- Use `update_many` to convert `latitude` and `longitude` to decimal numbers ($toDouble).
- Use `update_many` to convert `RatingValue` to integer numbers ($toInt).
## Part 3: Exploratory Analysis
### Which establishments have a hygiene score equal to 20?
- Find the establishments with a hygiene score of 20 by setting query = {"scores.Hygiene": 20}.
- Convert the result to a Pandas DataFrame by converting the fetched data into a list of dictionaries and create a DataFrame from the list of dictionaries.
### Which establishments in London have a `RatingValue` greater than or equal to 4?
- Find the establishments with London as the Local Authority and has a RatingValue greater than or equal to 4 by setting the query = {"LocalAuthorityName": {"$regex": "London", "$options": "i"}, "RatingValue": {"$gte": 4}}. Use $regex pattern to match against the "LocalAuthorityName" field and $gte operator stands for greater than or equal to.
- Convert the result to a Pandas DataFrame using the methodology mentioned previously.
### What are the top 5 establishments with a `RatingValue` rating value of 5, sorted by lowest hygiene score, nearest to the new restaurant added, "Penang Flavours"?
- Search within 0.01 degree on either side of the latitude and longitude by $gte latitude/longitude + degree_search (degree) and $lte latitude/longitude - degree_search (degree). Sort by hygiene score by using pymongo.ASCENDING. Print the 5 results by setting limit = 5.
- Convert the result to a Pandas DataFrame using the methodology mentioned previously.
### How many establishments in each Local Authority area have a hygiene score of 0?
- Match establishments with a hygiene score of 0 ({"$match": {"scores.Hygiene": 0}}), group the matches by Local Authority ({"$group": {"_id": "$LocalAuthorityName", "count": {"$sum": 1}}}), and sort and match from highest to lowest ({"$sort": {"count": pymongo.DESCENDING}}).
- Use the aggregation method (result = list(db.establishments.aggregate(pipeline))). Using StackOverflow: https://stackoverflow.com/questions/59487117/what-is-database-aggregates-in-nosql#:~:text=The%20aggregation%20parameters%20are%20passed,to%20return%20a%20single%20result.
- Convert the result to a Pandas DataFrame using the methodology mentioned previously.
