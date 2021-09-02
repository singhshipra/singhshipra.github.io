# singhshipra.github.io
My repository
# Assumption:
The ownCloud server installation is being done locally.
# To enable users to connect using local IP and port:
1. Create a new project directory. 
```
mkdir owncloud-docker-server
cd owncloud-docker-server
```
2. Copy the sample docker-compose.yml file and paste it into that new directory.
```
# Copy docker-compose.yml from the GitHub repository
wget https://raw.githubusercontent.com/owncloud/docs/master/modules/admin_manual/examples/installation/docker/docker-compose.yml
```
3. Create a .env configuration file containing all configuration settings.
```
cat << EOF > .env
OWNCLOUD_VERSION=10.8
OWNCLOUD_DOMAIN=localhost:8080
ADMIN_USERNAME=admin
ADMIN_PASSWORD=admin
HTTP_PORT=8080
EOF
```
4. Build and start the container using the docker command-line tool. 
```
docker-compose up -d
```
5. Execute the command `docker-compose ps` to verify that all the containers have successfully started.

ownCloud is accessible using the port 8080 on the host machine.
