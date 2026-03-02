## Ajouter le dépôt Graylog et installer Data Node :
```````
wget https://packages.graylog2.org/repo/packages/graylog-7.0-repository_latest.deb
sudo dpkg -i graylog-7.0-repository_latest.deb
sudo apt-get update
sudo apt-get install graylog-datanode
```````
## Générer un ``password_secret`` (chaîne aléatoire longue) :
````
pwgen -N 1 -s 96
````
## Copie la valeur, elle servira pour tous les Data Nodes et pour le serveur Graylog.

## Éditer ``/etc/graylog/datanode/datanode.conf``:
````
sudo nano /etc/graylog/datanode/datanode.conf
````


````
node_id_file = /etc/graylog/datanode/node-id

# location of your data-node configuration files - put additional files like manually created certificates etc. here
config_location = /etc/graylog/datanode

# You MUST set a secret to secure/pepper the stored user passwords here. Use at least 64 characters.
# Generate one by using for example: pwgen -N 1 -s 96
# ATTENTION: This value must be the same on all Graylog and Datanode nodes in the cluster.
# Changing this value after installation will render all user sessions and encrypted values in the database invalid. (e.g. encrypted access tokens)
password_secret = TON_LONG_SECRET_GENERE

# The default root user is named 'admin'
#root_username = admin

# You MUST specify a hash password for the root user (which you only need to initially set up the
# system and in case you lose connectivity to your authentication backend)
# This password cannot be changed using the API or via the web interface. If you need to change it,
# modify it in this file.
# Create one by using for example: echo -n yourpassword | sha256sum
# and put the resulting hash value into the following line
root_password_sha2 =

# connection to MongoDB, shared with the Graylog server
# See https://docs.mongodb.com/manual/reference/connection-string/ for details
mongodb_uri = mongodb://192.168.110.110:27017/graylog

#### HTTP bind address
#
# The network interface used by the Graylog DataNode to bind all services.
#
bind_address = 0.0.0.0

#### Hostname
#
# if you need to specify the hostname to use (because looking it up programmatically gives wrong results)
# hostname =

#### HTTP port
#
# The port where the DataNode REST api is listening
#
datanode_http_port = 8999

#### HTTP publish URI
#
# This configuration should be used if you want to connect to this Graylog DataNode's REST API and it is available on
# another network interface than $http_bind_address,
# for example if the machine has multiple network interfaces or is behind a NAT gateway.
# http_publish_uri =
opensearch_heap = 4g
# exemple de heap à 8 Go pour une machine 16 Go
#### OpenSearch HTTP port
#
# The port where OpenSearch HTTP is listening on
#
opensearch_http_port = 9200

#### OpenSearch transport port
#
# The port where OpenSearch transports is listening on
#
opensearch_transport_port = 9300

#### OpenSearch node name config option
#
# use this, if your node name should be different from the hostname that's found by programmatically looking it up
#
# node_name =

#### OpenSearch discovery_seed_hosts config option
#
# if you're not using the automatic data node setup and want to create a cluster, you have to setup the discovery seed hosts
#
# opensearch_discovery_seed_hosts =

#### OpenSearch initial_manager_nodes config option
#
# if you're not using the automatic data node setup and want to create a cluster, you have to setup the initial manager nodes
# make sure to remove this setting after the cluster has formed
#
# initial_cluster_manager_nodes =

#### OpenSearch folders
#
# set these if you need OpenSearch to be located in a special place or want to include an existing version
#
# Root directory of the used opensearch distribution
opensearch_location = /usr/share/graylog-datanode/dist

opensearch_config_location = /var/lib/graylog-datanode/opensearch/config
opensearch_data_location = /var/lib/graylog-datanode/opensearch/data
opensearch_logs_location = /var/log/graylog-datanode/opensearch

#### OpenSearch Certificate bundles for transport and http layer security
#
# if you're not using the automatic data node setup, you can manually configure your SSL certificates
# transport_certificate = datanode-transport-certificates.p12
# transport_certificate_password = password
# http_certificate = datanode-http-certificates.p12
# http_certificate_password = password

#### OpenSearch log buffers size
#
# the number of lines from stderr and stdout of the OpenSearch process that are buffered inside the DataNode for logging etc.
#
# process_logs_buffer_size = 500

#### OpenSearch JWT token usage
#
# communication between Graylog and OpenSearch is secured by JWT. These are the defaults used for the token usage
# adjust them, if you have special needs.
#
# indexer_jwt_auth_token_caching_duration = 60s
# indexer_jwt_auth_token_expiration_duration = 180s
````
## Activer et démarrer le service Data Node :
```
sudo systemctl daemon-reload
sudo systemctl enable graylog-datanode.service
sudo systemctl start graylog-datanode.service
```
Tu répètes ces étapes sur chaque nœud Data Node de ton cluster, **en réutilisant le même password_secret**
