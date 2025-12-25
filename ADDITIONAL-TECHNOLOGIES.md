# Variables d'environnement de débogage Spring - Technologies Additionnelles

Ce document complète le README principal en ajoutant les variables d'environnement pour les technologies supplémentaires couramment utilisées avec Spring Boot 3.x.

![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen?logo=spring)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Supported-blue?logo=kubernetes)
![OpenShift](https://img.shields.io/badge/OpenShift-Supported-orange?logo=redhatopenshift)
![YAML](https://img.shields.io/badge/Format-YAML-blue?logo=YAML)
![Cache](https://img.shields.io/badge/Cache-Redis%20Hazelcast-blue)
![Reactive](https://img.shields.io/badge/Reactive-WebFlux-success)
![GraphQL](https://img.shields.io/badge/GraphQL-Supported-purple)
![gRPC](https://img.shields.io/badge/gRPC-Supported-blue)
![WebSocket](https://img.shields.io/badge/WebSocket-Supported-orange)
![Service Discovery](https://img.shields.io/badge/Service%20Discovery-Eureka%20Consul-blue)
![Circuit Breaker](https://img.shields.io/badge/Circuit%20Breaker-Resilience4j-red)
![Tracing](https://img.shields.io/badge/Tracing-Sleuth%20Zipkin-blue)
![Documentation](https://img.shields.io/badge/Documentation-Complete-brightgreen)

## Sommaire

- [Couche 13 : Cache (Niveau cache)](#couche-13--cache-niveau-cache)
  - [Redis](#redis)
  - [Hazelcast](#hazelcast)
  - [Caffeine](#caffeine)
  - [EhCache](#ehcache)
- [Couche 14 : Reactive / WebFlux (Niveau réactif)](#couche-14--reactive--webflux-niveau-réactif)
- [Couche 15 : Spring Data (Niveau données)](#couche-15--spring-data-niveau-données)
- [Couche 16 : Batch & Scheduling (Niveau traitement par lots)](#couche-16--batch--scheduling-niveau-traitement-par-lots)
- [Couche 17 : Integration (Niveau intégration)](#couche-17--integration-niveau-intégration)
- [Couche 18 : GraphQL (Niveau API GraphQL)](#couche-18--graphql-niveau-api-graphql)
- [Couche 19 : gRPC (Niveau RPC)](#couche-19--grpc-niveau-rpc)
- [Couche 20 : WebSocket (Niveau WebSocket)](#couche-20--websocket-niveau-websocket)
- [Couche 21 : Service Discovery (Niveau découverte de services)](#couche-21--service-discovery-niveau-découverte-de-services)
- [Couche 22 : API Gateway (Niveau passerelle)](#couche-22--api-gateway-niveau-passerelle)
- [Couche 23 : Distributed Tracing (Niveau traçage distribué)](#couche-23--distributed-tracing-niveau-traçage-distribué)
- [Couche 24 : Circuit Breaker & Resilience (Niveau résilience)](#couche-24--circuit-breaker--resilience-niveau-résilience)
- [Couche 25 : Metrics & Observability (Niveau métriques)](#couche-25--metrics--observability-niveau-métriques)
- [Couche 26 : LDAP (Niveau annuaire)](#couche-26--ldap-niveau-annuaire)
- [Couche 27 : Transactions (Niveau transactions)](#couche-27--transactions-niveau-transactions)
- [Couche 28 : AOP (Niveau programmation orientée aspect)](#couche-28--aop-niveau-programmation-orientée-aspect)
- [Couche 29 : Testing (Niveau tests)](#couche-29--testing-niveau-tests)
- [Couche 30 : Bases de données supplémentaires](#couche-30--bases-de-données-supplémentaires)
- [Couche 31 : NoSQL supplémentaires](#couche-31--nosql-supplémentaires)

---

## Couche 13 : Cache (Niveau cache)

### Redis

#### `LOGGING_LEVEL_IO_LETTUCE` / `logging.level.io.lettuce`
Définit le niveau de journalisation pour Lettuce (client Redis asynchrone).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du client Redis Lettuce
- Trace les connexions, les commandes Redis et les réponses
- Utile pour déboguer les problèmes de connexion et de performance Redis

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_IO_LETTUCE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_REDIS` / `logging.level.org.springframework.data.redis`
Définit le niveau de journalisation pour Spring Data Redis.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Data Redis
- Trace les opérations sur les repositories Redis et les templates
- Utile pour déboguer les problèmes de cache et de stockage Redis

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_REDIS
    value: "DEBUG"
```

#### `LOGGING_LEVEL_REDIS_CLIENTS_JEDIS` / `logging.level.redis.clients.jedis`
Définit le niveau de journalisation pour Jedis (client Redis synchrone).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du client Redis Jedis
- Trace les connexions, les commandes Redis et les réponses
- Utile pour déboguer les problèmes avec Jedis

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_REDIS_CLIENTS_JEDIS
    value: "DEBUG"
```

### Hazelcast

#### `LOGGING_LEVEL_COM_HAZELCAST` / `logging.level.com.hazelcast`
Définit le niveau de journalisation pour Hazelcast.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Hazelcast
- Trace la gestion du cluster, le cache distribué et les opérations de map
- Utile pour déboguer les problèmes de cluster et de cache distribué

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_HAZELCAST
    value: "DEBUG"
```

### Caffeine

#### `LOGGING_LEVEL_COM_GITHUB_BEN_MANES_CAFFEINE` / `logging.level.com.github.benmanes.caffeine`
Définit le niveau de journalisation pour Caffeine Cache.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Caffeine Cache
- Trace les hits, misses et les évictions du cache
- Utile pour déboguer les problèmes de performance du cache

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_GITHUB_BEN_MANES_CAFFEINE
    value: "DEBUG"
```

### EhCache

#### `LOGGING_LEVEL_NET_SF_EHCACHE` / `logging.level.net.sf.ehcache`
Définit le niveau de journalisation pour EhCache.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations EhCache
- Trace les opérations de cache, les évictions et la configuration
- Utile pour déboguer les problèmes de cache EhCache

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_NET_SF_EHCACHE
    value: "DEBUG"
```

---

## Couche 14 : Reactive / WebFlux (Niveau réactif)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_REACTIVE` / `logging.level.org.springframework.web.reactive`
Définit le niveau de journalisation pour Spring WebFlux.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring WebFlux
- Trace les handlers réactifs, les filtres et le routage
- Utile pour déboguer les applications réactives

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_REACTIVE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_REACTOR` / `logging.level.reactor`
Définit le niveau de journalisation pour Reactor (bibliothèque réactive).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Reactor
- Trace les flux réactifs, les opérateurs et les backpressure
- Utile pour déboguer les problèmes de programmation réactive

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_REACTOR
    value: "DEBUG"
```

#### `LOGGING_LEVEL_REACTOR_NETTY` / `logging.level.reactor.netty`
Définit le niveau de journalisation pour Reactor Netty (serveur réactif).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Reactor Netty
- Trace les connexions, les requêtes HTTP et les réponses
- Utile pour déboguer les problèmes de serveur réactif

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_REACTOR_NETTY
    value: "DEBUG"
```

#### `LOGGING_LEVEL_IO_NETTY` / `logging.level.io.netty`
Définit le niveau de journalisation pour Netty.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Netty
- Trace les connexions, les buffers et les handlers
- Utile pour un débogage approfondi de Netty

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_IO_NETTY
    value: "DEBUG"
```

---

## Couche 15 : Spring Data (Niveau données)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA` / `logging.level.org.springframework.data`
Définit le niveau de journalisation pour Spring Data.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer toutes les opérations Spring Data
- Trace les repositories, les requêtes et les opérations de données
- Utile pour déboguer les problèmes avec Spring Data

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_JPA` / `logging.level.org.springframework.data.jpa`
Définit le niveau de journalisation pour Spring Data JPA.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Data JPA
- Trace les repositories JPA, les requêtes générées et les transactions
- Utile pour déboguer les problèmes avec les repositories JPA

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_JPA
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_REST` / `logging.level.org.springframework.data.rest`
Définit le niveau de journalisation pour Spring Data REST.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Data REST
- Trace l'exposition REST des repositories et les requêtes HTTP
- Utile pour déboguer les problèmes avec les APIs REST générées

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_REST
    value: "DEBUG"
```

---

## Couche 16 : Batch & Scheduling (Niveau traitement par lots)

### Spring Batch

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BATCH` / `logging.level.org.springframework.batch`
Définit le niveau de journalisation pour Spring Batch.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Batch
- Trace l'exécution des jobs, des steps et des tâches
- Utile pour déboguer les problèmes de traitement par lots

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BATCH
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BATCH_CORE` / `logging.level.org.springframework.batch.core`
Définit le niveau de journalisation pour le cœur de Spring Batch.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations core de Spring Batch
- Trace la gestion des jobs, des métadonnées et de l'exécution
- Utile pour déboguer les problèmes de gestion des jobs

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BATCH_CORE
    value: "DEBUG"
```

### Quartz Scheduler

#### `LOGGING_LEVEL_ORG_QUARTZ` / `logging.level.org.quartz`
Définit le niveau de journalisation pour Quartz Scheduler.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Quartz Scheduler
- Trace la planification des jobs, les triggers et l'exécution
- Utile pour déboguer les problèmes de planification

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_QUARTZ
    value: "DEBUG"
```

### Spring Scheduling

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SCHEDULING` / `logging.level.org.springframework.scheduling`
Définit le niveau de journalisation pour Spring Scheduling.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Scheduling
- Trace l'exécution des tâches planifiées (@Scheduled)
- Utile pour déboguer les problèmes de planification

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SCHEDULING
    value: "DEBUG"
```

---

## Couche 17 : Integration (Niveau intégration)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_INTEGRATION` / `logging.level.org.springframework.integration`
Définit le niveau de journalisation pour Spring Integration.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Integration
- Trace les messages, les channels et les endpoints
- Utile pour déboguer les problèmes d'intégration

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_INTEGRATION
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_CAMEL` / `logging.level.org.apache.camel`
Définit le niveau de journalisation pour Apache Camel (si utilisé).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Apache Camel
- Trace les routes, les échanges et les transformations
- Utile pour déboguer les problèmes avec Camel

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_CAMEL
    value: "DEBUG"
```

---

## Couche 18 : GraphQL (Niveau API GraphQL)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_GRAPHQL` / `logging.level.org.springframework.graphql`
Définit le niveau de journalisation pour Spring GraphQL.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring GraphQL
- Trace les requêtes GraphQL, les résolveurs et les erreurs
- Utile pour déboguer les problèmes avec les APIs GraphQL

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_GRAPHQL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_GRAPHQL` / `logging.level.graphql`
Définit le niveau de journalisation pour GraphQL Java.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations GraphQL Java
- Trace le parsing, la validation et l'exécution des requêtes GraphQL
- Utile pour un débogage approfondi de GraphQL

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_GRAPHQL
    value: "DEBUG"
```

---

## Couche 19 : gRPC (Niveau RPC)

#### `LOGGING_LEVEL_IO_GRPC` / `logging.level.io.grpc`
Définit le niveau de journalisation pour gRPC.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations gRPC
- Trace les appels RPC, les stubs et les connexions
- Utile pour déboguer les problèmes avec les services gRPC

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_IO_GRPC
    value: "DEBUG"
```

#### `LOGGING_LEVEL_NET_DEVOPS_BOOT_GRPC` / `logging.level.net.devh.boot.grpc`
Définit le niveau de journalisation pour Spring Boot gRPC.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Boot gRPC
- Trace la configuration et l'exposition des services gRPC
- Utile pour déboguer les problèmes avec Spring Boot gRPC

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_NET_DEVOPS_BOOT_GRPC
    value: "DEBUG"
```

---

## Couche 20 : WebSocket (Niveau WebSocket)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_SOCKET` / `logging.level.org.springframework.web.socket`
Définit le niveau de journalisation pour Spring WebSocket.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring WebSocket
- Trace les connexions WebSocket, les messages et les handlers
- Utile pour déboguer les problèmes avec les WebSockets

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_SOCKET
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_MESSAGING` / `logging.level.org.springframework.messaging`
Définit le niveau de journalisation pour Spring Messaging (STOMP, WebSocket).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Messaging
- Trace les messages STOMP, les subscriptions et les destinations
- Utile pour déboguer les problèmes avec STOMP et WebSocket

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_MESSAGING
    value: "DEBUG"
```

---

## Couche 21 : Service Discovery (Niveau découverte de services)

### Eureka

#### `LOGGING_LEVEL_COM_NETFLIX_EUREKA` / `logging.level.com.netflix.eureka`
Définit le niveau de journalisation pour Eureka Client.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Eureka Client
- Trace l'enregistrement, la découverte et le renouvellement des services
- Utile pour déboguer les problèmes de service discovery

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_NETFLIX_EUREKA
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_NETFLIX_EUREKA` / `logging.level.org.springframework.cloud.netflix.eureka`
Définit le niveau de journalisation pour Spring Cloud Eureka.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Cloud Eureka
- Trace l'intégration Eureka avec Spring Cloud
- Utile pour déboguer les problèmes avec Spring Cloud Eureka

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_NETFLIX_EUREKA
    value: "DEBUG"
```

### Consul

#### `LOGGING_LEVEL_COM_ECWID_CONSUL` / `logging.level.com.ecwid.consul`
Définit le niveau de journalisation pour Consul Client.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Consul Client
- Trace l'enregistrement, la découverte et la configuration Consul
- Utile pour déboguer les problèmes avec Consul

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_ECWID_CONSUL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONSUL` / `logging.level.org.springframework.cloud.consul`
Définit le niveau de journalisation pour Spring Cloud Consul.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Cloud Consul
- Trace l'intégration Consul avec Spring Cloud
- Utile pour déboguer les problèmes avec Spring Cloud Consul

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONSUL
    value: "DEBUG"
```

### Spring Cloud LoadBalancer

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_LOADBALANCER` / `logging.level.org.springframework.cloud.loadbalancer`
Définit le niveau de journalisation pour Spring Cloud LoadBalancer.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Cloud LoadBalancer
- Trace la sélection des instances et la répartition de charge
- Utile pour déboguer les problèmes de load balancing

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_LOADBALANCER
    value: "DEBUG"
```

---

## Couche 22 : API Gateway (Niveau passerelle)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_GATEWAY` / `logging.level.org.springframework.cloud.gateway`
Définit le niveau de journalisation pour Spring Cloud Gateway.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Cloud Gateway
- Trace le routage, les filtres et les prédicats
- Utile pour déboguer les problèmes de routage et de filtrage

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_GATEWAY
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_GATEWAY_FILTER` / `logging.level.org.springframework.cloud.gateway.filter`
Définit le niveau de journalisation pour les filtres Spring Cloud Gateway.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer l'exécution des filtres Gateway
- Trace l'ordre d'exécution et les modifications des requêtes/réponses
- Utile pour déboguer les problèmes avec les filtres Gateway

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_GATEWAY_FILTER
    value: "DEBUG"
```

---

## Couche 23 : Distributed Tracing (Niveau traçage distribué)

### Spring Cloud Sleuth

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_SLEUTH` / `logging.level.org.springframework.cloud.sleuth`
Définit le niveau de journalisation pour Spring Cloud Sleuth.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Cloud Sleuth
- Trace la création et la propagation des traces
- Utile pour déboguer les problèmes de traçage distribué

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_SLEUTH
    value: "DEBUG"
```

#### `LOGGING_LEVEL_BRAVE` / `logging.level.brave`
Définit le niveau de journalisation pour Brave (bibliothèque de traçage).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Brave
- Trace la création des spans et la propagation des traces
- Utile pour un débogage approfondi du traçage

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_BRAVE
    value: "DEBUG"
```

### Micrometer Tracing

#### `LOGGING_LEVEL_IO_MICROMETER_TRACING` / `logging.level.io.micrometer.tracing`
Définit le niveau de journalisation pour Micrometer Tracing.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Micrometer Tracing
- Trace la création et l'exportation des traces
- Utile pour déboguer les problèmes avec Micrometer Tracing

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_IO_MICROMETER_TRACING
    value: "DEBUG"
```

---

## Couche 24 : Circuit Breaker & Resilience (Niveau résilience)

### Resilience4j

#### `LOGGING_LEVEL_IO_GITHUB_RESILIENCE4J` / `logging.level.io.github.resilience4j`
Définit le niveau de journalisation pour Resilience4j.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Resilience4j
- Trace les circuit breakers, les retries et les timeouts
- Utile pour déboguer les problèmes de résilience

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_IO_GITHUB_RESILIENCE4J
    value: "DEBUG"
```

#### `LOGGING_LEVEL_IO_GITHUB_RESILIENCE4J_CIRCUITBREAKER` / `logging.level.io.github.resilience4j.circuitbreaker`
Définit le niveau de journalisation pour les circuit breakers Resilience4j.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer spécifiquement les circuit breakers
- Trace les ouvertures, fermetures et états des circuit breakers
- Utile pour déboguer les problèmes de circuit breakers

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_IO_GITHUB_RESILIENCE4J_CIRCUITBREAKER
    value: "DEBUG"
```

### Spring Cloud Circuit Breaker

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CIRCUITBREAKER` / `logging.level.org.springframework.cloud.circuitbreaker`
Définit le niveau de journalisation pour Spring Cloud Circuit Breaker.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Cloud Circuit Breaker
- Trace l'utilisation des circuit breakers dans Spring Cloud
- Utile pour déboguer les problèmes avec Spring Cloud Circuit Breaker

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CIRCUITBREAKER
    value: "DEBUG"
```

---

## Couche 25 : Metrics & Observability (Niveau métriques)

#### `LOGGING_LEVEL_IO_MICROMETER` / `logging.level.io.micrometer`
Définit le niveau de journalisation pour Micrometer.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Micrometer
- Trace la création, la mise à jour et l'exportation des métriques
- Utile pour déboguer les problèmes de métriques

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_IO_MICROMETER
    value: "DEBUG"
```

#### `LOGGING_LEVEL_IO_MICROMETER_PROMETHEUS` / `logging.level.io.micrometer.prometheus`
Définit le niveau de journalisation pour l'export Prometheus de Micrometer.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer l'export Prometheus des métriques
- Trace la conversion et l'exposition des métriques au format Prometheus
- Utile pour déboguer les problèmes avec Prometheus

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_IO_MICROMETER_PROMETHEUS
    value: "DEBUG"
```

---

## Couche 26 : LDAP (Niveau annuaire)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_LDAP` / `logging.level.org.springframework.ldap`
Définit le niveau de journalisation pour Spring LDAP.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring LDAP
- Trace les connexions LDAP, les recherches et les authentifications
- Utile pour déboguer les problèmes avec LDAP

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_LDAP
    value: "DEBUG"
```

#### `LOGGING_LEVEL_COM_SUN_JNDI_LDAP` / `logging.level.com.sun.jndi.ldap`
Définit le niveau de journalisation pour JNDI LDAP.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations JNDI LDAP
- Trace les connexions et les opérations LDAP au niveau JNDI
- Utile pour un débogage approfondi de LDAP

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_SUN_JNDI_LDAP
    value: "DEBUG"
```

---

## Couche 27 : Transactions (Niveau transactions)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_TRANSACTION` / `logging.level.org.springframework.transaction`
Définit le niveau de journalisation pour Spring Transactions.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations de transactions Spring
- Trace le début, la validation, le rollback des transactions
- Utile pour déboguer les problèmes de gestion des transactions

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_TRANSACTION
    value: "DEBUG"
```

#### `LOGGING_LEVEL_JAVAX_TRANSACTION` / `logging.level.javax.transaction`
Définit le niveau de journalisation pour JTA (Java Transaction API).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations JTA
- Trace les transactions distribuées et la gestion JTA
- Utile pour déboguer les problèmes avec les transactions distribuées

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVAX_TRANSACTION
    value: "DEBUG"
```

---

## Couche 28 : AOP (Niveau programmation orientée aspect)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_AOP` / `logging.level.org.springframework.aop`
Définit le niveau de journalisation pour Spring AOP.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring AOP
- Trace la création des proxies, l'exécution des aspects et des advices
- Utile pour déboguer les problèmes avec les aspects et les intercepteurs

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_AOP
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_ASPEJCTJ` / `logging.level.org.aspectj`
Définit le niveau de journalisation pour AspectJ.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations AspectJ
- Trace le weaving et l'exécution des aspects AspectJ
- Utile pour déboguer les problèmes avec AspectJ

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_ASPEJCTJ
    value: "DEBUG"
```

---

## Couche 29 : Testing (Niveau tests)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_TEST` / `logging.level.org.springframework.test`
Définit le niveau de journalisation pour Spring Test.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Test
- Trace le chargement des contextes de test et l'exécution des tests
- Utile pour déboguer les problèmes dans les tests

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_TEST
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_TESTCONTAINERS` / `logging.level.org.testcontainers`
Définit le niveau de journalisation pour TestContainers.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations TestContainers
- Trace le démarrage, l'arrêt et la gestion des conteneurs de test
- Utile pour déboguer les problèmes avec TestContainers

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_TESTCONTAINERS
    value: "DEBUG"
```

---

## Couche 30 : Bases de données supplémentaires

### SQL Server

#### `LOGGING_LEVEL_COM_MICROSOFT_SQLSERVER` / `logging.level.com.microsoft.sqlserver`
Définit le niveau de journalisation pour le driver SQL Server.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du driver SQL Server
- Trace les connexions, les requêtes et les erreurs SQL Server
- Utile pour déboguer les problèmes de connexion et de requêtes SQL Server

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_MICROSOFT_SQLSERVER
    value: "DEBUG"
```

### DB2

#### `LOGGING_LEVEL_COM_IBM_DB2` / `logging.level.com.ibm.db2`
Définit le niveau de journalisation pour le driver DB2.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du driver DB2
- Trace les connexions, les requêtes et les erreurs DB2
- Utile pour déboguer les problèmes de connexion et de requêtes DB2

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_IBM_DB2
    value: "DEBUG"
```

### H2

#### `LOGGING_LEVEL_ORG_H2` / `logging.level.org.h2`
Définit le niveau de journalisation pour H2 Database (souvent utilisé pour les tests).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations H2 Database
- Trace les connexions, les requêtes et les opérations sur la base H2
- Utile pour déboguer les problèmes avec H2 (souvent utilisé en développement/test)

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_H2
    value: "DEBUG"
```

### Derby

#### `LOGGING_LEVEL_ORG_APACHE_DERBY` / `logging.level.org.apache.derby`
Définit le niveau de journalisation pour Apache Derby.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Apache Derby
- Trace les connexions, les requêtes et les erreurs Derby
- Utile pour déboguer les problèmes avec Derby

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_DERBY
    value: "DEBUG"
```

---

## Couche 31 : NoSQL supplémentaires

### Cassandra

#### `LOGGING_LEVEL_COM_DATASTAX` / `logging.level.com.datastax`
Définit le niveau de journalisation pour DataStax Cassandra Driver.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du driver Cassandra
- Trace les connexions, les requêtes CQL et les erreurs Cassandra
- Utile pour déboguer les problèmes de connexion et de requêtes Cassandra

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_DATASTAX
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_CASSANDRA` / `logging.level.org.springframework.data.cassandra`
Définit le niveau de journalisation pour Spring Data Cassandra.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Data Cassandra
- Trace les interactions avec les repositories Cassandra
- Utile pour déboguer les problèmes de mapping et de requêtes Cassandra

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_CASSANDRA
    value: "DEBUG"
```

### Couchbase

#### `LOGGING_LEVEL_COM_COUCHBASE` / `logging.level.com.couchbase`
Définit le niveau de journalisation pour Couchbase Client.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Couchbase Client
- Trace les connexions, les opérations N1QL et les erreurs Couchbase
- Utile pour déboguer les problèmes de connexion et de requêtes Couchbase

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_COUCHBASE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_COUCHBASE` / `logging.level.org.springframework.data.couchbase`
Définit le niveau de journalisation pour Spring Data Couchbase.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Data Couchbase
- Trace les interactions avec les repositories Couchbase
- Utile pour déboguer les problèmes de mapping et de requêtes Couchbase

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_COUCHBASE
    value: "DEBUG"
```

### Neo4j

#### `LOGGING_LEVEL_ORG_NEO4J` / `logging.level.org.neo4j`
Définit le niveau de journalisation pour Neo4j Driver.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Neo4j Driver
- Trace les connexions, les requêtes Cypher et les erreurs Neo4j
- Utile pour déboguer les problèmes de connexion et de requêtes Neo4j

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_NEO4J
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_NEO4J` / `logging.level.org.springframework.data.neo4j`
Définit le niveau de journalisation pour Spring Data Neo4j.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Data Neo4j
- Trace les interactions avec les repositories Neo4j
- Utile pour déboguer les problèmes de mapping et de requêtes Neo4j

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_NEO4J
    value: "DEBUG"
```

---

## Liste des configurations par couche (Technologies Additionnelles)

```yaml
env:
  # ============================================
  # Couche 13 : Cache (Niveau cache)
  # ============================================
  # Redis
  - name: LOGGING_LEVEL_IO_LETTUCE
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_REDIS
    value: "DEBUG"
  - name: LOGGING_LEVEL_REDIS_CLIENTS_JEDIS
    value: "DEBUG"
  # Hazelcast
  - name: LOGGING_LEVEL_COM_HAZELCAST
    value: "DEBUG"
  # Caffeine
  - name: LOGGING_LEVEL_COM_GITHUB_BEN_MANES_CAFFEINE
    value: "DEBUG"
  # EhCache
  - name: LOGGING_LEVEL_NET_SF_EHCACHE
    value: "DEBUG"

  # ============================================
  # Couche 14 : Reactive / WebFlux (Niveau réactif)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_REACTIVE
    value: "DEBUG"
  - name: LOGGING_LEVEL_REACTOR
    value: "DEBUG"
  - name: LOGGING_LEVEL_REACTOR_NETTY
    value: "DEBUG"
  - name: LOGGING_LEVEL_IO_NETTY
    value: "DEBUG"

  # ============================================
  # Couche 15 : Spring Data (Niveau données)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_JPA
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_REST
    value: "DEBUG"

  # ============================================
  # Couche 16 : Batch & Scheduling (Niveau traitement par lots)
  # ============================================
  # Spring Batch
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BATCH
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BATCH_CORE
    value: "DEBUG"
  # Quartz
  - name: LOGGING_LEVEL_ORG_QUARTZ
    value: "DEBUG"
  # Spring Scheduling
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SCHEDULING
    value: "DEBUG"

  # ============================================
  # Couche 17 : Integration (Niveau intégration)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_INTEGRATION
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_APACHE_CAMEL
    value: "DEBUG"

  # ============================================
  # Couche 18 : GraphQL (Niveau API GraphQL)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_GRAPHQL
    value: "DEBUG"
  - name: LOGGING_LEVEL_GRAPHQL
    value: "DEBUG"

  # ============================================
  # Couche 19 : gRPC (Niveau RPC)
  # ============================================
  - name: LOGGING_LEVEL_IO_GRPC
    value: "DEBUG"
  - name: LOGGING_LEVEL_NET_DEVOPS_BOOT_GRPC
    value: "DEBUG"

  # ============================================
  # Couche 20 : WebSocket (Niveau WebSocket)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_SOCKET
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_MESSAGING
    value: "DEBUG"

  # ============================================
  # Couche 21 : Service Discovery (Niveau découverte de services)
  # ============================================
  # Eureka
  - name: LOGGING_LEVEL_COM_NETFLIX_EUREKA
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_NETFLIX_EUREKA
    value: "DEBUG"
  # Consul
  - name: LOGGING_LEVEL_COM_ECWID_CONSUL
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONSUL
    value: "DEBUG"
  # LoadBalancer
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_LOADBALANCER
    value: "DEBUG"

  # ============================================
  # Couche 22 : API Gateway (Niveau passerelle)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_GATEWAY
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_GATEWAY_FILTER
    value: "DEBUG"

  # ============================================
  # Couche 23 : Distributed Tracing (Niveau traçage distribué)
  # ============================================
  # Sleuth
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_SLEUTH
    value: "DEBUG"
  - name: LOGGING_LEVEL_BRAVE
    value: "DEBUG"
  # Micrometer Tracing
  - name: LOGGING_LEVEL_IO_MICROMETER_TRACING
    value: "DEBUG"

  # ============================================
  # Couche 24 : Circuit Breaker & Resilience (Niveau résilience)
  # ============================================
  # Resilience4j
  - name: LOGGING_LEVEL_IO_GITHUB_RESILIENCE4J
    value: "DEBUG"
  - name: LOGGING_LEVEL_IO_GITHUB_RESILIENCE4J_CIRCUITBREAKER
    value: "DEBUG"
  # Spring Cloud Circuit Breaker
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CIRCUITBREAKER
    value: "DEBUG"

  # ============================================
  # Couche 25 : Metrics & Observability (Niveau métriques)
  # ============================================
  - name: LOGGING_LEVEL_IO_MICROMETER
    value: "DEBUG"
  - name: LOGGING_LEVEL_IO_MICROMETER_PROMETHEUS
    value: "DEBUG"

  # ============================================
  # Couche 26 : LDAP (Niveau annuaire)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_LDAP
    value: "DEBUG"
  - name: LOGGING_LEVEL_COM_SUN_JNDI_LDAP
    value: "DEBUG"

  # ============================================
  # Couche 27 : Transactions (Niveau transactions)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_TRANSACTION
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVAX_TRANSACTION
    value: "DEBUG"

  # ============================================
  # Couche 28 : AOP (Niveau programmation orientée aspect)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_AOP
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_ASPEJCTJ
    value: "DEBUG"

  # ============================================
  # Couche 29 : Testing (Niveau tests)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_TEST
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_TESTCONTAINERS
    value: "DEBUG"

  # ============================================
  # Couche 30 : Bases de données supplémentaires
  # ============================================
  - name: LOGGING_LEVEL_COM_MICROSOFT_SQLSERVER
    value: "DEBUG"
  - name: LOGGING_LEVEL_COM_IBM_DB2
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_H2
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_APACHE_DERBY
    value: "DEBUG"

  # ============================================
  # Couche 31 : NoSQL supplémentaires
  # ============================================
  # Cassandra
  - name: LOGGING_LEVEL_COM_DATASTAX
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_CASSANDRA
    value: "DEBUG"
  # Couchbase
  - name: LOGGING_LEVEL_COM_COUCHBASE
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_COUCHBASE
    value: "DEBUG"
  # Neo4j
  - name: LOGGING_LEVEL_ORG_NEO4J
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_NEO4J
    value: "DEBUG"
```

---

## Notes importantes

- Ce document complète le README principal avec des technologies supplémentaires
- Toutes les variables suivent le même format que le README principal
- Les configurations sont fournies au format YAML pour Kubernetes/OpenShift
- Certaines technologies peuvent nécessiter des dépendances spécifiques dans votre projet
- Consultez la documentation officielle de chaque technologie pour plus de détails

---

## Références

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Spring Cloud Documentation](https://spring.io/projects/spring-cloud)
- [Spring Data Documentation](https://spring.io/projects/spring-data)
- [Spring Security Documentation](https://spring.io/projects/spring-security)

