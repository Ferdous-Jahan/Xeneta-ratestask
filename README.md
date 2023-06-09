# Overview

I have chosen Django rest framework as I have in depth knowledge of this framework. I have used the docker postgres instance that was supplied with the task. I have only used raw sql queries rather than ORM as instructed in the task description.

## My understanding from the requirements
My approach was if origin or destination was region then get that region and its child regions recursively and then get ports of those regions. If origin or destination is port code then use that one directly.

After getting origin and destination ports filter the rows from prices table within the date range having the origin and destination ports. Then make the response JSON according to requirements with average price per day within the date range from query params.

## Project setup

### Docker setup for postgres instance

* Run the existing docker that came with the task git repo [here](https://github.com/xeneta/ratestask)
* build docker container : ```docker build -t ratestask .```
* run container : ```docker run -p 0.0.0.0:5432:5432 --name ratestask ratestask```
* Get into exposed postgres instance by this command:

```docker exec -e PGPASSWORD=ratestask -it ratestask psql -U postgres```

 and run this query : 

```ALTER TABLE prices ADD COLUMN id serial PRIMARY KEY;```
	
This step is needed as the prices table has no primary key. Django rest framework can not run queries on rows without primary key.


### Run the API
* Clone the repo
* Create venv : ```python -m venv venv```
* Get inside venv. Command for git bash : ```source venv\Scripts\activate```
* Install packages : ```pip install -r requirements.txt```
* Run these commands one by one

    ```python manage.py makemigrations ratestask```

    ```python manage.py ratestask migrate --fake-initial```

    ```python manage.py runserver```

N.B : Fake initial migration is because I have used already existing table models.

* Now the program will be running on ```http://127.0.0.1:8000/```

### Run test cases
* Run this command for test cases : ``` python manage.py test ```
## Additional notes

I have spent around 12 hours on this task. Figuring out how to migrate the prices table with primary key 'id' as it had no primary key when created and I have to use fake initial migration as I am using the existing table took a good amount of time from these hours. It was overall a challenging and fun task. 

