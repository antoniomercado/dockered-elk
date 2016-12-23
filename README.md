# Dockered ELK

This project provides the foundation to deploy an Elasticsearch, Logstash, and Kibana (otherwise known as ELK) stack
with Filebeat plugin support via docker compose. 

ELK is a widely accepted solution for system and application logging and infrastructure visibility. It's one thing to
have system logs on every node in your network, its another to be able to see real-time data about their state. The Filebeat plugin support 
bundled with this stack defaults with TLS encryption and self signed certificates for "localhost"
support. The Kibana endpoint can be accessed by visiting `https://localhost` if you are deploying this locally. Logs can
be shipped using the Filebeat plugin to "localhost:8444".

# Versions

- Elasticsearch 2.3.1
- Logstash 2.3.1
- Kibana 4.5.0

# Dependencies

- Docker Engine >= 1.10.0
- Docker Compose >= 1.6.0


# Commands

### Build the stack
`docker-compose build`

### Run the stack
`docker-compose up -d`

### Kill the stack
`docker-compose down`

# Helpful Tips

### Tip 1
The Elasticsearch cluster that is deployed uses Shield for authentication. The default admin username is `admin` and
password `changeme`. Kibana uses these credentials to login to read the data that it visualizes for you and Logstash
uses it to write the log data to the Elasticsearch database. You will want to change the default credentials if using this stack definition in a production system. 
There are several configuration files in this project which reference these default settings, so make sure to search all files when making these changes.



### Tip 2
Each service in the stack has its own Dockerfile which defines the versions via Dockerfile `ENV` variables. This is
where you would go to change the version of different services as the Elastic community releases new versions of the software.

### Tip 3
To change the Logstash filters you need to edit `logstash/logstash.conf`. The stack defaults to filtering nginx access
and error logs.

# Contributions

All contributions are gladly welcomed. Leave a message or open a pull request.
