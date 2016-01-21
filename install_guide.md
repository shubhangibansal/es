STEPS FOR INSTALLING ES :

#Follow the link : https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-14-04

1)sudo apt-get install openjdk-7-jre
2)To verify your JRE is installed and can be used, run the command:

java -version

3)Downloading and installing : 

--wget https://download.elastic.co/elasticsearch/elasticsearch/elasticsearch-1.5.1.deb
--sudo dpkg -i elasticsearch-1.5.1.deb
--To make sure Elasticsearch starts and stops automatically with the Droplet, add its init script to the default runlevels with the command:

	sudo update-rc.d elasticsearch defaults

4)Configuring elastic

--To start editing the main elasticsearch.yml configuration file:

	sudo nano /etc/elasticsearch/elasticsearch.yml

--Remove the # character at the beginning of the lines for node.name and cluster.name to uncomment them, and then change their values. Your first configuration changes in the /etc/elasticsearch/elasticsearch.yml file should look like this:

	/etc/elasticsearch/elasticsearch.yml
	...
	#node.name: "My First Node"
	#cluster.name: mycluster1
	...

--Another important setting is the role of the server, which could be either "master" or "slave". "Masters" are responsible for the cluster health and stability. In large deployments with a lot of cluster nodes, it's recommended to have more than one dedicated "master." Typically, a dedicated "master" will not store data or create indexes. Thus, there should be no chance of being overloaded, by which the cluster health could be endangered.

"Slaves" are used as "workhorses" which can be loaded with data tasks. Even if a "slave" node is overloaded, the cluster health shouldn't be affected seriously, provided there are other nodes to take additional load.

The setting which determines the role of the server is called node.master. If you have only one Elasticsearch node, you should leave this option commented out so that it keeps its default value of true — i.e. the sole node should be also a master. Alternatively, if you wish to configure the node as a slave, remove the # character at the beginning of the node.master line, and change the value to false:

	/etc/elasticsearch/elasticsearch.yml
	...
	node.master: false
	...
Another important configuration option is node.data, which determines whether a node will store data or not. In most cases this option should be left to its default value (true), but there are two cases in which you might wish not to store data on a node. One is when the node is a dedicated "master," as we have already mentioned. The other is when a node is used only for fetching data from nodes and aggregating results. In the latter case the node will act up as a "search load balancer".

Again, if you have only one Elasticsearch node, you should leave this setting commented out so that it keeps the default true value. Otherwise, to disable storing data locally, uncomment the following line and change the value to false:

	/etc/elasticsearch/elasticsearch.yml
	...
	node.data: false
	...
Two other important options are index.number_of_shards and index.number_of_replicas. The first determines into how many pieces (shards) the index will be split into. The second defines the number of replicas which will be distributed across the cluster. Having more shards improves the indexing performance, while having more replicas makes searching faster.

Assuming that you are still exploring and testing Elasticsearch on a single node, it's better to start with only one shard and no replicas. Thus, their values should be set to the following (make sure to remove the # at the beginning of the lines):

	/etc/elasticsearch/elasticsearch.yml
	...
	index.number_of_shards: 1
	index.number_of_replicas: 0
	...
One final setting which you might be interested in changing is path.data, which determines the path where data is stored. The default path is /var/lib/elasticsearch. In a production environment it's recommended that you use a dedicated partition and mount point for storing Elasticsearch data. In the best case, this dedicated partition will be a separate storage media which will provide better performance and data isolation. You can specify a different path.data path by uncommenting the path.data line and changing its value:

	/etc/elasticsearch/elasticsearch.yml
	...
	path.data: /media/different_media
	...
	Once you make all the changes, please save and exit the file. Now you can start Elasticsearch for the first time with the command:

--sudo service elasticsearch start


5)Securing Elastic

--sudo nano /etc/elasticsearch/elasticsearch.yml
--network.bind_host: localhost
--Also, for additional security you can disable dynamic scripts which are used to evaluate custom expressions. By crafting a custom malicious expression, an attacker might be able to compromise your environment.

To disable custom expressions, add the following line is at the end of the /etc/elasticsearch/elasticsearch.yml file:

	/etc/elasticsearch/elasticsearch.yml
	...
	script.disable_dynamic: true
	...


6)Testing

--By now, Elasticsearch should be running on port 9200. You can test it with curl, the command line client-side URL transfers tool and a simple GET request like this:

curl -X GET 'http://localhost:9200'

7) Using ES
-- curl localhost:9200

8)Installing RABBIT MQ

-- Follow the video : https://www.youtube.com/watch?v=jbucr4wzQtM
-- GO tO : http://www.rabbitmq.com/install-debian.html

--sudo rabbitmq-server start
--sudo add-apt-repository "deb http://www.rabbitmq.com/debian/ testing main"
--wget https://www.rabbitmq.com/rabbitmq-signing-key-public.asc
--sudo apt-key add rabbitmq-signing-key-public.asc
--sudo apt-get update
--sudo apt-get install rabbitmq-server
--sudo rabbitmqctl status
--sudo rabbitmq-plugins enable rabbitmq_management
--now go to browser run : localhost:15672 and login as guest , pass : guest

9) INSTALLING RABBITMQ PLUGIN : https://github.com/elastic/elasticsearch-river-rabbitmq  //optional as done above
--bin/plugin install elasticsearch/elasticsearch-river-rabbitmq/2.6.0


