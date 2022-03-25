## Example 00
Running `docker run docker/whalesay cowsay boo`:
![00-01](https://user-images.githubusercontent.com/18493608/160162372-277c0c7a-1dcb-49d2-82a5-969996c15ec5.png)

## Example 01
Running Ubuntu container:
![01-01](https://user-images.githubusercontent.com/18493608/160162383-d6c008a4-553f-4e23-b35a-b723edbca2db.png)

Installing & Running Vim:
![01-02](https://user-images.githubusercontent.com/18493608/160162394-353138e6-fae4-4291-b251-3023916db60e.png)

Installing & Running Cowsay:
![01-03](https://user-images.githubusercontent.com/18493608/160162645-2bd7182d-f795-4136-91ef-3e0a675fa7b6.png)

## Example 02
Running Mongo container:
![02-01](https://user-images.githubusercontent.com/18493608/160162417-285ca0c1-d46d-4902-befe-5128a8b1ca8d.png)

Running Rocket.Chat container:
![02-02](https://user-images.githubusercontent.com/18493608/160162432-60c5d7e6-08e7-4f06-b100-e7405ed58994.png)

Rocket.Chat localhost application:
![02-03](https://user-images.githubusercontent.com/18493608/160162442-df348e3c-f10f-4bea-8404-c41ef114d80f.png)

Testing other docker commands:
![02-04](https://user-images.githubusercontent.com/18493608/160162448-bae8ba1a-1cd8-4d76-b817-cef72191b310.png)

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
![02-04](https://user-images.githubusercontent.com/18493608/160162464-1e7250b4-9f39-423f-a9d9-c013a246fa44.png)

Running container:
![03-02](https://user-images.githubusercontent.com/18493608/160162472-f939ff93-27c3-421b-8c6c-aa51f4d2c94f.png)

Application on localhost:
![03-03](https://user-images.githubusercontent.com/18493608/160162482-f205ae87-4e02-4247-ba2e-bac2c1445ba0.png)

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
![04-01](https://user-images.githubusercontent.com/18493608/160162501-35399953-54be-433d-b218-b46b90131b37.png)

Listing all images available:
![04-02](https://user-images.githubusercontent.com/18493608/160162506-360b0183-ed3c-43e9-aa5f-50abdbfbc8f7.png)

Running `message-app` Dockerfile:
![04-03](https://user-images.githubusercontent.com/18493608/160162510-8bdddea0-8880-42ee-8195-eaf36fd6a0da.png)

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
![04-04](https://user-images.githubusercontent.com/18493608/160162527-1e178942-a0b2-4f85-9e61-5b1bcf148e4d.png)

Running the services:
![04-05](https://user-images.githubusercontent.com/18493608/160162539-44ef4367-a213-4e5e-902d-add24df4100c.png)

Using app commands:
![04-06](https://user-images.githubusercontent.com/18493608/160162564-30bdf8c9-ae4f-430a-b448-bdb291c0ba0f.png)
![04-07](https://user-images.githubusercontent.com/18493608/160162581-47194450-c45f-476a-ada9-9860fb6e91a6.png)
