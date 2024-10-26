# Docker Compose for Kafka and Zookeeper

Este archivo `docker-compose.yml` define dos servicios: Zookeeper y Kafka. Ambos son necesarios para ejecutar un clúster de Apache Kafka en contenedores Docker. 

## Requisitos Previos

- [Docker](https://docs.docker.com/get-docker/) y [Docker Compose](https://docs.docker.com/compose/install/) deben estar instalados en tu sistema.

## Servicios

### 1. Zookeeper

Zookeeper es un servicio de coordinación que Kafka utiliza para mantener su configuración de clúster, realizar seguimiento de los brokers y administrar otros detalles de configuración.

- **Imagen**: `confluentinc/cp-zookeeper:latest`
- **Puertos**: Expone el puerto `2181` para la conexión de clientes.
- **Variables de Entorno**:
  - `ZOOKEEPER_CLIENT_PORT`: El puerto de cliente configurado en `2181`.
  - `ZOOKEEPER_TICK_TIME`: Tiempo de latencia del clúster de Zookeeper configurado en `2000` milisegundos (2 segundos).

### 2. Kafka

Kafka es un sistema de mensajería distribuido de alto rendimiento que permite transmitir mensajes en tiempo real a través de tópicos.

- **Imagen**: `confluentinc/cp-kafka:latest`
- **Dependencias**: Depende del servicio `zookeeper` para iniciar.
- **Puertos**: Expone el puerto `9092` para la conexión de clientes.
- **Variables de Entorno**:
  - `KAFKA_BROKER_ID`: Identificador único del broker, en este caso `1`.
  - `KAFKA_ZOOKEEPER_CONNECT`: Configura la conexión a Zookeeper en `zookeeper:2181`.
  - `KAFKA_ADVERTISED_LISTENERS`: Define el host y puerto de Kafka para los clientes como `PLAINTEXT://localhost:9092`.
  - `KAFKA_LISTENER_SECURITY_PROTOCOL_MAP`: Define el mapa de protocolo de seguridad como `PLAINTEXT`.
  - `KAFKA_AUTO_CREATE_TOPICS_ENABLE`: Activa la creación automática de tópicos, configurado en `true`.
  - `KAFKA_NUM_PARTITIONS`: Define el número predeterminado de particiones para los tópicos en `10`.
  - `KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR`: Factor de replicación para el tópico de offsets configurado en `1`.
- **Volúmenes**: Monte `./kafka-data` al directorio interno `/var/lib/kafka/data` para persistir los datos de Kafka.

## Uso

1. Inicia los contenedores ejecutando:

   ```bash
   docker-compose up -d
    ```
2. Verifica el estado de los contenedores:configs:

   ```bash
   docker-compose ps
   ```
3. Para detener los contenedores, ejecuta:

   ```bash
    docker-compose down
    ```