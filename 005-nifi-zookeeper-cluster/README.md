To interact with ZooKeeper, execute the following:

```
docker compose exec -it zookeeper1 /bin/bash
```


Once inside the ZooKeeper container, start the CLI:

to know zookeeper commands, may vary depends on docker container used for ZK
```
ls

ls bin

ls /opt
```

```
./bin/zkCli.sh
```

Use Ctrl + L to clear the screen in zkCli

```

ls /

ls /nifi

ls /nifi/leaders

```

```
ls "/nifi/leaders/Cluster Coordinator"
```

```
ls "/nifi/leaders/Primary Node"
```

```
get "/nifi/leaders/Cluster Coordinator/<<node-id>>"
```


```
get "/nifi/leaders/Primary Node/<<node-id>>"
```


if 8080 does not work, use 8081, 8082.

```
curl http://localhost:8080/nifi-api/controller/cluster | jq
```

Try curl against or browser against all the running nodes
```
curl -v http://localhost:8080/nifi/
curl -v http://localhost:8081/nifi/
curl -v http://localhost:8082/nifi/

```

now stop the active ordinator for a while using docker compose

```
docker compose stop nifi-node1
```


check cluster details on 

```
curl http://localhost:8080/nifi-api/controller/cluster | jq
curl http://localhost:8081/nifi-api/controller/cluster | jq
curl http://localhost:8082/nifi-api/controller/cluster | jq
```

check on zCli on other termial, same list command

notice one of the node is gone from the list.

```
ls "/nifi/leaders/Cluster Coordinator"
```

```
ls "/nifi/leaders/Primary Node"
```


now on Curl [other terminal],  try getting cluster details, notice if any change to cluster ordinator and primary node


if 8080 does not work, use 8081, 8082.

```
curl http://localhost:8080/nifi-api/controller/cluster | jq
curl http://localhost:8081/nifi-api/controller/cluster | jq
curl http://localhost:8082/nifi-api/controller/cluster | jq
```

Start 

```
docker compose start nifi-node1
```


```
ls /nifi/cluster/leader
get /nifi/cluster/leader
```

```
ls /nifi/cluster/failures
```


```
get /zookeeper/config
```

Health check


```
curl http://localhost:8080/nifi-api/flow/status

curl http://localhost:8081/nifi-api/flow/status

curl http://localhost:8082/nifi-api/flow/status
```



 If you want to dedicate specific nodes for Cluster Coordinator and Primary Node roles:

    Use nifi.cluster.coordinator.node.priority to prioritize certain nodes as Cluster Coordinators.

    Use nifi.cluster.node.primary.eligible=false to prevent certain nodes from becoming the Primary Node.

    Use manual API calls to explicitly assign roles when necessary.


    curl -u <username>:<password> -X POST http://<nifi-node>:8080/nifi-api/controller/cluster/nodes/<node-id>/coordinator

curl -u <username>:<password> -X DELETE http://<nifi-node>:8080/nifi-api/controller/cluster/nodes/<node-id>/primary-role


