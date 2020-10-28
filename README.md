# Deploying R Shiny Apps with Docker and renv

This repository is intended as an end-to-end example of deploying an
[R Shiny](https://shiny.rstudio.com/) application using [Docker](https://www.docker.com/). It also employs
the [renv](https://rstudio.github.io/renv/articles/renv.html) R package to simplify package installation in the Docker image.

It is based on [this blog post](https://www.statworx.com/de/blog/how-to-dockerize-shinyapps/) from [STATWORX](https://www.statworx.com/de/).

## Getting Started

I you don't have it already, install Docker.

Then, clone this repository.

```
git clone https://github.com/DavidASmith/r-shiny-docker-renv.git
```

Explore the files.

```
cd r-shiny-docker-renv
ls
```
Note that there is a `Dockerfile` which contains all the instructions to build an image of the R Shiny server and our app.

The `example-app` folder contains the R Shiny app we want to deploy.

## Running the App Locally

To run the app locally, we must follow these steps:

1. Build the Docker image.
2. Start the container.
3. Open the app in the browser.

### Build the Docker Image

To build the image, navigate to the folder containing the Dockerfile and run the following command.

```
docker build -t my-shinyapp-image .
```

### Start the Container

Wait for the build process to finish, then start the container by running the following.

```
docker run -d --rm -p 3838:3838 my-shinyapp-image
```

### Open the App in the Browser

Open your web browser and go to http://localhost:3838/. The example app 'Old Faithful Geyser Data' is displayed.
