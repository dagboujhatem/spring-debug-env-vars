# Variables d'environnement de débogage Spring

![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-brightgreen?logo=spring)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Supported-blue?logo=kubernetes)
![OpenShift](https://img.shields.io/badge/OpenShift-Supported-orange?logo=redhatopenshift)
![YAML](https://img.shields.io/badge/Format-YAML-blue?logo=YAML)
![License](https://img.shields.io/badge/License-MIT-green)

## Context :

Ce document répertorie toutes les variables d'environnement disponibles pour activer le débogage et le traçage dans Spring Boot 3.x, organisées par couches fonctionnelles.

**Objectif :** Fournir un guide complet pour configurer le logging et le débogage via des variables d'environnement, utilisables directement dans Spring Boot 3.x et déployables sur OpenShift/Kubernetes.

**Organisation :** Les variables sont organisées par couches, du plus haut niveau (Spring Boot) jusqu'au niveau le plus bas (JVM), couvrant :
- Les opérations de fichiers (I/O)
- Les connexions SSL/TLS
- Les accès aux bases de données
- Les requêtes HTTP/Web
- La sécurité Spring
- Et bien plus...

Toutes les configurations sont fournies au format YAML pour une utilisation directe dans Kubernetes/OpenShift.

### Variable globale de debug Spring Boot

#### `SPRING_DEBUG` / `spring.debug`
Active le mode débogage de Spring Boot.

**Valeurs possibles :**
- `true` - Active le mode débogage
- `false` - Désactive le mode débogage (par défaut)
- `1` - Active le mode débogage
- `0` - Désactive le mode débogage

**Exemple :**
```yaml
env:
  - name: SPRING_DEBUG
    value: "true"
```

### Niveaux de logs possibles (VALEURS AUTORISÉES)

Spring Boot repose sur SLF4J / Logback.

**Valeurs possibles pour les niveaux de logging :**
- `TRACE` - Le plus verbeux (affiche tout)
- `DEBUG` - Niveau débogage (détails de développement)
- `INFO` - Niveau info (par défaut)
- `WARN` - Niveau avertissement
- `ERROR` - Niveau erreur
- `OFF` - Désactive la journalisation

**Note :** Les niveaux de journalisation ne sont pas sensibles à la casse.

---
## Sommaire

- [Context](#context-)
- [Variable globale de debug Spring Boot](#variable-globale-de-debug-spring-boot)
- [Niveaux de logs possibles](#niveaux-de-logs-possibles-valeurs-autorisées)
- [Variables d'environnement par couches](#variables-denvironnement-par-couches)
  - [Couche 1 : Spring Boot (Niveau application)](#couche-1--spring-boot-niveau-application)
  - [Couche 2 : Spring Framework (Niveau framework)](#couche-2--spring-framework-niveau-framework)
  - [Couche 3 : Vault & Cloud Config (Niveau configuration externe)](#couche-3--vault--cloud-config-niveau-configuration-externe)
  - [Couche 4 : Java I/O (Niveau Java - Opérations de fichiers)](#couche-4--java-io-niveau-java---opérations-de-fichiers)
  - [Couche 5 : S3 / Cloud Storage (Niveau stockage cloud)](#couche-5--s3--cloud-storage-niveau-stockage-cloud)
  - [Couche 6 : SSL/TLS (Niveau sécurité)](#couche-6--ssltls-niveau-sécurité)
  - [Couche 7 : Database (Niveau base de données)](#couche-7--database-niveau-base-de-données)
    - [Spring JDBC](#spring-jdbc)
    - [Hibernate (ORM)](#hibernate-orm)
    - [Pool de connexions](#pool-de-connexions)
    - [Drivers de base de données](#drivers-de-base-de-données)
      - [MySQL - `LOGGING_LEVEL_COM_MYSQL_CJ`](#logging_level_com_mysql_cj--logginglevelcommysqlcj)
      - [PostgreSQL - `LOGGING_LEVEL_ORG_POSTGRESQL`](#logging_level_org_postgresql--logginglevelorgpostgresql)
      - [Oracle - `LOGGING_LEVEL_ORACLE_JDBC`](#logging_level_oracle_jdbc--loggingleveloraclejdbc)
      - [Teradata - `LOGGING_LEVEL_COM_TERADATA`](#logging_level_com_teradata--logginglevelcomteradata)
    - [Bases de données NoSQL](#bases-de-données-nosql)
      - [Elasticsearch](#logging_level_org_elasticsearch--logginglevelorgelasticsearch)
      - [MongoDB](#logging_level_org_mongodb--logginglevelorgmongodb)
  - [Couche 8 : MQ (Message Queue)](#couche-8--mq-message-queue)
    - [Kafka](#kafka)
    - [RabbitMQ](#rabbitmq)
    - [IBM MQ](#ibm-mq)
  - [Couche 9 : HTTP/Web (Niveau réseau)](#couche-9--httpweb-niveau-réseau)
  - [Couche 10 : Security (Niveau sécurité Spring)](#couche-10--security-niveau-sécurité-spring)
  - [Couche 11 : Actuator & Health Checks (Niveau monitoring)](#couche-11--actuator--health-checks-niveau-monitoring)
  - [Couche 12 : JVM (Niveau machine virtuelle)](#couche-12--jvm-niveau-machine-virtuelle)
- [Configuration complète pour tracer les opérations de fichiers](#configuration-complète-pour-tracer-les-opérations-de-fichiers)
  - [Configuration recommandée pour le débogage des fichiers](#configuration-recommandée-pour-le-débogage-des-fichiers)
  - [Configuration minimale pour le débogage des fichiers](#configuration-minimale-pour-le-débogage-des-fichiers)
  - [Configuration pour OpenShift / Kubernetes](#configuration-pour-openshift--kubernetes)
- [Configuration pour déboguer Vault & Cloud Config](#configuration-pour-déboguer-vault--cloud-config)
  - [Configuration complète pour déboguer la récupération des secrets](#configuration-complète-pour-déboguer-la-récupération-des-secrets)
  - [Configuration minimale pour déboguer Vault & Cloud Config](#configuration-minimale-pour-déboguer-vault--cloud-config)
  - [Ce que vous verrez dans les logs](#ce-que-vous-verrez-dans-les-logs)
  - [Points clés à vérifier dans les logs](#points-clés-à-vérifier-dans-les-logs)
- [Notes importantes](#notes-importantes)
- [Variables supplémentaires utiles](#variables-supplémentaires-utiles)

---
## Liste des configurations par couche

```yaml
env:
  # ============================================
  # Couche 1 : Spring Boot (Niveau application)
  # ============================================
  - name: LOGGING_LEVEL_ROOT
    value: "DEBUG"
  - name: SPRING_PROFILES_ACTIVE
    value: "dev,debug"
  - name: SPRING_OUTPUT_ANSI_ENABLED
    value: "always"
  - name: SPRING_DEBUG
    value: "true"

  # ============================================
  # Couche 2 : Spring Framework (Niveau framework)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CONTEXT
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BEANS
    value: "DEBUG"

  # ============================================
  # Couche 3 : Vault & Cloud Config (Niveau configuration externe)
  # ============================================
  # Cloud Config
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_CLIENT
    value: "DEBUG"
  - name: SPRING_CLOUD_CONFIG_URI
    value: "http://config-server:8888"
  - name: SPRING_CLOUD_CONFIG_NAME
    value: "myapp"
  - name: SPRING_CLOUD_CONFIG_PROFILE
    value: "dev"
  - name: SPRING_CLOUD_CONFIG_FAIL_FAST
    value: "true"
  # Vault
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_CORE
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_AUTHENTICATION
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_LEASE
    value: "DEBUG"
  - name: SPRING_CLOUD_VAULT_URI
    value: "http://vault:8200"
  - name: SPRING_CLOUD_VAULT_AUTHENTICATION
    value: "TOKEN"
  - name: SPRING_CLOUD_VAULT_KV_ENABLED
    value: "true"
  - name: SPRING_CLOUD_VAULT_FAIL_FAST
    value: "true"

  # ============================================
  # Couche 4 : Java I/O (Niveau Java - Opérations de fichiers)
  # ============================================
  - name: LOGGING_LEVEL_JAVA_IO
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_NIO
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_NIO_FILE
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_IO_FILEINPUTSTREAM
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_IO_FILEOUTPUTSTREAM
    value: "DEBUG"

  # ============================================
  # Couche 5 : S3 / Cloud Storage (Niveau stockage cloud)
  # ============================================
  - name: LOGGING_LEVEL_COM_AMAZONAWS
    value: "DEBUG"
  - name: LOGGING_LEVEL_COM_AMAZONAWS_SERVICES_S3
    value: "DEBUG"
  - name: LOGGING_LEVEL_COM_AMAZONAWS_AUTH
    value: "DEBUG"
  - name: AWS_REGION
    value: "us-east-1"
  - name: AWS_S3_BUCKET
    value: "my-bucket-name"
  - name: AWS_S3_ENDPOINT
    value: "http://minio:9000"
  - name: AWS_S3_PATH_STYLE_ACCESS
    value: "true"

  # ============================================
  # Couche 6 : SSL/TLS (Niveau sécurité)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_SSL
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVAX_NET_SSL
    value: "DEBUG"
  - name: LOGGING_LEVEL_SUN_SECURITY_SSL
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_APACHE_TOMCAT_UTIL_NET_SSL
    value: "DEBUG"

  # ============================================
  # Couche 7 : Database (Niveau base de données)
  # ============================================
  # Spring JDBC
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC_CORE
    value: "DEBUG"
  # Hibernate (ORM)
  - name: LOGGING_LEVEL_ORG_HIBERNATE
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_HIBERNATE_SQL
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_HIBERNATE_TYPE_DESCRIPTOR_SQL
    value: "TRACE"
  # Pool de connexions
  - name: LOGGING_LEVEL_COM_ZAXXER_HIKARI
    value: "DEBUG"
  # Drivers de base de données
  - name: LOGGING_LEVEL_COM_MYSQL_CJ
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_POSTGRESQL
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORACLE_JDBC
    value: "DEBUG"
  - name: LOGGING_LEVEL_COM_TERADATA
    value: "DEBUG"
  - name: LOGGING_LEVEL_COM_TERADATA_JDBC
    value: "DEBUG"

  # ============================================
  # Couche 8 : MQ (Message Queue)
  # ============================================
  # Kafka
  - name: LOGGING_LEVEL_ORG_APACHE_KAFKA
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_APACHE_KAFKA_CLIENTS
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_KAFKA
    value: "DEBUG"
  # RabbitMQ
  - name: LOGGING_LEVEL_COM_RABBITMQ
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_AMQP
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_AMQP_RABBIT
    value: "DEBUG"
  # IBM MQ
  - name: LOGGING_LEVEL_COM_IBM_MQ
    value: "DEBUG"
  - name: LOGGING_LEVEL_COM_IBM_MQJMS
    value: "DEBUG"

  # ============================================
  # Couche 9 : HTTP/Web (Niveau réseau)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_SERVLET
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_FILTER
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_APACHE_CATALINA
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_APACHE_CATALINA_CONNECTOR
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_APACHE_HTTP
    value: "DEBUG"

  # ============================================
  # Couche 9 : Security (Niveau sécurité Spring)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_WEB
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_AUTHENTICATION
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_ACCESS
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_OAUTH2
    value: "DEBUG"

  # ============================================
  # Couche 10 : Actuator & Health Checks (Niveau monitoring)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_HEALTH
    value: "DEBUG"
  - name: MANAGEMENT_ENDPOINTS_WEB_EXPOSURE_INCLUDE
    value: "health,info,metrics,env"
  - name: MANAGEMENT_ENDPOINT_HEALTH_SHOW_DETAILS
    value: "always"
  - name: MANAGEMENT_ENDPOINT_HEALTH_SHOW_COMPONENTS
    value: "true"
  - name: MANAGEMENT_HEALTH_DB_ENABLED
    value: "true"

  # ============================================
  # Couche 11 : JVM (Niveau machine virtuelle)
  # ============================================
  - name: JAVA_TOOL_OPTIONS
    value: "-Djava.util.logging.config.file=/path/to/logging.properties"
  - name: JAVA_OPTS
    value: "-Djava.io.tmpdir=/tmp -XX:+TraceClassLoading"
```

---

## Variables d'environnement par couches

### Couche 1 : Spring Boot (Niveau application)

#### `LOGGING_LEVEL_ROOT` / `logging.level.root`
Définit le niveau de journalisation racine pour toute l'application.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Définit le niveau de logging par défaut pour toute l'application
- Tous les loggers qui n'ont pas de niveau spécifique hériteront de ce niveau

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ROOT
    value: "DEBUG"
```

#### `SPRING_PROFILES_ACTIVE` / `spring.profiles.active`
Spécifie quels profils Spring sont actifs.

**Valeurs possibles :**
- `dev` - Profil de développement
- `test` - Profil de test
- `prod` - Profil de production
- `debug` - Profil de débogage
- Profils multiples : `dev,debug` ou `dev,test,prod`
- Tout nom de profil personnalisé

**Usage :**
- Active les profils Spring pour charger les configurations spécifiques à un environnement
- Permet de séparer les configurations dev/test/prod

**Exemple :**
```yaml
env:
  - name: SPRING_PROFILES_ACTIVE
    value: "dev,debug"
```

#### `SPRING_OUTPUT_ANSI_ENABLED` / `spring.output.ansi.enabled`
Active la sortie couleur ANSI dans la console.

**Valeurs possibles :**
- `always` - Active toujours les couleurs ANSI
- `never` - Désactive toujours les couleurs ANSI
- `detect` - Détecte selon les capacités du terminal (par défaut)

**Usage :**
- Améliore la lisibilité des logs en activant les couleurs ANSI dans la console
- Utile pour le développement local

**Exemple :**
```yaml
env:
  - name: SPRING_OUTPUT_ANSI_ENABLED
    value: "always"
```

---

### Couche 2 : Spring Framework (Niveau framework)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK` / `logging.level.org.springframework`
Définit le niveau de journalisation du framework Spring.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer toutes les opérations du framework Spring
- Utile pour comprendre le cycle de vie de l'application
- Trace le chargement des beans et les interactions internes

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB` / `logging.level.org.springframework.web`
Définit le niveau de journalisation de Spring Web.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les requêtes HTTP
- Trace le routage et les contrôleurs
- Trace toutes les opérations liées à Spring MVC/WebFlux

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CONTEXT` / `logging.level.org.springframework.context`
Définit le niveau de journalisation du contexte Spring (chargement de fichiers de configuration, beans, etc.).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer le chargement du contexte Spring
- Trace l'initialisation des beans
- Trace le chargement des fichiers de configuration
- Trace les événements du cycle de vie

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CONTEXT
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BEANS` / `logging.level.org.springframework.beans`
Définit le niveau de journalisation des beans Spring.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer la création des beans Spring
- Trace l'injection de dépendances
- Trace le cycle de vie des beans
- Utile pour déboguer les problèmes d'injection

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BEANS
    value: "DEBUG"
```

---

### Couche 3 : Vault & Cloud Config (Niveau configuration externe)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG` / `logging.level.org.springframework.cloud.config`
Définit le niveau de journalisation pour Spring Cloud Config (client).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :** Permet de tracer toutes les opérations Cloud Config :
- Connexion au serveur Config
- Récupération des configurations
- Erreurs de connexion ou de parsing

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_CLIENT` / `logging.level.org.springframework.cloud.config.client`
Définit le niveau de journalisation pour le client Spring Cloud Config.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :** Permet de déboguer spécifiquement le client Config :
- Requêtes HTTP vers le serveur Config
- Gestion des profils et labels
- Retry et fail-fast

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_CLIENT
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_SERVER` / `logging.level.org.springframework.cloud.config.server`
Définit le niveau de journalisation pour le serveur Spring Cloud Config.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du serveur Config (si vous utilisez un serveur Config dans votre application)
- Utile pour déboguer les problèmes de serveur

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_SERVER
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_BOOTSTRAP` / `logging.level.org.springframework.cloud.bootstrap`
Définit le niveau de journalisation pour le bootstrap Spring Cloud (chargement initial avant le contexte principal).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :** Très utile pour comprendre :
- Chargement avant le contexte principal
- Erreurs précoces Vault / Config
- Ordre d'initialisation des PropertySources
- Problèmes de connexion au démarrage

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_BOOTSTRAP
    value: "DEBUG"
```

#### `SPRING_CLOUD_CONFIG_URI` / `spring.cloud.config.uri`
Définit l'URI du serveur Spring Cloud Config.

**Valeurs possibles :**
- URL complète du serveur Config (ex. `http://config-server:8888`)
- URL avec protocole HTTPS si SSL activé

**Usage :**
- Configure l'URL du serveur Cloud Config
- Nécessaire pour que l'application puisse se connecter au serveur de configuration

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_CONFIG_URI
    value: "http://config-server:8888"
```

#### `SPRING_CLOUD_CONFIG_NAME` / `spring.cloud.config.name`
Définit le nom de l'application pour Spring Cloud Config.

**Valeurs possibles :**
- Nom de l'application (ex. `myapp`)
- Nom par défaut : `application`

**Usage :**
- Spécifie le nom de l'application utilisé pour récupérer la configuration depuis le serveur Config
- Le serveur cherchera les fichiers `{name}-{profile}.yml` ou `{name}-{profile}.properties`

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_CONFIG_NAME
    value: "myapp"
```

#### `SPRING_CLOUD_CONFIG_PROFILE` / `spring.cloud.config.profile`
Définit le profil pour Spring Cloud Config.

**Valeurs possibles :**
- `dev` - Profil développement
- `test` - Profil test
- `prod` - Profil production
- Profils multiples séparés par des virgules

**Usage :**
- Spécifie le profil utilisé pour récupérer la configuration
- Permet de charger des configurations différentes selon l'environnement

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_CONFIG_PROFILE
    value: "dev"
```

#### `SPRING_CLOUD_CONFIG_LABEL` / `spring.cloud.config.label`
Définit la branche/label pour Spring Cloud Config (Git).

**Valeurs possibles :**
- Nom de branche Git (ex. `main`, `master`, `develop`)
- Par défaut : `master`

**Usage :**
- Spécifie la branche Git à utiliser pour récupérer la configuration
- Utile pour tester des configurations depuis différentes branches

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_CONFIG_LABEL
    value: "main"
```

#### `SPRING_CLOUD_CONFIG_FAIL_FAST` / `spring.cloud.config.fail-fast`
Définit si l'application doit échouer au démarrage si le serveur Config n'est pas disponible.

**Valeurs possibles :**
- `true` - Échoue si le serveur Config n'est pas disponible
- `false` - Continue même si le serveur Config n'est pas disponible (par défaut)

**Usage :**
- Force l'application à échouer immédiatement si le serveur Config n'est pas accessible
- Utile pour détecter rapidement les problèmes de connexion en production

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_CONFIG_FAIL_FAST
    value: "true"
```

#### `SPRING_CLOUD_CONFIG_RETRY_INITIAL_INTERVAL` / `spring.cloud.config.retry.initial-interval`
Définit l'intervalle initial pour les tentatives de reconnexion au serveur Config (en millisecondes).

**Valeurs possibles :**
- Nombre en millisecondes (ex. `1000` pour 1 seconde)
- Par défaut : `1000`

**Usage :**
- Configure le délai initial entre les tentatives de reconnexion au serveur Config
- Utile pour gérer les problèmes de réseau temporaires

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_CONFIG_RETRY_INITIAL_INTERVAL
    value: "1000"
```

#### `SPRING_CLOUD_CONFIG_RETRY_MAX_ATTEMPTS` / `spring.cloud.config.retry.max-attempts`
Définit le nombre maximum de tentatives de reconnexion au serveur Config.

**Valeurs possibles :**
- Nombre entier (ex. `6`)
- Par défaut : `6`

**Usage :**
- Définit combien de fois l'application doit essayer de se connecter au serveur Config avant d'abandonner
- Utile pour gérer les problèmes de réseau temporaires

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_CONFIG_RETRY_MAX_ATTEMPTS
    value: "6"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT` / `logging.level.org.springframework.vault`
Définit le niveau de journalisation pour Spring Vault (HashiCorp Vault).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :** Permet de tracer toutes les opérations Vault :
- Connexion au serveur Vault
- Authentification (token, AppRole, Kubernetes, etc.)
- Récupération des secrets depuis les backends KV
- Erreurs d'authentification ou de connexion

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_CORE` / `logging.level.org.springframework.vault.core`
Définit le niveau de journalisation pour le cœur de Spring Vault.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations de base de Vault
- Trace la lecture/écriture des secrets
- Trace les opérations sur les backends KV

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_CORE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_AUTHENTICATION` / `logging.level.org.springframework.vault.authentication`
Définit le niveau de journalisation pour l'authentification Vault.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer le processus d'authentification Vault (token, AppRole, Kubernetes, etc.)
- Utile pour déboguer les problèmes d'authentification

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_AUTHENTICATION
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_LEASE` / `logging.level.org.springframework.vault.lease`
Définit le niveau de journalisation pour la gestion des leases Vault.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Définition:** c’est quoi un Vault lease ?

Un lease Vault c'est une durée de vie associée à un secret dynamique ou renouvelable
(ex : DB credentials, KV v2 avec lease, tokens, etc.)

Exemples :
- lease_duration = 3600
- renewable = true

Spring Cloud Vault :
- Attache le lease
- Le renouvelle automatiquement
- Réagit s’il expire

**Usage :**
- Permet de tracer la gestion des leases Vault (renouvellement, expiration)
- Trace le cycle de vie des leases et leur renouvellement automatique
- Utile pour déboguer les problèmes de renouvellement de leases et d'expiration de secrets
- Important pour les secrets dynamiques qui nécessitent un renouvellement périodique

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_LEASE
    value: "DEBUG"
```

#### `SPRING_CLOUD_VAULT_URI` / `spring.cloud.vault.uri`
Définit l'URI du serveur HashiCorp Vault.

**Valeurs possibles :**
- URL complète du serveur Vault (ex. `http://vault:8200`)
- URL avec protocole HTTPS si SSL activé

**Usage :**
- Configure l'URL du serveur Vault
- Nécessaire pour que l'application puisse se connecter à Vault et récupérer les secrets

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_VAULT_URI
    value: "http://vault:8200"
```

#### `SPRING_CLOUD_VAULT_AUTHENTICATION` / `spring.cloud.vault.authentication`
Définit la méthode d'authentification pour Vault.

**Valeurs possibles :**
- `TOKEN` - Authentification par token
- `APPROLE` - Authentification par AppRole
- `AWS_EC2` - Authentification AWS EC2
- `AWS_IAM` - Authentification AWS IAM
- `KUBERNETES` - Authentification Kubernetes
- `LDAP` - Authentification LDAP
- `CERT` - Authentification par certificat

**Usage :**
- Spécifie la méthode d'authentification à utiliser pour se connecter à Vault
- Chaque méthode nécessite des paramètres spécifiques (token, role-id/secret-id, etc.)

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_VAULT_AUTHENTICATION
    value: "TOKEN"
```

#### `SPRING_CLOUD_VAULT_TOKEN` / `spring.cloud.vault.token`
Définit le token d'authentification Vault (pour l'authentification TOKEN).

**Valeurs possibles :**
- Token Vault valide
- Variable d'environnement ou secret Kubernetes recommandé

**Usage :**
- Fournit le token Vault pour l'authentification
- Utilisez des secrets Kubernetes pour stocker ce token de manière sécurisée

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_VAULT_TOKEN
    valueFrom:
      secretKeyRef:
        name: vault-token
        key: token
```

#### `SPRING_CLOUD_VAULT_APPROLE_ROLE_ID` / `spring.cloud.vault.app-role.role-id`
Définit le Role ID pour l'authentification AppRole Vault.

**Valeurs possibles :**
- Role ID Vault valide

**Usage :**
- Fournit le Role ID pour l'authentification AppRole
- Utilisé avec le Secret ID pour s'authentifier auprès de Vault

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_VAULT_APPROLE_ROLE_ID
    valueFrom:
      secretKeyRef:
        name: vault-approle
        key: role-id
```

#### `SPRING_CLOUD_VAULT_APPROLE_SECRET_ID` / `spring.cloud.vault.app-role.secret-id`
Définit le Secret ID pour l'authentification AppRole Vault.

**Valeurs possibles :**
- Secret ID Vault valide

**Usage :**
- Fournit le Secret ID pour l'authentification AppRole
- Utilisé avec le Role ID
- Stockez ce secret de manière sécurisée (secrets Kubernetes)

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_VAULT_APPROLE_SECRET_ID
    valueFrom:
      secretKeyRef:
        name: vault-approle
        key: secret-id
```

#### `SPRING_CLOUD_VAULT_KV_ENABLED` / `spring.cloud.vault.kv.enabled`
Active ou désactive le support Key-Value (KV) pour Vault.

**Valeurs possibles :**
- `true` - Active le support KV (par défaut)
- `false` - Désactive le support KV

**Usage :**
- Active ou désactive la récupération des secrets depuis les backends KV de Vault
- Désactivez si vous n'utilisez pas de backends KV

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_VAULT_KV_ENABLED
    value: "true"
```

#### `SPRING_CLOUD_VAULT_KV_BACKEND` / `spring.cloud.vault.kv.backend`
Définit le backend KV à utiliser dans Vault.

**Valeurs possibles :**
- `secret` - Backend secret par défaut
- Nom personnalisé du backend KV

**Usage :**
- Spécifie quel backend KV Vault utiliser pour récupérer les secrets
- Par défaut, utilise le backend `secret`

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_VAULT_KV_BACKEND
    value: "secret"
```

#### `SPRING_CLOUD_VAULT_KV_APPLICATION_NAME` / `spring.cloud.vault.kv.application-name`
Définit le nom de l'application pour le backend KV Vault.

**Valeurs possibles :**
- Nom de l'application (ex. `myapp`)
- Par défaut : nom de l'application Spring

**Usage :**
- Spécifie le nom de l'application utilisé pour construire le chemin dans Vault (ex. `secret/{application-name}`)
- Par défaut, utilise le nom de l'application Spring

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_VAULT_KV_APPLICATION_NAME
    value: "myapp"
```

#### `SPRING_CLOUD_VAULT_SSL_ENABLED` / `spring.cloud.vault.ssl.enabled`
Active ou désactive SSL pour les connexions Vault.

**Valeurs possibles :**
- `true` - Active SSL (recommandé en production)
- `false` - Désactive SSL (par défaut)

**Usage :**
- Active le chiffrement SSL/TLS pour les connexions à Vault
- Obligatoire en production pour sécuriser les communications

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_VAULT_SSL_ENABLED
    value: "true"
```

#### `SPRING_CLOUD_VAULT_FAIL_FAST` / `spring.cloud.vault.fail-fast`
Définit si l'application doit échouer au démarrage si Vault n'est pas disponible.

**Valeurs possibles :**
- `true` - Échoue si Vault n'est pas disponible
- `false` - Continue même si Vault n'est pas disponible (par défaut)

**Usage :**
- Force l'application à échouer immédiatement si Vault n'est pas accessible
- Utile pour détecter rapidement les problèmes de connexion en production

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_VAULT_FAIL_FAST
    value: "true"
```

---

### Couche 4 : Java I/O (Niveau Java - Opérations de fichiers)

#### `LOGGING_LEVEL_JAVA_IO` / `logging.level.java.io`
Définit le niveau de journalisation pour les opérations I/O Java (lecture/écriture de fichiers).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer toutes les opérations I/O Java classiques
- Trace les opérations de lecture/écriture de fichiers
- Utile pour déboguer les problèmes d'accès aux fichiers

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVA_IO
    value: "DEBUG"
```

#### `LOGGING_LEVEL_JAVA_NIO` / `logging.level.java.nio`
Définit le niveau de journalisation pour les opérations NIO (New I/O) Java.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations NIO (New I/O) Java
- Trace les opérations asynchrones et les buffers
- Utile pour déboguer les problèmes de performance I/O

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVA_NIO
    value: "DEBUG"
```

#### `LOGGING_LEVEL_JAVA_NIO_FILE` / `logging.level.java.nio.file`
Définit le niveau de journalisation pour les opérations sur les fichiers via NIO (Files.read(), Files.write(), etc.).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations sur les fichiers via l'API NIO Files
- Trace les opérations Files.read(), Files.write(), Files.copy(), etc.
- Utile pour déboguer les problèmes de manipulation de fichiers

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVA_NIO_FILE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_JAVA_IO_FILEINPUTSTREAM` / `logging.level.java.io.FileInputStream`
Définit le niveau de journalisation pour FileInputStream (lecture de fichiers).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer spécifiquement les opérations de lecture de fichiers via FileInputStream
- Trace l'ouverture, la lecture et la fermeture des fichiers
- Utile pour déboguer les problèmes de lecture de fichiers

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVA_IO_FILEINPUTSTREAM
    value: "DEBUG"
```

#### `LOGGING_LEVEL_JAVA_IO_FILEOUTPUTSTREAM` / `logging.level.java.io.FileOutputStream`
Définit le niveau de journalisation pour FileOutputStream (écriture de fichiers).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer spécifiquement les opérations d'écriture de fichiers via FileOutputStream
- Trace l'ouverture, l'écriture et la fermeture des fichiers
- Utile pour déboguer les problèmes d'écriture de fichiers

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVA_IO_FILEOUTPUTSTREAM
    value: "DEBUG"
```

---

### Couche 5 : S3 / Cloud Storage (Niveau stockage cloud)

#### `LOGGING_LEVEL_COM_AMAZONAWS` / `logging.level.com.amazonaws`
Définit le niveau de journalisation pour AWS SDK (opérations S3, etc.).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer toutes les opérations AWS SDK
- Trace les requêtes vers les services AWS (S3, SQS, SNS, etc.)
- Utile pour déboguer les problèmes de connexion et d'authentification AWS

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_AMAZONAWS
    value: "DEBUG"
```

#### `LOGGING_LEVEL_COM_AMAZONAWS_SERVICES_S3` / `logging.level.com.amazonaws.services.s3`
Définit le niveau de journalisation pour les opérations S3 spécifiques.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer spécifiquement les opérations S3
- Trace les opérations PUT, GET, DELETE, LIST sur les buckets et objets
- Utile pour déboguer les problèmes d'accès aux buckets S3

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_AMAZONAWS_SERVICES_S3
    value: "DEBUG"
```

#### `LOGGING_LEVEL_COM_AMAZONAWS_AUTH` / `logging.level.com.amazonaws.auth`
Définit le niveau de journalisation pour l'authentification AWS.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer le processus d'authentification AWS
- Trace la signature des requêtes et la gestion des credentials
- Utile pour déboguer les problèmes d'authentification AWS

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_AMAZONAWS_AUTH
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_HTTP` / `logging.level.org.apache.http`
Définit le niveau de journalisation pour Apache HTTP Client (utilisé par AWS SDK).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les requêtes HTTP effectuées par Apache HTTP Client
- Utilisé par AWS SDK pour les requêtes vers les services AWS
- Utile pour voir les requêtes HTTP brutes vers S3

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_HTTP
    value: "DEBUG"
```

#### `AWS_ACCESS_KEY_ID` / `aws.access-key-id`
Définit la clé d'accès AWS pour l'authentification S3.

**Valeurs possibles :**
- Clé d'accès AWS valide
- Variable d'environnement ou secret Kubernetes recommandé

**Usage :**
- Fournit la clé d'accès AWS pour l'authentification
- Nécessaire pour accéder aux buckets S3
- Utilisez des secrets Kubernetes pour stocker cette valeur de manière sécurisée

**Exemple :**
```yaml
env:
  - name: AWS_ACCESS_KEY_ID
    valueFrom:
      secretKeyRef:
        name: aws-credentials
        key: access-key-id
```

#### `AWS_SECRET_ACCESS_KEY` / `aws.secret-access-key`
Définit la clé secrète AWS pour l'authentification S3.

**Valeurs possibles :**
- Clé secrète AWS valide
- Variable d'environnement ou secret Kubernetes recommandé

**Usage :**
- Fournit la clé secrète AWS pour l'authentification
- Utilisée avec AWS_ACCESS_KEY_ID pour s'authentifier
- Utilisez des secrets Kubernetes pour stocker cette valeur de manière sécurisée

**Exemple :**
```yaml
env:
  - name: AWS_SECRET_ACCESS_KEY
    valueFrom:
      secretKeyRef:
        name: aws-credentials
        key: secret-access-key
```

#### `AWS_REGION` / `aws.region`
Définit la région AWS pour les opérations S3.

**Valeurs possibles :**
- Nom de région AWS (ex. `us-east-1`, `eu-west-1`, `ap-southeast-1`)

**Usage :**
- Spécifie la région AWS où se trouvent vos buckets S3
- Nécessaire pour que l'application sache quelle région utiliser
- Impacte la latence et les coûts

**Exemple :**
```yaml
env:
  - name: AWS_REGION
    value: "us-east-1"
```

#### `AWS_S3_BUCKET` / `aws.s3.bucket`
Définit le nom du bucket S3 à utiliser.

**Valeurs possibles :**
- Nom de bucket S3 valide

**Usage :**
- Spécifie le nom du bucket S3 par défaut à utiliser
- Utilisé par Spring Cloud AWS pour les opérations S3
- Peut être surchargé dans le code si nécessaire

**Exemple :**
```yaml
env:
  - name: AWS_S3_BUCKET
    value: "my-bucket-name"
```

#### `AWS_S3_ENDPOINT` / `aws.s3.endpoint`
Définit l'endpoint personnalisé pour S3 (utile pour S3-compatible comme MinIO).

**Valeurs possibles :**
- URL complète de l'endpoint (ex. `http://minio:9000`)
- Vide pour utiliser l'endpoint AWS par défaut

**Usage :**
- Permet d'utiliser un endpoint S3-compatible (MinIO, LocalStack, etc.)
- Utile pour le développement local ou les environnements de test
- Remplace l'endpoint AWS par défaut

**Exemple :**
```yaml
env:
  - name: AWS_S3_ENDPOINT
    value: "http://minio:9000"
```

#### `AWS_S3_PATH_STYLE_ACCESS` / `aws.s3.path-style-access`
Active l'accès style path pour S3 (nécessaire pour certains endpoints S3-compatibles).

**Valeurs possibles :**
- `true` - Active l'accès style path
- `false` - Utilise l'accès style virtual-hosted (par défaut)

**Usage :**
- Active l'accès style path pour S3 (ex. `http://endpoint/bucket/key`)
- Nécessaire pour certains endpoints S3-compatibles comme MinIO
- Par défaut, AWS utilise le style virtual-hosted (ex. `http://bucket.endpoint/key`)

**Exemple :**
```yaml
env:
  - name: AWS_S3_PATH_STYLE_ACCESS
    value: "true"
```

#### `SPRING_CLOUD_AWS_S3_ENABLED` / `spring.cloud.aws.s3.enabled`
Active ou désactive le support Spring Cloud AWS S3.

**Valeurs possibles :**
- `true` - Active le support S3 (par défaut si dépendance présente)
- `false` - Désactive le support S3

**Usage :**
- Active ou désactive l'intégration Spring Cloud AWS S3
- Utile pour désactiver S3 si vous n'utilisez pas cette fonctionnalité
- Par défaut activé si la dépendance Spring Cloud AWS est présente

**Exemple :**
```yaml
env:
  - name: SPRING_CLOUD_AWS_S3_ENABLED
    value: "true"
```

---

### Couche 6 : SSL/TLS (Niveau sécurité)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_SSL` / `logging.level.org.springframework.boot.ssl`
Définit le niveau de journalisation pour SSL/TLS dans Spring Boot.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations SSL/TLS dans Spring Boot
- Trace le chargement des certificats et la configuration SSL
- Utile pour déboguer les problèmes de certificats et de configuration HTTPS

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_SSL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_JAVAX_NET_SSL` / `logging.level.javax.net.ssl`
Définit le niveau de journalisation pour les opérations SSL/TLS au niveau Java (javax.net.ssl).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations SSL/TLS au niveau Java standard (javax.net.ssl)
- Trace les handshakes SSL, les négociations de protocole et les erreurs de certificat
- Utile pour déboguer les problèmes de connexion SSL/TLS

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVAX_NET_SSL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_SUN_SECURITY_SSL` / `logging.level.sun.security.ssl`
Définit le niveau de journalisation pour les opérations SSL/TLS au niveau Sun Security (implémentation Java).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations SSL/TLS au niveau de l'implémentation Sun Security
- Trace les détails internes des handshakes SSL et des opérations de chiffrement
- Utile pour un débogage approfondi des problèmes SSL/TLS

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_SUN_SECURITY_SSL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_TOMCAT_UTIL_NET_SSL` / `logging.level.org.apache.tomcat.util.net.ssl`
Définit le niveau de journalisation pour SSL/TLS dans Tomcat (si utilisé comme serveur embarqué).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations SSL/TLS dans Tomcat
- Trace la configuration HTTPS, le chargement des certificats et les connexions SSL
- Utile pour déboguer les problèmes de configuration HTTPS dans Tomcat

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_TOMCAT_UTIL_NET_SSL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_CATALINA_LOADER` / `logging.level.org.apache.catalina.loader`
Définit le niveau de journalisation pour le chargement SSL dans Tomcat.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer le chargement des classes et ressources SSL dans Tomcat
- Trace le chargement des certificats et des keystores
- Utile pour déboguer les problèmes de chargement de certificats SSL

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_CATALINA_LOADER
    value: "DEBUG"
```

---

### Couche 7 : Database (Niveau base de données)

#### Spring JDBC

##### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC` / `logging.level.org.springframework.jdbc`
Définit le niveau de journalisation pour Spring JDBC (accès aux bases de données).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer toutes les opérations Spring JDBC
- Trace les requêtes SQL, les résultats et les erreurs
- Utile pour déboguer les problèmes d'accès aux bases de données

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC_CORE` / `logging.level.org.springframework.jdbc.core`
Définit le niveau de journalisation pour le cœur JDBC de Spring (exécution des requêtes SQL).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer l'exécution des requêtes SQL via Spring JDBC
- Trace les requêtes préparées, les paramètres et les résultats
- Utile pour déboguer les problèmes d'exécution de requêtes SQL

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC_CORE
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC_DATASOURCE` / `logging.level.org.springframework.jdbc.datasource`
Définit le niveau de journalisation pour les sources de données Spring JDBC.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations sur les sources de données Spring JDBC
- Trace la création, la configuration et la gestion des connexions
- Utile pour déboguer les problèmes de pool de connexions

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC_DATASOURCE
    value: "DEBUG"
```

#### Hibernate (ORM)

##### `LOGGING_LEVEL_ORG_HIBERNATE` / `logging.level.org.hibernate`
Définit le niveau de journalisation pour Hibernate (ORM).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer toutes les opérations Hibernate
- Trace les sessions, les transactions, le cache et les requêtes
- Utile pour déboguer les problèmes d'ORM et de mapping

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_HIBERNATE
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_HIBERNATE_SQL` / `logging.level.org.hibernate.SQL`
Définit le niveau de journalisation pour les requêtes SQL générées par Hibernate.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de voir toutes les requêtes SQL générées par Hibernate
- Trace les requêtes SELECT, INSERT, UPDATE, DELETE
- Utile pour optimiser les requêtes et déboguer les problèmes de performance

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_HIBERNATE_SQL
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_HIBERNATE_TYPE_DESCRIPTOR_SQL` / `logging.level.org.hibernate.type.descriptor.sql.BasicBinder`
Définit le niveau de journalisation pour les paramètres SQL bindés par Hibernate.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de voir les valeurs des paramètres bindés dans les requêtes SQL
- Trace les paramètres des requêtes préparées avec leurs valeurs réelles
- Utile pour déboguer les problèmes de paramètres et de types

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_HIBERNATE_TYPE_DESCRIPTOR_SQL
    value: "TRACE"
```

#### Pool de connexions

##### `LOGGING_LEVEL_COM_ZAXXER_HIKARI` / `logging.level.com.zaxxer.hikari`
Définit le niveau de journalisation pour HikariCP (pool de connexions).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du pool de connexions HikariCP
- Trace l'obtention, la libération et la gestion des connexions
- Utile pour déboguer les problèmes de pool (timeout, fuites de connexions)

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_ZAXXER_HIKARI
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_APACHE_TOMCAT_JDBC_POOL` / `logging.level.org.apache.tomcat.jdbc.pool`
Définit le niveau de journalisation pour Tomcat JDBC Pool (si utilisé).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du pool de connexions Tomcat JDBC
- Trace l'obtention, la libération et la gestion des connexions
- Utile pour déboguer les problèmes de pool de connexions Tomcat

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_TOMCAT_JDBC_POOL
    value: "DEBUG"
```

#### Drivers de base de données

##### `LOGGING_LEVEL_COM_MYSQL_CJ` / `logging.level.com.mysql.cj`
Définit le niveau de journalisation pour le driver MySQL Connector/J.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du driver MySQL
- Trace les connexions, les requêtes et les erreurs MySQL
- Utile pour déboguer les problèmes de connexion et de requêtes MySQL

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_MYSQL_CJ
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_POSTGRESQL` / `logging.level.org.postgresql`
Définit le niveau de journalisation pour le driver PostgreSQL.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du driver PostgreSQL
- Trace les connexions, les requêtes et les erreurs PostgreSQL
- Utile pour déboguer les problèmes de connexion et de requêtes PostgreSQL

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_POSTGRESQL
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORACLE_JDBC` / `logging.level.oracle.jdbc`
Définit le niveau de journalisation pour le driver Oracle JDBC.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du driver Oracle JDBC
- Trace les connexions, les requêtes et les erreurs Oracle
- Utile pour déboguer les problèmes de connexion et de requêtes Oracle

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORACLE_JDBC
    value: "DEBUG"
```

##### `LOGGING_LEVEL_COM_TERADATA` / `logging.level.com.teradata`
Définit le niveau de journalisation pour le driver Teradata JDBC.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du driver Teradata JDBC
- Trace les connexions, les requêtes SQL et les erreurs Teradata
- Utile pour déboguer les problèmes de connexion et de requêtes Teradata
- Trace les opérations sur les entrepôts de données Teradata

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_TERADATA
    value: "DEBUG"
```

##### `LOGGING_LEVEL_COM_TERADATA_JDBC` / `logging.level.com.teradata.jdbc`
Définit le niveau de journalisation pour le driver Teradata JDBC (package spécifique).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer spécifiquement les opérations du package JDBC Teradata
- Trace les connexions, les statements et les résultats de requêtes
- Utile pour un débogage plus précis des opérations JDBC Teradata

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_TERADATA_JDBC
    value: "DEBUG"
```

#### Bases de données NoSQL

##### `LOGGING_LEVEL_ORG_ELASTICSEARCH` / `logging.level.org.elasticsearch`
Définit le niveau de journalisation pour Elasticsearch.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Elasticsearch
- Trace les requêtes de recherche et d'indexation
- Utile pour déboguer les problèmes de connexion et de requêtes

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_ELASTICSEARCH
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_ELASTICSEARCH` / `logging.level.org.springframework.data.elasticsearch`
Définit le niveau de journalisation pour Spring Data Elasticsearch.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Data Elasticsearch
- Trace les interactions avec les repositories Elasticsearch
- Utile pour déboguer les problèmes de mapping et de requêtes

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_ELASTICSEARCH
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_MONGODB` / `logging.level.org.mongodb`
Définit le niveau de journalisation pour MongoDB Driver.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations MongoDB
- Trace les requêtes, insertions, mises à jour et suppressions
- Utile pour déboguer les problèmes de connexion et de performance

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_MONGODB
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_MONGODB_DRIVER` / `logging.level.org.mongodb.driver`
Définit le niveau de journalisation pour le driver MongoDB.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du driver MongoDB
- Trace les connexions, les requêtes et les opérations réseau
- Utile pour déboguer les problèmes de connexion et de réplication

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_MONGODB_DRIVER
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_MONGODB` / `logging.level.org.springframework.data.mongodb`
Définit le niveau de journalisation pour Spring Data MongoDB.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Data MongoDB
- Trace les interactions avec les repositories MongoDB
- Utile pour déboguer les problèmes de mapping et de requêtes

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_DATA_MONGODB
    value: "DEBUG"
```

---

### Couche 8 : MQ (Message Queue)

#### Kafka

##### `LOGGING_LEVEL_ORG_APACHE_KAFKA` / `logging.level.org.apache.kafka`
Définit le niveau de journalisation pour Apache Kafka.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Kafka
- Trace les connexions aux brokers, la production et la consommation de messages
- Utile pour déboguer les problèmes de connexion et de performance

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_KAFKA
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_APACHE_KAFKA_CLIENTS` / `logging.level.org.apache.kafka.clients`
Définit le niveau de journalisation pour les clients Kafka.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations des clients Kafka (producers et consumers)
- Trace les envois et réceptions de messages
- Utile pour déboguer les problèmes de sérialisation et de désérialisation

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_KAFKA_CLIENTS
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_KAFKA` / `logging.level.org.springframework.kafka`
Définit le niveau de journalisation pour Spring Kafka.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring Kafka
- Trace les listeners, les templates Kafka et les transactions
- Utile pour déboguer les problèmes de configuration et de gestion des messages

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_KAFKA
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_KAFKA_LISTENER` / `logging.level.org.springframework.kafka.listener`
Définit le niveau de journalisation pour les listeners Kafka.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations des listeners Kafka
- Trace la réception et le traitement des messages
- Utile pour déboguer les problèmes de consommation de messages

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_KAFKA_LISTENER
    value: "DEBUG"
```

#### RabbitMQ

##### `LOGGING_LEVEL_COM_RABBITMQ` / `logging.level.com.rabbitmq`
Définit le niveau de journalisation pour RabbitMQ Client.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations RabbitMQ
- Trace les connexions, les canaux, les échanges et les files d'attente
- Utile pour déboguer les problèmes de connexion et de routage

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_RABBITMQ
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_AMQP` / `logging.level.org.springframework.amqp`
Définit le niveau de journalisation pour Spring AMQP (RabbitMQ).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations Spring AMQP
- Trace les templates, les listeners et les conversions de messages
- Utile pour déboguer les problèmes de configuration et de gestion des messages

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_AMQP
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_AMQP_RABBIT` / `logging.level.org.springframework.amqp.rabbit`
Définit le niveau de journalisation pour Spring AMQP RabbitMQ.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations spécifiques à RabbitMQ dans Spring AMQP
- Trace les connexions, les listeners et les transactions
- Utile pour déboguer les problèmes de connexion et de gestion des messages

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_AMQP_RABBIT
    value: "DEBUG"
```

##### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_AMQP_RABBIT_LISTENER` / `logging.level.org.springframework.amqp.rabbit.listener`
Définit le niveau de journalisation pour les listeners RabbitMQ.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations des listeners RabbitMQ
- Trace la réception et le traitement des messages depuis les files d'attente
- Utile pour déboguer les problèmes de consommation de messages

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_AMQP_RABBIT_LISTENER
    value: "DEBUG"
```

#### IBM MQ

##### `LOGGING_LEVEL_COM_IBM_MQ` / `logging.level.com.ibm.mq`
Définit le niveau de journalisation pour IBM MQ.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations IBM MQ
- Trace les connexions aux gestionnaires de files d'attente, les opérations PUT/GET
- Utile pour déboguer les problèmes de connexion et de gestion des messages

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_IBM_MQ
    value: "DEBUG"
```

##### `LOGGING_LEVEL_COM_IBM_MQJMS` / `logging.level.com.ibm.mqjms`
Définit le niveau de journalisation pour IBM MQ JMS.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations JMS avec IBM MQ
- Trace les sessions JMS, les producteurs et les consommateurs
- Utile pour déboguer les problèmes de connexion JMS et de gestion des messages

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_IBM_MQJMS
    value: "DEBUG"
```

##### `LOGGING_LEVEL_COM_IBM_MSGCLIENT` / `logging.level.com.ibm.msgclient`
Définit le niveau de journalisation pour IBM MQ Client.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du client IBM MQ
- Trace les connexions, les canaux et les opérations de file d'attente
- Utile pour déboguer les problèmes de connexion et de communication

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_IBM_MSGCLIENT
    value: "DEBUG"
```

##### `LOGGING_LEVEL_COM_IBM_MSGCLIENT_JMS` / `logging.level.com.ibm.msgclient.jms`
Définit le niveau de journalisation pour IBM MQ JMS Client.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations JMS du client IBM MQ
- Trace les connexions JMS, les factories et les opérations de message
- Utile pour déboguer les problèmes de configuration JMS

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_IBM_MSGCLIENT_JMS
    value: "DEBUG"
```

---

### Couche 9 : HTTP/Web (Niveau réseau)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB` / `logging.level.org.springframework.web`
Définit le niveau de journalisation pour Spring Web (contrôleurs, requêtes HTTP).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer toutes les requêtes HTTP entrantes et sortantes
- Trace le routage vers les contrôleurs et les méthodes handler
- Trace les opérations Spring MVC et WebFlux
- Utile pour déboguer les problèmes de routage et de mapping des requêtes

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_SERVLET` / `logging.level.org.springframework.web.servlet`
Définit le niveau de journalisation pour Spring MVC Servlet.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer spécifiquement les opérations Spring MVC Servlet
- Trace le traitement des requêtes HTTP via DispatcherServlet
- Trace la résolution des vues et la génération des réponses
- Utile pour déboguer les problèmes de traitement des requêtes MVC

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_SERVLET
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_FILTER` / `logging.level.org.springframework.web.filter`
Définit le niveau de journalisation pour les filtres Spring Web.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer l'exécution des filtres Spring Web
- Trace l'ordre d'exécution des filtres et leur chaînage
- Utile pour déboguer les problèmes de filtrage des requêtes

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_FILTER
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_CATALINA` / `logging.level.org.apache.catalina`
Définit le niveau de journalisation pour Tomcat Catalina (serveur embarqué).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations internes de Tomcat Catalina
- Trace le cycle de vie du serveur, les contextes et les applications
- Utile pour déboguer les problèmes de démarrage et de configuration Tomcat

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_CATALINA
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_CATALINA_CONNECTOR` / `logging.level.org.apache.catalina.connector`
Définit le niveau de journalisation pour les connecteurs Tomcat (HTTP/HTTPS).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations des connecteurs Tomcat
- Trace les connexions HTTP/HTTPS, les sockets et les threads
- Utile pour déboguer les problèmes de connexions réseau et de performance

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_CATALINA_CONNECTOR
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_CATALINA_CORE` / `logging.level.org.apache.catalina.core`
Définit le niveau de journalisation pour le cœur de Tomcat.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du cœur de Tomcat
- Trace les composants core, les valves et le pipeline de traitement
- Utile pour un débogage approfondi de Tomcat

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_CATALINA_CORE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_ECLIPSE_JETTY` / `logging.level.org.eclipse.jetty`
Définit le niveau de journalisation pour Jetty (si utilisé comme serveur embarqué).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations du serveur Jetty
- Trace les connexions, les handlers et le traitement des requêtes
- Utile pour déboguer les problèmes de configuration et de performance Jetty

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_ECLIPSE_JETTY
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_HTTP` / `logging.level.org.apache.http`
Définit le niveau de journalisation pour Apache HttpClient (si utilisé).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les requêtes HTTP effectuées par Apache HttpClient
- Trace les connexions, les requêtes et les réponses HTTP
- Utile pour voir les requêtes HTTP brutes vers les services externes

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_HTTP
    value: "DEBUG"
```

---

### Couche 10 : Security (Niveau sécurité Spring)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY` / `logging.level.org.springframework.security`
Définit le niveau de journalisation pour Spring Security.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer toutes les opérations Spring Security
- Trace l'authentification, l'autorisation et la gestion des sessions
- Utile pour déboguer les problèmes de sécurité et d'accès

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_WEB` / `logging.level.org.springframework.security.web`
Définit le niveau de journalisation pour Spring Security Web (filtres de sécurité).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les filtres de sécurité Spring Security
- Trace l'exécution de la chaîne de filtres de sécurité
- Utile pour déboguer les problèmes de filtrage et de protection des endpoints

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_WEB
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_AUTHENTICATION` / `logging.level.org.springframework.security.authentication`
Définit le niveau de journalisation pour l'authentification Spring Security.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer le processus d'authentification Spring Security
- Trace la validation des credentials et la création des tokens d'authentification
- Utile pour déboguer les problèmes d'authentification (login, tokens, etc.)

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_AUTHENTICATION
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_ACCESS` / `logging.level.org.springframework.security.access`
Définit le niveau de journalisation pour le contrôle d'accès Spring Security.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les décisions de contrôle d'accès Spring Security
- Trace l'évaluation des règles d'autorisation et des permissions
- Utile pour déboguer les problèmes d'autorisation et d'accès refusé

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_ACCESS
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_OAUTH2` / `logging.level.org.springframework.security.oauth2`
Définit le niveau de journalisation pour OAuth2 dans Spring Security.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations OAuth2 dans Spring Security
- Trace le flux OAuth2, la validation des tokens et l'échange de tokens
- Utile pour déboguer les problèmes d'authentification OAuth2 et de gestion des tokens

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_OAUTH2
    value: "DEBUG"
```

---

### Couche 11 : Actuator & Health Checks (Niveau monitoring)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE` / `logging.level.org.springframework.boot.actuate`
Définit le niveau de journalisation pour Spring Boot Actuator.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer toutes les opérations Spring Boot Actuator
- Trace l'accès aux endpoints, les health checks et les métriques
- Utile pour déboguer les problèmes d'exposition et d'accès aux endpoints Actuator

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_ENDPOINT` / `logging.level.org.springframework.boot.actuate.endpoint`
Définit le niveau de journalisation pour les endpoints Actuator.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer spécifiquement les opérations sur les endpoints Actuator
- Trace les appels aux endpoints, les réponses et les erreurs
- Utile pour déboguer les problèmes d'accès aux endpoints individuels

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_ENDPOINT
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_WEB` / `logging.level.org.springframework.boot.actuate.web`
Définit le niveau de journalisation pour les endpoints web Actuator.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les endpoints Actuator exposés via HTTP
- Trace les requêtes HTTP vers les endpoints et les réponses
- Utile pour déboguer les problèmes d'exposition web des endpoints

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_WEB
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_HEALTH` / `logging.level.org.springframework.boot.actuate.health`
Définit le niveau de journalisation pour les health checks Actuator.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations de health checks Actuator
- Trace l'exécution des health checks individuels (DB, Redis, RabbitMQ, etc.)
- Utile pour déboguer les problèmes de santé des composants

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_HEALTH
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_METRICS` / `logging.level.org.springframework.boot.actuate.metrics`
Définit le niveau de journalisation pour les métriques Actuator.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Usage :**
- Permet de tracer les opérations de collecte et d'exposition des métriques
- Trace la création, la mise à jour et l'exposition des métriques
- Utile pour déboguer les problèmes de métriques et de monitoring

**Exemple :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_METRICS
    value: "DEBUG"
```

#### `MANAGEMENT_ENDPOINTS_WEB_EXPOSURE_INCLUDE` / `management.endpoints.web.exposure.include`
Définit quels endpoints Actuator sont exposés via HTTP.

**Valeurs possibles :**
- `health` - Endpoint de santé
- `info` - Informations sur l'application
- `metrics` - Métriques
- `prometheus` - Métriques Prometheus
- `env` - Variables d'environnement et propriétés
- `*` - Tous les endpoints
- Liste séparée par des virgules : `health,info,metrics,env`

**Usage :**
- Expose les endpoints Actuator via HTTP
- L'endpoint `env` permet de consulter toutes les variables d'environnement et propriétés de l'application
- Utile pour déboguer la configuration en production

**Exemple :**
```yaml
env:
  - name: MANAGEMENT_ENDPOINTS_WEB_EXPOSURE_INCLUDE
    value: "health,info,metrics,env"
```

**Exemple de requête Actuator :**

Pour consulter les variables d'environnement via l'endpoint `env` :

```bash
# Requête GET pour obtenir toutes les variables d'environnement
curl http://localhost:8080/actuator/env

# Requête GET pour obtenir une propriété spécifique
curl http://localhost:8080/actuator/env/spring.application.name

# Requête GET pour obtenir les variables d'environnement système
curl http://localhost:8080/actuator/env/systemProperties

# Requête GET pour obtenir les variables d'environnement système (OS)
curl http://localhost:8080/actuator/env/systemEnvironment
```

**Note de sécurité :** L'endpoint `env` expose des informations sensibles (secrets, mots de passe, etc.). Ne l'exposez pas en production sans authentification appropriée.

#### `MANAGEMENT_ENDPOINT_HEALTH_SHOW_DETAILS` / `management.endpoint.health.show-details`
Définit si les détails des health checks doivent être affichés.

**Valeurs possibles :**
- `always` - Toujours afficher les détails
- `when-authorized` - Afficher seulement si autorisé
- `never` - Ne jamais afficher (par défaut)

**Usage :**
- Contrôle l'affichage des détails dans les réponses des health checks
- Permet de voir l'état détaillé de chaque composant (DB, Redis, etc.)
- Utile pour le débogage en développement, mais attention en production

**Exemple :**
```yaml
env:
  - name: MANAGEMENT_ENDPOINT_HEALTH_SHOW_DETAILS
    value: "always"
```

#### `MANAGEMENT_ENDPOINT_HEALTH_SHOW_COMPONENTS` / `management.endpoint.health.show-components`
Définit si les composants individuels des health checks doivent être affichés.

**Valeurs possibles :**
- `true` - Afficher les composants
- `false` - Ne pas afficher (par défaut)

**Usage :**
- Active l'affichage des composants individuels dans les health checks
- Permet de voir l'état de chaque composant (DB, Redis, RabbitMQ, etc.) séparément
- Utile pour identifier quel composant spécifique est en panne

**Exemple :**
```yaml
env:
  - name: MANAGEMENT_ENDPOINT_HEALTH_SHOW_COMPONENTS
    value: "true"
```

#### `MANAGEMENT_HEALTH_DISKSPACE_ENABLED` / `management.health.diskspace.enabled`
Active ou désactive le health check de l'espace disque.

**Valeurs possibles :**
- `true` - Active le health check disque (par défaut)
- `false` - Désactive le health check disque

**Usage :**
- Active ou désactive la vérification de l'espace disque disponible
- Le health check vérifie si l'espace disque est suffisant
- Utile pour surveiller l'espace disque et éviter les problèmes de stockage

**Exemple :**
```yaml
env:
  - name: MANAGEMENT_HEALTH_DISKSPACE_ENABLED
    value: "true"
```

#### `MANAGEMENT_HEALTH_DB_ENABLED` / `management.health.db.enabled`
Active ou désactive le health check de la base de données.

**Valeurs possibles :**
- `true` - Active le health check DB (par défaut)
- `false` - Désactive le health check DB

**Usage :**
- Active ou désactive la vérification de santé de la base de données
- Le health check teste la connexion à la base de données
- Utile pour surveiller la disponibilité de la base de données

**Exemple :**
```yaml
env:
  - name: MANAGEMENT_HEALTH_DB_ENABLED
    value: "true"
```

#### `MANAGEMENT_HEALTH_RABBIT_ENABLED` / `management.health.rabbit.enabled`
Active ou désactive le health check RabbitMQ.

**Valeurs possibles :**
- `true` - Active le health check RabbitMQ (par défaut si présent)
- `false` - Désactive le health check RabbitMQ

**Usage :**
- Active ou désactive la vérification de santé de RabbitMQ
- Le health check teste la connexion à RabbitMQ
- Utile pour surveiller la disponibilité de RabbitMQ

**Exemple :**
```yaml
env:
  - name: MANAGEMENT_HEALTH_RABBIT_ENABLED
    value: "true"
```

#### `MANAGEMENT_HEALTH_REDIS_ENABLED` / `management.health.redis.enabled`
Active ou désactive le health check Redis.

**Valeurs possibles :**
- `true` - Active le health check Redis (par défaut si présent)
- `false` - Désactive le health check Redis

**Usage :**
- Active ou désactive la vérification de santé de Redis
- Le health check teste la connexion à Redis
- Utile pour surveiller la disponibilité de Redis

**Exemple :**
```yaml
env:
  - name: MANAGEMENT_HEALTH_REDIS_ENABLED
    value: "true"
```

#### `MANAGEMENT_HEALTH_ELASTICSEARCH_ENABLED` / `management.health.elasticsearch.enabled`
Active ou désactive le health check Elasticsearch.

**Valeurs possibles :**
- `true` - Active le health check Elasticsearch (par défaut si présent)
- `false` - Désactive le health check Elasticsearch

**Usage :**
- Active ou désactive la vérification de santé d'Elasticsearch
- Le health check teste la connexion à Elasticsearch
- Utile pour surveiller la disponibilité d'Elasticsearch

**Exemple :**
```yaml
env:
  - name: MANAGEMENT_HEALTH_ELASTICSEARCH_ENABLED
    value: "true"
```

#### `MANAGEMENT_METRICS_EXPORT_PROMETHEUS_ENABLED` / `management.metrics.export.prometheus.enabled`
Active ou désactive l'export des métriques vers Prometheus.

**Valeurs possibles :**
- `true` - Active l'export Prometheus
- `false` - Désactive l'export Prometheus (par défaut)

**Usage :**
- Active ou désactive l'exposition des métriques au format Prometheus
- Les métriques sont exposées via l'endpoint `/actuator/prometheus`
- Utile pour l'intégration avec Prometheus et le monitoring

**Exemple :**
```yaml
env:
  - name: MANAGEMENT_METRICS_EXPORT_PROMETHEUS_ENABLED
    value: "true"
```

#### `MANAGEMENT_ENDPOINTS_WEB_BASE_PATH` / `management.endpoints.web.base-path`
Définit le chemin de base pour les endpoints Actuator.

**Valeurs possibles :**
- `/actuator` - Chemin par défaut
- Tout chemin personnalisé (ex. `/monitoring`)

**Usage :**
- Configure le chemin de base pour tous les endpoints Actuator
- Par défaut, les endpoints sont accessibles sous `/actuator/*`
- Utile pour personnaliser l'URL des endpoints ou pour des raisons de sécurité

**Exemple :**
```yaml
env:
  - name: MANAGEMENT_ENDPOINTS_WEB_BASE_PATH
    value: "/actuator"
```

---

### Couche 12 : JVM (Niveau machine virtuelle)

#### `JAVA_TOOL_OPTIONS`
Options JVM pour activer le traçage au niveau JVM.

**Valeurs possibles :**
- `-Djava.util.logging.config.file=...` - Configuration de logging JVM
- `-XX:+TraceClassLoading` - Trace le chargement des classes
- `-XX:+TraceClassUnloading` - Trace le déchargement des classes
- `-Djava.io.tmpdir=...` - Répertoire temporaire (peut être loggé)

**Usage :**
- Permet de configurer les options JVM pour le traçage et le débogage
- Utile pour tracer le chargement des classes, la configuration du logging JVM
- Les options sont appliquées automatiquement par la JVM au démarrage

**Exemple :**
```yaml
env:
  - name: JAVA_TOOL_OPTIONS
    value: "-Djava.util.logging.config.file=/path/to/logging.properties"
```

#### `JAVA_OPTS`
Options JVM supplémentaires.

**Valeurs possibles :**
- Toute option JVM valide (ex. `-Xmx512m`, `-XX:+HeapDumpOnOutOfMemoryError`)
- Options de débogage (ex. `-XX:+TraceClassLoading`, `-XX:+PrintGCDetails`)

**Usage :**
- Permet de configurer des options JVM supplémentaires pour le débogage
- Utile pour configurer la mémoire, le garbage collection, et le traçage JVM
- Les options sont passées à la JVM au démarrage de l'application

**Exemple :**
```yaml
env:
  - name: JAVA_OPTS
    value: "-Djava.io.tmpdir=/tmp -XX:+TraceClassLoading"
```

---

## Configuration complète pour tracer les opérations de fichiers

### Configuration recommandée pour le débogage des fichiers

```yaml
env:
  # Spring Boot
  - name: SPRING_DEBUG
    value: "true"
  - name: SPRING_PROFILES_ACTIVE
    value: "dev,debug"
  - name: SPRING_OUTPUT_ANSI_ENABLED
    value: "always"
  # Logging général
  - name: LOGGING_LEVEL_ROOT
    value: "INFO"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK
    value: "DEBUG"
  # Logging spécifique pour les opérations de fichiers
  - name: LOGGING_LEVEL_JAVA_IO
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_NIO
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_NIO_FILE
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_IO_FILEINPUTSTREAM
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_IO_FILEOUTPUTSTREAM
    value: "DEBUG"
```

### Configuration minimale pour le débogage des fichiers

```yaml
env:
  - name: LOGGING_LEVEL_ROOT
    value: "INFO"
  - name: LOGGING_LEVEL_JAVA_NIO_FILE
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_IO_FILEINPUTSTREAM
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_IO_FILEOUTPUTSTREAM
    value: "DEBUG"
```

### Configuration pour OpenShift / Kubernetes

Dans un fichier `ConfigMap` ou directement dans le déploiement :

```yaml
env:
  - name: SPRING_DEBUG
    value: "true"
  - name: LOGGING_LEVEL_ROOT
    value: "INFO"
  - name: LOGGING_LEVEL_JAVA_NIO_FILE
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_IO_FILEINPUTSTREAM
    value: "DEBUG"
  - name: LOGGING_LEVEL_JAVA_IO_FILEOUTPUTSTREAM
    value: "DEBUG"
```

---

## Configuration pour déboguer Vault & Cloud Config

### Configuration complète pour déboguer la récupération des secrets

Configuration complète pour tracer toutes les opérations de récupération de secrets depuis Vault et Cloud Config :

```yaml
env:
  # ============================================
  # Debug Vault - Récupération des secrets
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_CORE
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_AUTHENTICATION
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_SUPPORT
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_CLIENT
    value: "DEBUG"
  
  # ============================================
  # Debug Cloud Config - Récupération des configs
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_CLIENT
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_CLIENT_CONFIG_SERVICE_BOOTSTRAP
    value: "TRACE"
  
  # ============================================
  # Debug Bootstrap (chargement initial)
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_BOOTSTRAP
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONTEXT
    value: "DEBUG"
  
  # ============================================
  # Debug HTTP pour voir les requêtes
  # ============================================
  - name: LOGGING_LEVEL_ORG_APACHE_HTTP
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_APACHE_HTTP_WIRE
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_CLIENT
    value: "DEBUG"
  
  # ============================================
  # Configuration Vault (pour vérifier les paramètres)
  # ============================================
  - name: SPRING_CLOUD_VAULT_URI
    value: "http://vault:8200"  # Votre URI Vault
  - name: SPRING_CLOUD_VAULT_AUTHENTICATION
    value: "TOKEN"  # ou APPROLE, KUBERNETES, etc.
  - name: SPRING_CLOUD_VAULT_KV_ENABLED
    value: "true"
  - name: SPRING_CLOUD_VAULT_KV_BACKEND
    value: "secret"
  - name: SPRING_CLOUD_VAULT_FAIL_FAST
    value: "true"  # Pour voir les erreurs immédiatement
  
  # ============================================
  # Configuration Cloud Config (pour vérifier les paramètres)
  # ============================================
  - name: SPRING_CLOUD_CONFIG_URI
    value: "http://config-server:8888"  # Votre URI Config Server
  - name: SPRING_CLOUD_CONFIG_NAME
    value: "myapp"  # Nom de votre application
  - name: SPRING_CLOUD_CONFIG_PROFILE
    value: "dev"  # Votre profil
  - name: SPRING_CLOUD_CONFIG_FAIL_FAST
    value: "true"  # Pour voir les erreurs immédiatement
  
  # ============================================
  # Debug général Spring pour voir le contexte
  # ============================================
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CONTEXT
    value: "DEBUG"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BEANS
    value: "DEBUG"
```

### Configuration minimale pour déboguer Vault & Cloud Config

Configuration minimale pour un débogage rapide :

```yaml
env:
  # Vault - Debug complet
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT
    value: "TRACE"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_CORE
    value: "TRACE"
  
  # Cloud Config - Debug complet
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG
    value: "TRACE"
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_CLIENT
    value: "TRACE"
  
  # Bootstrap pour voir le chargement initial
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_BOOTSTRAP
    value: "DEBUG"
  
  # Fail fast pour voir les erreurs immédiatement
  - name: SPRING_CLOUD_VAULT_FAIL_FAST
    value: "true"
  - name: SPRING_CLOUD_CONFIG_FAIL_FAST
    value: "true"
```

### Ce que vous verrez dans les logs

**Pour Vault :**
- Connexion au serveur Vault
- Processus d'authentification (token, AppRole, Kubernetes, etc.)
- Requêtes vers les backends KV (Key-Value)
- Secrets récupérés (les valeurs sont masquées pour la sécurité)
- Erreurs d'authentification ou de connexion

**Pour Cloud Config :**
- Connexion au serveur Config
- Requêtes HTTP vers le serveur Config
- Configuration récupérée depuis le serveur
- Erreurs de connexion ou de parsing

**Pour le Bootstrap :**
- Ordre de chargement (Vault puis Config, ou l'inverse selon la configuration)
- Propriétés chargées depuis chaque source
- Résolution des conflits de propriétés

### Points clés à vérifier dans les logs

- `VaultTemplate` - Toutes les opérations Vault
- `ConfigServicePropertySourceLocator` - Récupération depuis Cloud Config
- `VaultAuthentication` - Processus d'authentification Vault
- `RestTemplate` ou `WebClient` - Requêtes HTTP vers Vault/Config Server
- `BootstrapApplicationListener` - Ordre de chargement des configurations

**Note :** Le niveau `TRACE` génère beaucoup de logs. Commencez par `DEBUG`, puis passez à `TRACE` si vous avez besoin de plus de détails.

---

## Notes importantes

- Les variables d'environnement peuvent être écrites en majuscules (ex. `SPRING_DEBUG`) ou en minuscules (ex. `spring.debug`)
- Les underscores dans les variables d'environnement sont convertis en points dans les propriétés Spring (ex. `LOGGING_LEVEL_ROOT` → `logging.level.root`)
- Les valeurs booléennes peuvent être `true`/`false` ou `1`/`0`
- Plusieurs profils peuvent être spécifiés séparés par des virgules
- Les niveaux de journalisation ne sont pas sensibles à la casse
- Pour OpenShift, utilisez les variables d'environnement dans les `Deployment` ou `ConfigMap`
- Le niveau `TRACE` peut générer beaucoup de logs, utilisez-le avec précaution en production

---

## Variables supplémentaires utiles

#### `DEBUG` / `debug`
Active la journalisation de débogage pour Spring Boot.

**Valeurs possibles :**
- `true` - Active la journalisation de débogage
- `false` - Désactive la journalisation de débogage (par défaut)
- `1` - Active la journalisation de débogage
- `0` - Désactive la journalisation de débogage

**Exemple :**
```yaml
env:
  - name: DEBUG
    value: "true"
```

#### `SPRING_JMX_ENABLED` / `spring.jmx.enabled`
Active la surveillance JMX.

**Valeurs possibles :**
- `true` - Active JMX (par défaut)
- `false` - Désactive JMX

**Exemple :**
```yaml
env:
  - name: SPRING_JMX_ENABLED
    value: "true"
```

#### `SPRING_DEVTOOLS_RESTART_ENABLED` / `spring.devtools.restart.enabled`
Active le redémarrage automatique avec Spring DevTools.

**Valeurs possibles :**
- `true` - Active le redémarrage automatique (par défaut en mode dev)
- `false` - Désactive le redémarrage automatique

**Exemple :**
```yaml
env:
  - name: SPRING_DEVTOOLS_RESTART_ENABLED
    value: "true"
```

#### `SPRING_DEVTOOLS_LIVERELOAD_ENABLED` / `spring.devtools.livereload.enabled`
Active le support LiveReload.

**Valeurs possibles :**
- `true` - Active LiveReload (par défaut en mode dev)
- `false` - Désactive LiveReload

**Exemple :**
```yaml
env:
  - name: SPRING_DEVTOOLS_LIVERELOAD_ENABLED
    value: "true"
```

#### `SPRING_SHELL_INTERACTIVE_ENABLED` / `spring.shell.interactive.enabled`
Active le mode shell interactif.

**Valeurs possibles :**
- `true` - Active le shell interactif
- `false` - Désactive le shell interactif

**Exemple :**
```yaml
env:
  - name: SPRING_SHELL_INTERACTIVE_ENABLED
    value: "true"
```
