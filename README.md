# 📊 Kafka Infrastructure

Infraestrutura Apache Kafka com Docker Compose para streaming de dados e processamento de eventos em tempo real.

## 🏗️ Componentes

- **Apache Kafka** - Message broker distribuído
- **Zookeeper** - Coordenação de cluster (ou KRaft mode)
- **Schema Registry** - Gerenciamento de schemas
- **Kafka Connect** - Integração de dados
- **KSQL Server** - Processamento de streams SQL

## 🚀 Quick Start

### Pré-requisitos

- Docker 20.10+
- Docker Compose 2.0+
- Mínimo 4GB RAM disponível

### Inicialização

```bash
# Clone o repositório
git clone <repository-url>
cd conexao-de-sorte-kafka-infraestrutura

# Copie as variáveis de ambiente
cp .env.example .env

# Configure as variáveis necessárias
vim .env

# Inicie os serviços
docker-compose up -d

# Verifique os logs
docker-compose logs -f
```

### Verificação

```bash
# Listar tópicos
docker-compose exec kafka kafka-topics.sh --bootstrap-server localhost:9092 --list

# Criar um tópico de teste
docker-compose exec kafka kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test-topic --partitions 3 --replication-factor 1

# Produzir mensagens
docker-compose exec kafka kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test-topic

# Consumir mensagens
docker-compose exec kafka kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test-topic --from-beginning
```

## 🔧 Configuração

### Variáveis de Ambiente

- `KAFKA_CLUSTER_ID` - ID único do cluster
- `KAFKA_HEAP_OPTS` - Configurações de memória JVM
- `KAFKA_BROKER_ID` - ID único do broker
- `KAFKA_LISTENERS` - Configuração de listeners
- `KAFKA_ADVERTISED_LISTENERS` - Listeners anunciados

### Rede

O cluster Kafka utiliza a rede Docker `kafka-network` para comunicação interna.

### Volumes

- `kafka-data` - Dados persistentes do Kafka
- `zookeeper-data` - Dados do Zookeeper
- `kafka-logs` - Logs do Kafka

## 📊 Monitoramento

### Health Checks

```bash
# Status dos containers
docker-compose ps

# Health check do Kafka
docker-compose exec kafka kafka-broker-api-versions.sh --bootstrap-server localhost:9092

# Métricas JMX (se habilitado)
curl http://localhost:9999/metrics
```

### Logs

```bash
# Logs do Kafka
docker-compose logs kafka

# Logs do Zookeeper
docker-compose logs zookeeper

# Logs em tempo real
docker-compose logs -f
```

## 🛡️ Segurança

### Autenticação SASL

```yaml
KAFKA_SASL_ENABLED_MECHANISMS: SCRAM-SHA-256
KAFKA_SASL_MECHANISM_INTER_BROKER_PROTOCOL: SCRAM-SHA-256
```

### SSL/TLS

```yaml
KAFKA_SSL_KEYSTORE_LOCATION: /etc/kafka/ssl/kafka.keystore.jks
KAFKA_SSL_TRUSTSTORE_LOCATION: /etc/kafka/ssl/kafka.truststore.jks
```

### ACLs (Access Control Lists)

```bash
# Criar ACL para usuário
docker-compose exec kafka kafka-acls.sh --bootstrap-server localhost:9092 --add --allow-principal User:producer --operation Write --topic my-topic
```

## 🚀 Produção

### Scaling

```bash
# Adicionar brokers
docker-compose scale kafka=3

# Rebalanceamento de partições
docker-compose exec kafka kafka-reassign-partitions.sh --bootstrap-server localhost:9092 --reassignment-json-file reassignment.json --execute
```

### Backup

```bash
# Backup de configurações
docker-compose exec kafka kafka-configs.sh --bootstrap-server localhost:9092 --describe --entity-type topics > topics-backup.txt

# Backup de dados (via Kafka Connect)
# Configure conectores apropriados no Connect
```

### Performance Tuning

- Ajuste `num.network.threads` e `num.io.threads`
- Configure `log.segment.bytes` adequadamente  
- Otimize `batch.size` e `linger.ms` nos produtores
- Configure `fetch.min.bytes` nos consumidores

## 🔗 Integração

### Microserviços

Os microserviços se conectam via:
- **Producers**: Publicam eventos nos tópicos
- **Consumers**: Consomem eventos dos tópicos
- **Schema Registry**: Validação e evolução de schemas

### Conectores

- **JDBC Connector** - Integração com bancos de dados
- **Elasticsearch Connector** - Indexação de dados
- **S3 Connector** - Armazenamento em nuvem

## 📋 Troubleshooting

### Problemas Comuns

1. **Kafka não inicia**: Verificar logs do Zookeeper primeiro
2. **Conexão recusada**: Verificar configuração de `KAFKA_ADVERTISED_LISTENERS`
3. **Consumidor lento**: Ajustar configurações de fetch e partições
4. **Disk full**: Configurar retenção de logs adequadamente

### Comandos Úteis

```bash
# Verificar grupos de consumidores
docker-compose exec kafka kafka-consumer-groups.sh --bootstrap-server localhost:9092 --list

# Descrever grupo de consumidor
docker-compose exec kafka kafka-consumer-groups.sh --bootstrap-server localhost:9092 --describe --group my-group

# Reset offset
docker-compose exec kafka kafka-consumer-groups.sh --bootstrap-server localhost:9092 --reset-offsets --to-earliest --topic my-topic --group my-group --execute
```

## 📈 Métricas

### JMX Metrics

- `kafka.server:type=BrokerTopicMetrics` - Métricas de tópicos
- `kafka.server:type=KafkaRequestHandlerPool` - Pool de threads
- `kafka.network:type=RequestMetrics` - Métricas de requisições

### Prometheus Integration

Configure JMX Exporter para exposição de métricas Prometheus.

## 🔄 CI/CD

Pipeline automatizado com:
- ✅ Validação de Docker Compose
- 🧪 Testes de conectividade
- 🛡️ Security scanning
- 🚀 Deploy automatizado
- 📢 Notificações Slack

---

**Generated with Claude Code**  
Co-Authored-By: Claude <noreply@anthropic.com>