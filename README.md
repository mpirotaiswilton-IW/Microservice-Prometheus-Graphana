# prometheus-grafana-docker-MaxPW
 
This Github repository a Prometheus monitoring system and a Grafana metrics dashboard, meant to work in tandem with a Dockerized Springboot API. 

## Summary

[prometheus-grafana-docker-MaxPW](#prometheus-grafana-docker-MaxPW)
* [Summary](#summary)
* [Setup and Pre-requisites](#setup-and-pre-requisites)
* [Running Naughts-and-crosses](#running-naughts-and-crosses)
    * [Playing a game using Postman](#playing-a-game-using-postman)
        * [Optional Endpoints](#optional-endpoints)
    * [Role-Based Authorization](#role-based-authentication)
        * [Admin Endpoints](#admin-endpoints)
    * [Accessing the pgAdmin Dashboard](#accessing-the-pgadmin-dashboard)
    * [Setting up a Grafana dashboard](#setting-up-a-grafana-dashboard)


## Setup and Pre-requisites

1. If not already installed:

-  Install Docker on your device (you can use the following link for a guide: [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/))

2. Run an instance of the [Dockerized-Springboot-naughts-and-crosses](https://github.com/mpirotaiswilton-IW/Dockerized-Springboot-naughts-and-crosses-MaxPW) 

3. Clone this repository or download the .zip file from GitHub (extract the downloaded zip file )

## Running The Prometheus and Grafana containers

1. Using a Command Line Interface used to run Docker and docker-compose commands, change directory to the downloaded/cloned repository

2. Run the following command: 

```
docker-compose up
```

3. 2 docker containers should now be running:
    * `naughts-and-crosses-prometheus`
    * `naughts-and-crosses-grafana`

### Setting up a Grafana dashboard

After successfully deploying the docker containers to run the Naughts-and-crosses API, you can access a Grafana metrics dashboard at [http://localhost:3000/](http://localhost:3000/)

1. Open the link above, you will be prompted to enter user credentials. Enter the following :
    * `admin` in the `Email or username` field
    * `admin` in the `Password` field

2. After logging in with those credentials, Grafana will prompt you to enter a new password. While you can skip this step, Changing to a new password at this stage is highly recommended to improve system security. Enter a new password of your choosing in both the `New password` and `Confirm new password` fields and then hit the Submit button.

From here, you will have accessed the Grafana home page. From here, our next step will be to create a set up a data source

3. In the `Basic` row on the Home page, select the `Data Source` option

4. From the data source type list, select `Prometheus`

5. Fill in the following fields as detailed below and leave all the others to their default:
    * `Name` : `Name of your choosing` (the default name will be `prometheus`)
    * `Prometheus server URL` : `http://prometheus:9090`

6. Hit the `Save & test` button. A message should display saying `Successfully queried the Prometheus API` if the data source was successfully added. You can also verify your data source has been added by going to [http://localhost:3000/connections/datasources](http://localhost:3000/connections/datasources), where you will see a single data source with the name and prometheus URL you provided in the previous step.

Now we can add a dashboard to monitor our API:

7. Navigate back to the Grafana home page and, from the `Basic` row, select `Create your first dashboard`

8. On the bottom right hand side of the screen, underneath the `+ Add visualization` button, hit the `Import dashboard` button. From there, upload the `Basic Dashboard.json` file present in this repository.

9. Hit `Load`. You should now see a modest dashboard, featuring (from left to right and top to bottom):

    * The application Start Time
    * Uptime
    * System CPU usage
    * 2xx SUCCESSFUL response rates
    * 4xx CLIENT ERROR response rates (both measured in responses per second)

## Monitoring a Springboot Application

### Example: Naughts-and-Crosses API

To demonstrate both containers working successfully, we can launch the `naughts-and-crosses` Springboot API, located at the [Dockerized-Springboot-naughts-and-crosses-MaxPW](https://github.com/mpirotaiswilton-IW/Dockerized-Springboot-naughts-and-crosses-MaxPW "naughts and crosses Springboot API repository link") GitHub repository, and use its endpoints. This will give prometheus data that will then be reflected in the Grafana dashboard.

1. Ensure the API and the monitoring services are running.
2. Perform 5 requests to `http://localhost:8080/user/` in rapid succession, then check the Grafana dashboard. The `2xx response rate` graph visualisation panel should show a spike in responses per second.
3. Perform 5 requests to `http://localhost:8080/user/1234` in rapid succession, then check the Grafana dashboard. The `4xx response rate` graph visualisation panel should show a spike in responses per second.