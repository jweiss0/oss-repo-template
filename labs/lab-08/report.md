## Example 00
Running `docker run docker/whalesay cowsay boo`:

## Example 01
Running Ubuntu container:

Installing & Running Vim:

Installing & Running Cowsay:

## Example 02
Running Mongo container:

Running Rocket.Chat container:

Rocket.Chat localhost application:

Testing other docker commands:

## Example 03
Created Dockerfile:
```
# Use Python as base image
FROM python:3.5

# Make sure container is up to date
RUN apt-get update

# Install python packages
RUN pip install Flask

# Add file from system to container
ADD . /opt/webapp/

# Add environment variable
ENV FLASK_APP=hello.py

# Set working directory
WORKDIR /opt/webapp

# Expose application port
EXPOSE 5000

# Run application
CMD ["flask", "run", "--host=0.0.0.0"]
```

Building Dockerfile:

Running container:

Application on localhost:

## Example 04
Created Dockerfile:
```
# Use node 10.15.3 LTS
FROM node:10.15.3
ENV LAST_UPDATED 20190325T175400

# Copy source code
COPY . /app

# Change working directory
WORKDIR /app

# Install dependencies
RUN npm install

# Fix up some of the issues
RUN npm audit fix

# Expose API port to the outside
EXPOSE 1337

# Launch application
CMD ["node","app.js"]
```

Building Dockerfile:

Listing all images available:

Running `message-app` Dockerfile:

Created YAML file:
```
# Define Version 3 YAML
version: '3'
services:
  # Specify mongo as one of the base images
  mongo:
    image: mongo:4.0.7
    # Reference persistent storage
    volumes:
      - mongo-data:/data/db
    # Expose port for usage
    expose:
      - "27017"
  # Specify app image as another service
  app:
    build: .
    # Expose port for usage
    ports:
      - "1337:1337"
    # Link to mongo
    links:
      - mongo
    depends_on:
      - mongo
    # Add environment variable for communication with database
    environment:
      - MONGO_URL=mongodb://mongo/messageApp
# Define persistent storage volume
volumes:
  mongo-data:
```

Building `docker-compose`:

Running the services:

Using app commands:
