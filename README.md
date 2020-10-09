
[![CircleCI](https://circleci.com/gh/circleci/Operationalize-a-MachineLearning-MicroserviceAPI.svg?style=svg)](https://app.circleci.com/pipelines/github/SamikshyaAd/Operationalize-a-MachineLearning-MicroserviceAPI)

# Operationalize-a-Machine-Learning-Microservice-API

## Project Overview
The goal of this project is to oprationalize a machine learning microservice using kubernetes cluster. This project deploys containerized python flask application on kubernetes cluster.  
The microservice serves out predictions (inference) about housing prices through API calls. It uses pre-trained, `sklearn` model that has been trained to predict housing prices in Boston according to several features, such as average rooms in a home and data about highway access, teacher-to-pupil ratios and many more.

## Files included
* `config.yml`: CircleCI configuration file for running the tests
* `app.py`: Python flask app that serves out predictions (inference) about housing prices through API calls
* `Dockerfile`: Dockerfile for building the image
* `make_prediction.sh`: Send a request to the Python flask app to get a prediction for localhost
* `Makefile`: includes instructions on environment setup and lint tests
* `run_docker.sh`: file to be able to get Docker running, locally
* `run_kubernetes.sh`: file to run the app in kubernetes
* `upload_docker.sh`: file to upload the image to docker


## Setup the Environment

* Create a virtualenv and activate it
    `python3 -m venv <your_venv>
     source <your_venv>/bin/activate`

* Install dependencies via project `Makefile`. Dependencies are listed in the file `requirements.txt`.
Run `make install` to install the necessary dependencies

While you still have your virtualenv environment activated, you will need to `install`:

### Docker
You will need to use Docker to build and upload a containerized application. If you already have this installed and created a docker account, you may skip this step.

* You’ll need to create a free docker account: https://hub.docker.com/, where you’ll choose a unique username and link your email to a docker account. Your username is your unique docker ID.

* To install the latest version of docker, choose the Community Edition (CE) for your operating system, on docker’s installation site: https://docs.docker.com/get-docker/. It is also recommended that you install the latest, stable release.

* Verify successful installation by printing its version in your terminal: `docker --version`

### Hadolint

#### Run Lint Checks
This project also must pass two lint checks; `hadolint` checks the Dockerfile for errors and `pylint` checks the app.py source code for errors.
* Install `hadolint` following the instructions, on hadolint's page: https://github.com/hadolint/hadolint

For Mac:

 `brew install hadolint`

For Windows:

 `scoop install hadolint`

 * In your terminal, type: `make lint` to run lint checks on the project code. If you haven’t changed any code, all requirements should be satisfied, and you should see a printed statement that rates your code (and prints out any additional comments):

`------------------------------------`
`Your code has been rated at 10.00/10`

### Kubernetes (Minikube)
### Install Minikube
To run a Kubernetes cluster locally, for testing and project purposes, you need the Kubernetes package, Minikube. This operates in a virtual machine and so you'll need to download two things: A virtual machine (aka a hypervisor) then minikube. Thorough installation instructions can be found here:https://kubernetes.io/docs/tasks/tools/install-minikube/. Here is how I installed minikube:

* Install VirtualBox:

For Mac:

`brew cask install virtualbox`

For Windows, I recommend using a Windows host: https://www.virtualbox.org/wiki/Downloads

* Install minikube:

For Mac:

`brew cask install minikube`

For Windows, I recommend using the Windows installer: https://kubernetes.io/docs/tasks/tools/install-minikube/

### Troubleshooting
In general, you can verify installation by checking the version of a library, ex. `kubectl version` or `docker --version`. If there is no package found, you may need to install that library.

Mac issue: If you get an error `error: invalid active developer path`, that means you need to install some Xcode developer tools. You can do this on Mac by running this terminal command: `xcode-select --install`

## Run the Project

### Run a Container & Make a Prediction
In order to run a containerized application, you’ll need to build and run the docker image that you defined in the `Dockerfile`, and then you should be able to test your application, locally, by having the containerized application accept some input data and produce a prediction about housing prices. 
To run and build a docker image, go to project folder from your terminal and type `./run_docker.sh` 

You will see something like: 
`Successfully built <build id>`
`Successfully tagged <your tag>`

Then, to make a prediction, you have to open a separate tab or terminal window. In this new window, navigate to the main project directory and run `./make_prediction.sh`

This shell script is responsible for sending some input data to your containerized application via the appropriate port. Each numerical value in here represents some feature that is important for determining the price of a house in Boston. The source code is responsible for passing that data through a trained, machine learning model, and giving back a predicted value for the house price.

In the prediction window, you should see the value of the prediction, and in your main window, where it indicates that your application is running, you should see some log statements print out. You’ll see that it prints out the input payload at multiple steps; when it is JSON and when it’s been converted to a DataFrame and about to be scaled.

### Upload the Docker Image
Now that you’ve tested your containerized image locally, you’ll want to upload your built image to docker. This will make it accessible to a Kubernets cluster.

Assuming you’ve already built the docker image with `./run_docker.sh`, you can now upload the image by calling `./upload_docker.sh`
You should see a successful login statement and a repository name that you specified, printed in your terminal. You should also be able to see your image as a repository in your docker hub account: https://hub.docker.com/

### Configure Kubernetes to Run Locally
To start a local cluster, type the terminal command: `minikube start`

After minikube starts, a cluster should be running locally. You can check that you have one cluster running by typing `kubectl config view` where you should see at least one cluster with a `certificate-authority` and `server`.

### Deploy with Kubernetes
Run `./run_kubernetes.sh` in your terminal.
This script should create a pod with a name you specify.

* Make a prediction
After you’ve called `./run_kubernetes.sh`, and a pod is up and running, make a prediction using a separate terminal tab, and a call to `./make_prediction.sh`

### Delete Cluster
Call `minikube delete`


### Running `app.py`

1. Standalone:  `python app.py`
2. Run in Docker:  `./run_docker.sh`
3. Run in Kubernetes:  `./run_kubernetes.sh`

### Kubernetes Steps

* Setup and Configure Docker locally
* Setup and Configure Kubernetes locally
* Create Flask app in Container
* Run via kubectl
