# API Server Setup

* [ ] \[1d\] write steps
* [ ] \[2h\] link to other sections for pre-requisites and add pages for them in the relevant place in the ToC.

The API Server acts as an oracle onto the Kaspa network by by connecting to a full Kaspa node while running requests locally rather then connecting to a network node over the Internet.



## QuickStart

## Build & Run a Kaspa API Server:

Clone the Kaspa open source code from DAGlabs' Github onto the machine you want to use.

[https://github.com/daglabs/api-server](https://github.com/daglabs/api-server) \[todo: update url\]

### Prerequisites

* Setup Go runtime
* Install MySql
* Build & run Node \(btcd\)

### API Server Installation Steps

Install the following prerequisites:

1. Build & run API Server
2. Local Database Setup
3. Connecting to the Kaspa network
4. Test API Server connections & settings
5. Starting the API Server
6. Making sure it works

### Download & Launch Docker Image of MySQL Database

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

### Migrating API Server MySQL Database

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

### Start a Kaspa Full Node & Connect to DevNet

```bash
:~/code/btcd$ ./btcd --devnet --rpclisten=localhost:18334 --rpcuser=user --rpcpass=pass 
--notls --acceptanceindex --txindex
```

```text
:~/code/btcd/apiserver$ ./apiserver --rpcserver=localhost:18334 
--rpcuser=user --rpcpass=pass --notls --dbuser=root --dbpass=pass 
--dbaddress=localhost:42069 --dbname=kaspa --devnet
```





