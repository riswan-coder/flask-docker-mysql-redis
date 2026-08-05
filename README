Flask REST Application Docker Deployment

Project Overview

This project containerizes a Flask REST application and deploys it using Docker. The application is connected to a MySQL database and a Redis server. Two application containers are deployed to satisfy the assignment requirements.

Components

1. Flask REST Application (2 Containers)
2. MySQL 8.0 Database Container
3. Redis 7 Container

Docker Image Build

Build the Flask application image:

docker build -t flask-app-image .

Database Setup

Create MySQL container:

docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=root123 -e MYSQL_DATABASE=flaskdb -p 3306:3306 -d mysql:8.0

Redis Setup

Create Redis container:

docker run --name redis-server -p 6379:6379 -d redis:7

Docker Network

Create network:

docker network create flask-network

Connect MySQL and Redis containers to the network:

docker network connect flask-network mysql-db

docker network connect flask-network redis-server

Application Deployment

Run first Flask application container:

docker run -d --name flask-app-1 --network flask-network -p 5000:5000 -e APP_HOST=0.0.0.0 -e APP_PORT=5000 -e REDIS_HOST=redis-server -e REDIS_PORT=6379 -e DATABASE_URI="mysql+pymysql://root:root123@mysql-db:3306/flaskdb" flask-app-image

Run second Flask application container:

docker run -d --name flask-app-2 --network flask-network -p 5001:5000 -e APP_HOST=0.0.0.0 -e APP_PORT=5000 -e REDIS_HOST=redis-server -e REDIS_PORT=6379 -e DATABASE_URI="mysql+pymysql://root:root123@mysql-db:3306/flaskdb" flask-app-image

Database Initialization

Create application tables:

docker exec -it flask-app-1 python create_db.py

Testing

1. Verify application:

curl http://localhost:5000/

Expected Output:

Hello, World!

2. Verify application readiness:

curl http://localhost:5000/status

Expected Output:

OK

3. Verify Redis connectivity:

curl http://localhost:5000/redis-hits

Expected Output:

Redis hits 1

Subsequent requests increase the counter value.

4. Verify palindrome endpoint:

curl http://localhost:5000/palindrom/radar

Expected Output:

Text is palindrom

5. Verify second application instance:

curl http://localhost:5001/

Expected Output:

Hello, World!

Verification

Check running containers:

docker ps

Expected running containers:

* mysql-db
* redis-server
* flask-app-1
* flask-app-2

Issue Encountered and Resolution

Issue:
Flask-SQLAlchemy failed to initialize because the application was reading DATABASE_URI but was not assigning it to SQLALCHEMY_DATABASE_URI.

Resolution:
Updated app.py to configure:

app.config['SQLALCHEMY_DATABASE_URI'] = connection_string
app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False

before initializing SQLAlchemy.

Result

The application was successfully containerized, connected to MySQL and Redis, and deployed with two Flask application containers. All required endpoints were tested successfully and returned the expected results.
