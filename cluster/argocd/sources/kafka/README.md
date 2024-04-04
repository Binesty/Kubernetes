# Strimzi
* Adição do chart do strimzi
```bash
helm repo add strimzi https://strimzi.io/charts/
helm repo update
```

* Gerar uma saída com os valores do chart
```bash
helm show values strimzi/strimzi-kafka-operator > helm_strimzi_values.yaml
```

* Aplicar a criação do operator
```bash
helm upgrade --install strimzi strimzi/strimzi-kafka-operator \
  -f helm_strimzi_values.yaml \
  --namespace kafka \
  --create-namespace
```

# Kafka
* Aplique o yaml
```bash
kubectl apply -f kafka_cluster.yaml
```

_O processo de deploy do kafka, normalmente, leva em média de 2 minutos_

Essa etapa irá criar vários recursos, como:
| Resource | Name | Comando |
| - | - | - |
| kafka | kafka cluster | kubectl get kafka [-n namespace] |
| kafkatopics | - | kubectl get kafka-topics [-n namespace] |
| kafkauser | - | kubectl get kafkauser [-n namespace] |
| kafkabrige | - | kubectl get kafkabridge [-n namespace] |
| service | {cluster}-kafka-bootstrap<br>{cluster}-kafka-brokers<br>{cluster}-zookeeper-client<br>{cluster}-zookeeper-nodes | kubectl get services [-n namespace] |
| pod | {cluster}-entity-operator<br>{cluster}-kafka-{#}<br>{cluster}-zookeeper-{#0} | kubectl get pods [-n namespace] |
| strimzipodsets | {cluster}-kafka<br>{cluster}-zookeeper | kubectl get strimzipodsets [-n namespace] |
| deployments | cruise-control<br>entity-operator | kubectl get deployments [-n namespace] |

Podemos visualizar o deploy do kafka com:
```bash
$ kubectl get kafka -n kafka
NAME            DESIRED KAFKA REPLICAS   DESIRED ZK REPLICAS   READY   WARNINGS
kafka-cluster   3                        3                     True    
```

# ACLs

## Resources
| Resource | Operation | APIs Allowed |                                         
| - | - | - |                                                                   
| Cluster | Create | CreateTopics<br> Metadata |                                
| Topic | Alter | CreatePartitions |                                            
| Topic | AlterConfigs | AlterConfigs |                                         
| Topic | Create | CreateTopics <br> Metadata|                                  
| Topic | Describe | ListOffsets <br> Metadata <br> OffsetFetch <br> OffsetForLeaderEpoch |
| Topic | Delete | DeleteRecords <br> DeleteTopics |                            
| Topic | Read | Fetch<br> OffsetCommit <br> TxnOffsetCommit |                  
| Topic | Write | Produce<br> AddPartitionsToTxn |                              
