# Module_one-Homework
Data-engineering-zoomcamp module 1 homework: Docker &amp; SQL

## Question 1. Understanding Docker images
### a. Run docker with the `python:3.13` image. Use an entrypoint `bash` to interact with the container.
#### Solution
```bash
docker run -it --rm --entrypoint=bash python:3.13 
```
This runs an interactive bash shell inside a Python 3.13 container that gets deleted after exiting.  

Understanding each part of the command:
* docker run: Creates and starts a new container
* -it: Two flags combined:
* -i (interactive): Keeps STDIN open so you can type commands
* -t (tty): Allocates a pseudo-terminal for a proper shell experience
* --rm: Automatically removes the container when it exits (cleanup)
* --entrypoint=bash: Overrides the default command the container runs with bash
* python:3.13: The Docker image to use (Python version 3.13)

What happens:
Instead of dropping into a Python interpreter (the default for python images), you get a bash shell where you can run any commands, install packages, explore the filesystem, etc. When you type exit, the container stops and is automatically deleted.  

This is useful for debugging, testing, or exploring what's inside a Python container environment. 


### b. What's the version of `pip` in the image?

- 25.3
- 24.3.1
- 24.2.1
- 23.3.1
  #### Solution
  To check the pip version, we run:
  ```bash
  pip --version
  ```
  This output - 25.3 as the pip version as seen in the image below
  
  <img width="689" height="354" alt="image" src="https://github.com/user-attachments/assets/6c4b7460-5255-4274-98cd-72def1c42040" />

## Question 2. Understanding Docker networking and docker-compose

Given the following `docker-compose.yaml`, what is the `hostname` and `port` that pgadmin should use to connect to the postgres database?

```yaml
services:
  db:
    container_name: postgres
    image: postgres:17-alpine
    environment:
      POSTGRES_USER: 'postgres'
      POSTGRES_PASSWORD: 'postgres'
      POSTGRES_DB: 'ny_taxi'
    ports:
      - '5433:5432'
    volumes:
      - vol-pgdata:/var/lib/postgresql/data

  pgadmin:
    container_name: pgadmin
    image: dpage/pgadmin4:latest
    environment:
      PGADMIN_DEFAULT_EMAIL: "pgadmin@pgadmin.com"
      PGADMIN_DEFAULT_PASSWORD: "pgadmin"
    ports:
      - "8080:80"
    volumes:
      - vol-pgadmin_data:/var/lib/pgadmin

volumes:
  vol-pgdata:
    name: vol-pgdata
  vol-pgadmin_data:
    name: vol-pgadmin_data
```

- postgres:5433
- localhost:5432
- db:5433
- postgres:5432
- db:5432

#### Solution
we copied the above code block and created a docker-compose.yaml file with it in our project repository and run the command below in our bash terminal from within our project root folder where the docker-compose.yaml file is housed:  
```bash
docker compose up -d
``` 

This uses our docker-compose.yaml file to sets up a dockerize local database environment with two services: PostgreSQL and pgAdmin.  

The pgadmin service (database management tool) Provides a web-based interface to manage our PostgreSQL database which we can access at http://localhost:8080 from our browser!   

Using Login credentials as defined in our docker-compose.yaml file
* email: pgadmin@pgadmin.com,
* password: pgadmin  


we were then able to Connect to our postgres database from pgAdmin (the web interface at localhost:8080), using the parameters:

* Host: db (not localhost or 127.0.0.1, because pgAdmin is inside Docker)  

* Port: 5432 (not 5433, because inside Docker it uses the internal port)  

* Username: postgres  

* Password: postgres  

