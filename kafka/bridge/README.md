# Kafka Bridge

Cria um interface RESTful para comunicação com o Kafka

## Configuração
```bash
kubectl apply -f bridge.yaml
```

Fazer port-forward para testes:
```bash
kubectl port-forward svc/kafka-bridge-bridge-service 8080:8080
```

Nota: Recomenda-se criar um ingress para utilização do bridge

```bash
$ curl http://localhost:8080
{"bridge_version":"0.26.1"}
```

## GET /healthy
Ao chamar `http://localhost:8080/healthy` o retorno será branco, contudo o código de resposta HTTP deve ser 200, indicando que está OK.

## Operações Comuns

| Path | Método | Descrição |
| - | - | - |
| / | GET | Versão do Kafka Bridge |
| /healthy | GET | Retorna 204 se Bridge está rodando sem problemas |
| /ready   | GET | Verifica se Bridge está pronto e aceitando conexões |
| /metrics | GET | Métricas do Prometheus |
| /topics | GET | Retorna lista dos tópicos |
| /topics/{topicname} | GET | Retorna metadata sobre o tópico |
| /consumers/{groupid} | POST | Criar um consumer no consumergroup |

## Produzindo Mensagens
Abaixo exemplo em curl de como produzir mensagens para o Kafka utilizando o Bridge:

```bash
curl -X POST \
  http://localhost:8080/topics/teste \
  -H 'content-type: application/vnd.kafka.json.v2+json' \
  -d '{
    "records": [
        {
            "key": "key-1",
            "value": "value-1"
        },
        {
            "key": "key-2",
            "value": "value-2"
        }
    ]
}'
```

## Lendo mensagens

1) Criar um consumer/consumer-group
2) Subscription do consumer num topico

---

1) Criar um consumer/consumer-group

```bash
curl -X POST http://localhost:8080/consumers/my-group \
  -H 'content-type: application/vnd.kafka.v2+json' \
  -d '{
    "name": "my-consumer",
    "format": "json",
    "auto.offset.reset": "earliest",
    "enable.auto.commit": false
  }'
```

2) Subscription do consumer num topico
```bash
curl -X POST http://localhost:8080/consumers/my-group/instances/my-consumer/subscription \
-H 'content-type: application/vnd.kafka.json.v2+json' \
-d '{
    "topics": [
        "teste"
    ]
}'
```

# Referência
https://strimzi.io/docs/bridge/latest/