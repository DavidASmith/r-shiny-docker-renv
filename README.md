# Deploying R Shiny Apps with Docker and renv

This repository is intended as an end-to-end example of deploying an
[R Shiny](https://shiny.rstudio.com/) application using [Docker](https://www.docker.com/). It also employs
the [renv](https://rstudio.github.io/renv/articles/renv.html) R package to simplify package installation in the Docker image.

It is based on [this blog post](https://www.statworx.com/de/blog/how-to-dockerize-shinyapps/) from [STATWORX](https://www.statworx.com/de/).

## Getting Started

I you don't have it already, install Docker.

Then, move to folder you want to work in and clone this repository.

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

## Deploying to Microsoft Azure as a Web App

Assuming you have already...

- Got an Azure project you can work in.
- Have created a resource group for the app.

We can deploy the dockerised app to Azure as a Webapp by performing the following steps:

1. Create a container registry to store the image of our app.
2. Build the image in the container registry.
3. Deploy the image as a web app.

### Create the Container Registry

Use the following command to create a container registry to store the image of the app.

```
az acr create --name myregistry --resource-group mygroup --sku standard --admin-enabled true
```
Where:

- `myregistry` is what you want to name your container registry.
- `mygroup` is the name of the resource group in which the registry will be created.

### Build the Image

Next we need to build the image in the container registry. Run this from the folder which contains the Dockerfile.

```
az acr build --file Dockerfile --registry myregistry --image myimage .
```
Where:

- `myregistry` is the name of your container registry.
- `myimage` is what you want to name the image in the registry.

### Deploy the Web App

To deploy the image as a Web App:

1. In the [Azure portal](https://portal.azure.com/), navigate to the resource group you have been using in the steps above.
2. Click **+ Add** and select **Web App**. The Create Web App page is displayed.
3. Enter a **Name** for your app. Note that this will form part of the URL on which the app can be accessed.
4. Select **Docker Container** as the **Publish** option.
5. Select **Linux** as the **Operating System**.
6. Select the **Region** where your app will be hosted.
7. Select an **App Service Plan** (or leave as default for this example).
8. Click **Next : Docker >**.
9. For the **Image Source**, select **Azure Container Registry**.
10. Select the **Registry** you created in the steps above.
11. Select the **Image** you created.
12. Click **Review + create**.

After some time for deployment, you should be able to navigate to the newly created App Service in the portal. On the Overview page there is a URL. Go to this URL to view your app.
