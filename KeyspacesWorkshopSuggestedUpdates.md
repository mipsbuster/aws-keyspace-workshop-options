# Keyspaces Workshop

Following are suggestions for the keyspaces overview workshop

https://amazon-keyspaces-immersionday.workshop.aws/en/setup/first-content/_first-content0.html

## Create an EC2  Instance for Keyspaces Workshop

For flow and new or newbie AWS users creating an instance could be a challege. will add steps for creating EC2 instance plus connecting

### Command Line EC2 Instance

add content here for creating EC2 instance

### AWS Console Line EC2 Instance

add content here for creating EC2 instance

### Potential changes when running EC2 AWS linux instance

### `Install Docker CentOS

need sudo to run commands. 

```
$ sudo -y yum install docker
$ sudo systemctl start docker
```



When testing the docker install you may encounter the following error related to the ec2-user permissions for docker.

```
$docker run hello-world
docker: Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post http://%2Fvar%2Frun%2Fdocker.sock/v1.40/contain
ers/create: dial unix /var/run/docker.sock: connect: permission denied.
```

If you encounter the access error run the following to allow the ec2-user access. 

```
$sudo chmod 666 /var/run/docker.sock
```

Then run the test again

```
$docker run hello-world
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### Install GIT

```
$sudo yum -y install git
```



# Generate Credentials

### Console

### Create Keyspaces Group

add content

### Create Keyspaces User

1. Create IAM user for accessing Keyspaces
2. Select Users in IAM
3. Click on AddUser  and enter keyspaceUser1 as the user name
4. Select programtic access
5. Click "Next Permissions"
6. Select "Add User to Group" and select key spaceUsers
7. Hit "Next Tags"
8. Add a tag "Name" with value "KeyspaceUser"
9. Hit "Review"
10. Hit "Create user"
11. Download credentials in csv
12. hit "Close"
13. Select user you just created
14. Select the 'Security Credentials" tab and scroll down to "Credentials for Amazon Keyspaces (for Apache Cassandra)"'
15. Select "Generate Credentials" and copy the user name and password also down the csv file. You need this info for clash. 

### Command Line

Add steps here

## Loading Data

Need to load java if not available

```
$sudo yum -y install java
$java --version
openjdk 11.0.10 2021-01-19 LTS
OpenJDK Runtime Environment Corretto-11.0.10.9.1 (build 11.0.10+9-LTS)
OpenJDK 64-Bit Server VM Corretto-11.0.10.9.1 (build 11.0.10+9-LTS, mixed mode)
```

issue with dsload

```
[ec2-user@ip-172-31-24-197 ~]$ dsbulk-1.6.0/bin/dsbulk load -f default-ks-bulk-loader.conf -url ordered.csv -k aws -t ordered --driver.basic.contact-p
oints '["cassandra.us-east-1.amazonaws.com:9142"]' --driver.advanced.ssl-engine-factory.truststore-path cassandra_truststore.jks --driver.advanced.ssl
-engine-factory.truststore-password amazon -u "keyspaceUser1-at-610880146692" -p "BACQJZfJzWZ/HZGKbtoSYqTD6PgOBgkEH2HpaBByO2Q=" --executor.maxPerSecon
d 2000
Username and password provided but auth provider not specified, inferring PlainTextAuthProvider
Operation directory: /home/ec2-user/logs/LOAD_20210223-191354-725245
[driver] Control node cassandra.us-east-1.amazonaws.com/3.234.248.205:9142 has an entry for itself in system.peers: this entry will be ignored. This i
s likely due to a misconfiguration; please verify your rpc_address configuration in cassandra.yaml on all nodes in your cluster.
[driver] Unsupported partitioner 'com.amazonaws.cassandra.DefaultPartitioner', token map will be empty.
[driver] Unknown peer 6c945ed5-b04e-3fc4-8dc0-9aea1128bfa8, excluding from schema agreement check
[driver|cassandra.us-east-1.amazonaws.com/3.234.248.205:9142]  Error while opening new channel (DriverTimeoutException: [driver|id: 0xfbadc8ed, L:/172
.31.24.197:34828 - R:cassandra.us-east-1.amazonaws.com/3.234.248.205:9142] Protocol initialization request, step 1 (STARTUP {CQL_VERSION=3.0.0, DRIVER
_NAME=DataStax Java driver for Apache Cassandra(R), DRIVER_VERSION=4.7.2, CLIENT_ID=5258b12a-1222-32cd-afaa-b0e48b127cbb, APPLICATION_NAME=DataStax Bu
lk Loader LOAD_20210223-191354-725245, APPLICATION_VERSION=1.6.0}): timed out after 300000 ms)
 total | failed | rows/s | p50ms |  p99ms | p999ms
41,762 |      0 |  1,358 | 45.29 | 167.77 | 221.25
```

Error with table "hotkey"

```
 dsbulk-1.6.0/bin/dsbulk load -f default-ks-bulk-loader.conf -url random.csv -k aws -t hotkey --driver.basic.contact-poi
nts '["cassandra.us-east-1.amazonaws.com:9142"]' --driver.advanced.ssl-engine-factory.truststore-path cassandra_truststore.jks --driver.advanced.ssl-e
ngine-factory.truststore-password amazon  -u "keyspaceUser1-at-610880146692" -p "BACQJZfJzWZ/HZGKbtoSYqTD6PgOBgkEH2HpaBByO2Q=" --executor.maxPerSecond
 2000 -m "0=id,1=event,2=data"
Username and password provided but auth provider not specified, inferring PlainTextAuthProvider
Operation directory: /home/ec2-user/logs/LOAD_20210223-192701-860535
[driver] Unsupported partitioner 'com.amazonaws.cassandra.DefaultPartitioner', token map will be empty.
Operation LOAD_20210223-192701-860535 failed: Table hotkey does not exist.
```

## Point in time recovery

```
COPY aws.pitr_table_a (id,name,project,region,division,role,pay_scale,vacation_hrs,manager_id) FROM '/source/pitr
.csv' AND header=TRUE ;
Improper COPY command.
```

