# SonarQube Server

How to install SonarQube using a Docker Compose file. SonarQube consists in two parts, the first one is the SonarQube server and the last one is the SonarScanner that is going to read our project code and communicate with SonarQube server to get corrections, etc.

# Requirements
- Linux
- Docker
- Docker Compose
- Free port on host machine

# Installation
**NOTE:** Choose a free port of your host machine in order to avoid conflicts with other services or apps. <br>
In the same directory where is located the docker-compose.yml file use this command:

```
docker-compose up -d 
```

### Possible Error 
It's possible to get an error in the SonarQube container logs related with the memory. The error looks similar to

```
[1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```

### Solution
The solution to this problem is increase the memory until 262144 in the host machine as the error says. With this command we can fix it. <br>
**NOTE:** If you want to read more about it, [go to this GitHub issue thread](https://github.com/SonarSource/docker-sonarqube/issues/282)
```
sysctl -w vm.max_map_count=262144
```
Now, you can run the docker compose again and everything must be fine.

# Sonar Scanner
The Sonar Scanner is the part that analyze our project and send the results to the SonarQube server.

## Using Sonar Scanner Docker image 
In order to avoid the installation of the Sonar Scanner in our machine, we can use the sonar scanner docker image and pass some arguments in the CLI like the `SONAR_HOST_URL` and the `SONAR_LOGIN`
```
docker run \
    --rm \
    -e SONAR_HOST_URL="http://${SONARQUBE_URL}" \
    -e SONAR_LOGIN="myAuthenticationToken" \
    -v "${YOUR_REPO}:/usr/src" \
    sonarsource/sonar-scanner-cli
```

### Usage

**NOTE:** Before run the command `sonar-scanner` you must have created the project on the SonarQube server

We must have a file named `sonar-project.properties` in the folder of the project that we want to scan. In this repo there is a `sonar-project.properties` file that you can use for other projects replacing some values like `project_key`, `project_name`, `host.url`, `login` y `password`. If you don't want to analyze a folder o certain files you can add it to the esclusions property inside the file.
<br> <br>
If the SonarQube server is up, then we can type the command `sonar-scanner` inside our project folder in our shell and the scanner begin to analyze our project.
<br><br>
If you see something like the message below, then hte process finished successfully.
```
INFO: ANALYSIS SUCCESSFUL, you can browse http://10.150.1.79:9005/dashboard?id=wiser_api_back
```

# Source
- [How to Setup Sonar Cube + Sonar Scanner with docker compose (Simple)](https://derryberni.medium.com/how-to-setup-sonar-cube-sonar-scanner-with-docker-compose-simple-15c9d84966dc)

- [Sonarqube crashed still with latest image](https://github.com/SonarSource/docker-sonarqube/issues/282)
- [Elasticsearch: Max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]](https://stackoverflow.com/questions/51445846/elasticsearch-max-virtual-memory-areas-vm-max-map-count-65530-is-too-low-inc)
