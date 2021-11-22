# Here, Pulling images

docker pull infracloudio/csvserver:latest
docker pull prom/prometheus:v2.22.0

# Creating container with the following image and running it as a daemon.
docker run -d --name csv infracloudio/csvserver:latest

# Checking the list of the containers
docker ps -a

# Checking the logs of the container for errors.
docker logs csv

# This error is showing in the container logs
2021/11/22 09:38:34 error while reading the file "/csvserver/inputdata": open /csvserver/inputdata: no such file or directory

# Creating bash script for creating inputdata file and named it as gencsv.sh

#!/bin/bash
name=inputdata
echo -en '0, 234\n1, 98\n2, 34\n' >> $name

# Again, creating the container and mounting the volume to it.
docker run -d -it -v /csvserver:/csvserver --name csv infracloudio/csvserver:latest /bin/bash

# Checking the status of the container

docker ps -a        # Running successfully

# Taking bash shell in the container
docker exec -it csv /bin/bash

# Checking the ports in the container
netstat -tulnp 

# Stopping the container
docker stop csv

# Deleting the container csv.
docker rm csv

# Creating the container and exposing the port and passing the environment variable and mounting a directory into it.
docker run -d -it -p 9300:9393 --env CSVSERVER_BORDER=Orange -v /csvserver:/csvserver --name csv infracloudio/csvserver:latest /bin/bash

