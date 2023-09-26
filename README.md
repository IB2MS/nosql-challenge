# nosql-challenge

# Background 

The UK Food Standards Agency evaluates various establishments across the United Kingdom, and gives them a food hygiene rating. I've been contracted by the editors of a food magazine, Eat Safe, Love, to evaluate some of the ratings data in order to help their journalists and food critics decide where to focus future articles.

##  Part 1: Database and Jupyter Notebook Set Up

The `NoSQL_setup_starter.ipynb` sets up and updates the database. The `NoSQL_analysis_starter.ipynb` queries relevant information for analyses and converts results into Pandas DataFrame. The data provided in the `establishments.json` file was imported using Terminal with `mongoimport --type json -d uk_food -c establishments --drop --jsonArray '/Users/lesya/Desktop/nosql-challenge/Resources/establishments.json`. 

##  Part 2: Update the Database

1.  Insert the new_restaurant opened in Greenwich to the Database.
    ```
    establishments.insert_one(new_restaurant)
    ```
    
2.  Update the new restauarant with the correct BusineesTypeID.
    ```
    establishments.update_one(
        new_restaurant, 
        {'$set': 
            {'BusinessTypeID': 1}
        }
    )
    ```
    
* Drop all establishments that has Dover as their Local Authority from the database. 
    ```
    establishments.delete_many({'LocalAuthorityName': 'Dover'})
    ```
    
* Used update_many to convert latitude and longitude to decimal numbers.
    ```
    establishments.update_many({}, [ {'$set': { 'geocode.longitude': {'$toDouble': "$geocode.longitude"},
                                                "geocode.latitude" : {'$toDouble': "$geocode.latitude"} } } ])
                      
    ```

    ## Exploratory Analysis
1. Which establishments have a hygiene score equal to 20?
   
   There are 41 establishments with a hygiene score of 20 from the `uk_food` dataset.
   
2. Which establishments in London have a RatingValue greater than or equal to 4?

    There are 33 establishments in London that have a RatingValue greater than or equal to 4 from the `uk_food` dataset.

3. What are the top 5 establishments with a RatingValue rating value of 5, sorted by lowest hygiene score, nearest to the new restaurant added, "Penang Flavours"?
4. 
   TOP 5 establishments with a RatingValue of '5' sorted by lowest hygiene score nearest to "Penang Flavours": 
   -- 'Howe and Co Fish and Chips - Van 17'
   -- 'Atlantic Fish Bar'
   -- 'Plumstead Manor Nursery'
   -- 'Iceland'
   -- 'Volunteer'
   
   
5. How many establishments in each Local Authority area have a hygiene score of 0?
   
   Number of documents in result:  55
   There are 55 rows (Number of documents) in the DataFrame. 10 rows: 
    | _id | count |
    |-----|-------|
    |Thanet|1130|
    |Greenwich|882|
    |Maidstone|713|
    |Newham|711|
    |Swale|686|
    |Chelmsford|680|
    |Medway|672|
    |Bexley|607|
    |Southend-On-Sea|586|
    |Tendring|542|
