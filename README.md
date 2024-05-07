# Car price predictor

This python project uses spider crawler to get data of cars on the second-hand market from tutti.ch. With this data a machine learning model is trained and then combined with a webpage which allows users to predict an estimate of the price, for which their car is currently being sold on the secondary market.

## Spider

The spider is responsible for scraping new and additional data on a regular basis. The output is saved in a `file.jl` (json list) format. The scraped data is then loaded into MongoDB. The spider also updates the model by producing a correlation heatmap, checking the R2 value (closer to 1 is better), and checking the MSE value (lower is better, squared seconds). The updated model is saved to `model/GradientBoostingRegressor.pkl`.

## Azure Blob Storage

The model is also saved to Azure Blob Storage. It is important to always save a new version of the model. Access to Azure Blob Storage is granted through the storage account's access key, which can be set as an environment variable for Docker or as a secret for GitHub.

## GitHub Action

The GitHub Action workflow includes the following steps:
- Scrape data
- Load data into MongoDB (Azure Cosmos DB)
- Update the model and save it to Azure Blob Storage

Unfortunately, GitHub Action is not working flawlessly due to blocked crawling of tutti.ch by GitHub Actions. Running the application locally works perfectly fine.

## App

The application consists of a Python Flask backend (`backend/service.py`) and a SvelteKit frontend. The frontend is currently built manually.

## Deployment with Docker

To deploy the application, a Dockerfile is provided. The dependencies are installed using pip. The frontend (prebuilt) is copied into the Docker image. The access key for Azure Blob Storage is set as an environment variable.

## Ideas

One idea for further development is to use a different webpage for the crawling, so that it can be automated via GitHub Actions. Additionally, more data can be used to improve the model.
