---
description: 'Install your own Kaspa API Server, from source'
---

# Set Up from Source

The API Server connects to the Kaspa BlockDAG network by connecting to a Kaspa full node running on the same machine. You will need to [setup a full node](../running-a-node/build-a-node-server-from-source-code.md) before proceeding.  

### Prerequisites

* Setup Go runtime, and Git onto the machine you want to use. 
* \*\*\*\*[**Docker installed**](https://hub.docker.com/) ****& Docker Hub user account.  
* Clone the Kaspa open source code from [https://github.com/daglabs/api-server](https://github.com/daglabs/api-server).

### API Server Installation Steps  <a id="api-server-installation-steps"></a>

Once you have completed the above per-requisites, the remaining steps for setting up the API Server are:

1. Download & Launch Docker Image of MySQL Database
2. Migrating API Server MySQL Database
3. Connecting to your node on DevNet
4. Starting the API Server
5. Making sure it works

### Download & Launch Docker Image of MySQL Database <a id="download-and-launch-docker-image-of-mysql-database"></a>

The following command downloads and installs a ready made Docker image for a pre-configured MySQL database for a Kaspa API Server.  You can download and learn more about Docker [here](https://hub.docker.com/).

```bash
:~/code$ docker run --name kaka -p 42069:3306 -e MYSQL_ROOT_PASSWORD=pass mysql
```

After launching the MySQL Docker image, run the following code in a separate terminal to start the database within the above Docker environment.  

```bash
:~/code$ docker exec -it kaka bash
```

You should now see the _mysql&gt;_  prompt.  Type the following command to create a database, using this database name later when launching your Kaspa API Server:

```sql
mysql> create database kaspa;
Query OK, 1 row affected (0.02 sec)
```

Next, you will need to run the following database migrations on to finish configuring your new database to work with the Kaspa API Server.

### Migrating API Server MySQL Database <a id="migrating-api-server-mysql-database"></a>

Run the following command from the /apiserver directory to run all necessary database migrations \(you can check out the SQL scripts for these migrations under the _btcd/apiserver/miogrations_ directory in the Kaspa repository\).

```bash
:~/code/btcd/apiserver$ ./apiserver --migrate --dbuser=root --dbpass=pass 
--dbaddress=localhost:42069 --dbname=kaspa --notls
```

If successful, you should see the following response, meaning that you are finally ready to start your API Server.

```bash
2019-10-24 10:39:28.127 [INF] DTBS: Migrated database to the latest version (version 9)
```

You are now ready to start the API Server & connect to your newly created database.

## Start the API Serve & Connect to the network:

## API Server Reference Guide









