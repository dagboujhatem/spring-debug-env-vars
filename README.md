# Variables d'environnement de débogage Spring

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

**Utilisation :**
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
  - [Couche 8 : HTTP/Web (Niveau réseau)](#couche-8--httpweb-niveau-réseau)
  - [Couche 9 : Security (Niveau sécurité Spring)](#couche-9--security-niveau-sécurité-spring)
  - [Couche 10 : Actuator & Health Checks (Niveau monitoring)](#couche-10--actuator--health-checks-niveau-monitoring)
  - [Couche 11 : JVM (Niveau machine virtuelle)](#couche-11--jvm-niveau-machine-virtuelle)
- [Configuration complète pour tracer les opérations de fichiers](#configuration-complète-pour-tracer-les-opérations-de-fichiers)
  - [Configuration recommandée pour le débogage des fichiers](#configuration-recommandée-pour-le-débogage-des-fichiers)
  - [Configuration minimale pour le débogage des fichiers](#configuration-minimale-pour-le-débogage-des-fichiers)
  - [Configuration pour OpenShift / Kubernetes](#configuration-pour-openshift--kubernetes)
- [Notes importantes](#notes-importantes)
- [Variables supplémentaires utiles](#variables-supplémentaires-utiles)

---
## List des config par couche 

```yaml
env:
  # 
  - name: LOGGING_LEVEL_ROOT
    value: "DEBUG"
```
---

## Variables d'environnement par couches

### Couche 1 : Spring Boot (Niveau application)

#### `LOGGING_LEVEL_ROOT` / `logging.level.root`
Définit le niveau de journalisation racine pour toute l'application.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB` / `logging.level.org.springframework.web`
Définit le niveau de journalisation de Spring Web.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CONTEXT` / `logging.level.org.springframework.context`
Définit le niveau de journalisation du contexte Spring (chargement de fichiers de configuration, beans, etc.).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CONTEXT
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BEANS` / `logging.level.org.springframework.beans`
Définit le niveau de journalisation des beans Spring.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
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

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_CLIENT` / `logging.level.org.springframework.cloud.config.client`
Définit le niveau de journalisation pour le client Spring Cloud Config.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_CLIENT
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_SERVER` / `logging.level.org.springframework.cloud.config.server`
Définit le niveau de journalisation pour le serveur Spring Cloud Config.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_CLOUD_CONFIG_SERVER
    value: "DEBUG"
```

#### `SPRING_CLOUD_CONFIG_URI` / `spring.cloud.config.uri`
Définit l'URI du serveur Spring Cloud Config.

**Valeurs possibles :**
- URL complète du serveur Config (ex. `http://config-server:8888`)
- URL avec protocole HTTPS si SSL activé

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
```yaml
env:
  - name: SPRING_CLOUD_CONFIG_RETRY_MAX_ATTEMPTS
    value: "6"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT` / `logging.level.org.springframework.vault`
Définit le niveau de journalisation pour Spring Vault (HashiCorp Vault).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_CORE` / `logging.level.org.springframework.vault.core`
Définit le niveau de journalisation pour le cœur de Spring Vault.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_CORE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_AUTHENTICATION` / `logging.level.org.springframework.vault.authentication`
Définit le niveau de journalisation pour l'authentification Vault.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_VAULT_AUTHENTICATION
    value: "DEBUG"
```

#### `SPRING_CLOUD_VAULT_URI` / `spring.cloud.vault.uri`
Définit l'URI du serveur HashiCorp Vault.

**Valeurs possibles :**
- URL complète du serveur Vault (ex. `http://vault:8200`)
- URL avec protocole HTTPS si SSL activé

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVA_IO
    value: "DEBUG"
```

#### `LOGGING_LEVEL_JAVA_NIO` / `logging.level.java.nio`
Définit le niveau de journalisation pour les opérations NIO (New I/O) Java.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVA_NIO
    value: "DEBUG"
```

#### `LOGGING_LEVEL_JAVA_NIO_FILE` / `logging.level.java.nio.file`
Définit le niveau de journalisation pour les opérations sur les fichiers via NIO (Files.read(), Files.write(), etc.).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVA_NIO_FILE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_JAVA_IO_FILEINPUTSTREAM` / `logging.level.java.io.FileInputStream`
Définit le niveau de journalisation pour FileInputStream (lecture de fichiers).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVA_IO_FILEINPUTSTREAM
    value: "DEBUG"
```

#### `LOGGING_LEVEL_JAVA_IO_FILEOUTPUTSTREAM` / `logging.level.java.io.FileOutputStream`
Définit le niveau de journalisation pour FileOutputStream (écriture de fichiers).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
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

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_AMAZONAWS
    value: "DEBUG"
```

#### `LOGGING_LEVEL_COM_AMAZONAWS_SERVICES_S3` / `logging.level.com.amazonaws.services.s3`
Définit le niveau de journalisation pour les opérations S3 spécifiques.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_AMAZONAWS_SERVICES_S3
    value: "DEBUG"
```

#### `LOGGING_LEVEL_COM_AMAZONAWS_AUTH` / `logging.level.com.amazonaws.auth`
Définit le niveau de journalisation pour l'authentification AWS.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_AMAZONAWS_AUTH
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_HTTP` / `logging.level.org.apache.http`
Définit le niveau de journalisation pour Apache HTTP Client (utilisé par AWS SDK).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
```yaml
env:
  - name: AWS_REGION
    value: "us-east-1"
```

#### `AWS_S3_BUCKET` / `aws.s3.bucket`
Définit le nom du bucket S3 à utiliser.

**Valeurs possibles :**
- Nom de bucket S3 valide

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_SSL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_JAVAX_NET_SSL` / `logging.level.javax.net.ssl`
Définit le niveau de journalisation pour les opérations SSL/TLS au niveau Java (javax.net.ssl).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_JAVAX_NET_SSL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_SUN_SECURITY_SSL` / `logging.level.sun.security.ssl`
Définit le niveau de journalisation pour les opérations SSL/TLS au niveau Sun Security (implémentation Java).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_SUN_SECURITY_SSL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_TOMCAT_UTIL_NET_SSL` / `logging.level.org.apache.tomcat.util.net.ssl`
Définit le niveau de journalisation pour SSL/TLS dans Tomcat (si utilisé comme serveur embarqué).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_TOMCAT_UTIL_NET_SSL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_CATALINA_LOADER` / `logging.level.org.apache.catalina.loader`
Définit le niveau de journalisation pour le chargement SSL dans Tomcat.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_CATALINA_LOADER
    value: "DEBUG"
```

---

### Couche 7 : Database (Niveau base de données)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC` / `logging.level.org.springframework.jdbc`
Définit le niveau de journalisation pour Spring JDBC (accès aux bases de données).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC_CORE` / `logging.level.org.springframework.jdbc.core`
Définit le niveau de journalisation pour le cœur JDBC de Spring (exécution des requêtes SQL).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC_CORE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC_DATASOURCE` / `logging.level.org.springframework.jdbc.datasource`
Définit le niveau de journalisation pour les sources de données Spring JDBC.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_JDBC_DATASOURCE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_HIBERNATE` / `logging.level.org.hibernate`
Définit le niveau de journalisation pour Hibernate (ORM).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_HIBERNATE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_HIBERNATE_SQL` / `logging.level.org.hibernate.SQL`
Définit le niveau de journalisation pour les requêtes SQL générées par Hibernate.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_HIBERNATE_SQL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_HIBERNATE_TYPE_DESCRIPTOR_SQL` / `logging.level.org.hibernate.type.descriptor.sql.BasicBinder`
Définit le niveau de journalisation pour les paramètres SQL bindés par Hibernate.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_HIBERNATE_TYPE_DESCRIPTOR_SQL
    value: "TRACE"
```

#### `LOGGING_LEVEL_COM_ZAXXER_HIKARI` / `logging.level.com.zaxxer.hikari`
Définit le niveau de journalisation pour HikariCP (pool de connexions).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_ZAXXER_HIKARI
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_TOMCAT_JDBC_POOL` / `logging.level.org.apache.tomcat.jdbc.pool`
Définit le niveau de journalisation pour Tomcat JDBC Pool (si utilisé).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_TOMCAT_JDBC_POOL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_COM_MYSQL_CJ` / `logging.level.com.mysql.cj`
Définit le niveau de journalisation pour le driver MySQL Connector/J.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_COM_MYSQL_CJ
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_POSTGRESQL` / `logging.level.org.postgresql`
Définit le niveau de journalisation pour le driver PostgreSQL.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_POSTGRESQL
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORACLE_JDBC` / `logging.level.oracle.jdbc`
Définit le niveau de journalisation pour le driver Oracle JDBC.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORACLE_JDBC
    value: "DEBUG"
```

---

### Couche 8 : HTTP/Web (Niveau réseau)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB` / `logging.level.org.springframework.web`
Définit le niveau de journalisation pour Spring Web (contrôleurs, requêtes HTTP).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_SERVLET` / `logging.level.org.springframework.web.servlet`
Définit le niveau de journalisation pour Spring MVC Servlet.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_SERVLET
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_FILTER` / `logging.level.org.springframework.web.filter`
Définit le niveau de journalisation pour les filtres Spring Web.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_WEB_FILTER
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_CATALINA` / `logging.level.org.apache.catalina`
Définit le niveau de journalisation pour Tomcat Catalina (serveur embarqué).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_CATALINA
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_CATALINA_CONNECTOR` / `logging.level.org.apache.catalina.connector`
Définit le niveau de journalisation pour les connecteurs Tomcat (HTTP/HTTPS).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_CATALINA_CONNECTOR
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_CATALINA_CORE` / `logging.level.org.apache.catalina.core`
Définit le niveau de journalisation pour le cœur de Tomcat.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_CATALINA_CORE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_ECLIPSE_JETTY` / `logging.level.org.eclipse.jetty`
Définit le niveau de journalisation pour Jetty (si utilisé comme serveur embarqué).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_ECLIPSE_JETTY
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_APACHE_HTTP` / `logging.level.org.apache.http`
Définit le niveau de journalisation pour Apache HttpClient (si utilisé).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_APACHE_HTTP
    value: "DEBUG"
```

---

### Couche 9 : Security (Niveau sécurité Spring)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY` / `logging.level.org.springframework.security`
Définit le niveau de journalisation pour Spring Security.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_WEB` / `logging.level.org.springframework.security.web`
Définit le niveau de journalisation pour Spring Security Web (filtres de sécurité).

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_WEB
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_AUTHENTICATION` / `logging.level.org.springframework.security.authentication`
Définit le niveau de journalisation pour l'authentification Spring Security.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_AUTHENTICATION
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_ACCESS` / `logging.level.org.springframework.security.access`
Définit le niveau de journalisation pour le contrôle d'accès Spring Security.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_ACCESS
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_OAUTH2` / `logging.level.org.springframework.security.oauth2`
Définit le niveau de journalisation pour OAuth2 dans Spring Security.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_SECURITY_OAUTH2
    value: "DEBUG"
```

---

### Couche 10 : Actuator & Health Checks (Niveau monitoring)

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE` / `logging.level.org.springframework.boot.actuate`
Définit le niveau de journalisation pour Spring Boot Actuator.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_ENDPOINT` / `logging.level.org.springframework.boot.actuate.endpoint`
Définit le niveau de journalisation pour les endpoints Actuator.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_ENDPOINT
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_WEB` / `logging.level.org.springframework.boot.actuate.web`
Définit le niveau de journalisation pour les endpoints web Actuator.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_WEB
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_HEALTH` / `logging.level.org.springframework.boot.actuate.health`
Définit le niveau de journalisation pour les health checks Actuator.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_HEALTH
    value: "DEBUG"
```

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_ACTUATE_METRICS` / `logging.level.org.springframework.boot.actuate.metrics`
Définit le niveau de journalisation pour les métriques Actuator.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
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
- `*` - Tous les endpoints
- Liste séparée par des virgules : `health,info,metrics`

**Utilisation :**
```yaml
env:
  - name: MANAGEMENT_ENDPOINTS_WEB_EXPOSURE_INCLUDE
    value: "health,info,metrics"
```

#### `MANAGEMENT_ENDPOINT_HEALTH_SHOW_DETAILS` / `management.endpoint.health.show-details`
Définit si les détails des health checks doivent être affichés.

**Valeurs possibles :**
- `always` - Toujours afficher les détails
- `when-authorized` - Afficher seulement si autorisé
- `never` - Ne jamais afficher (par défaut)

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
```yaml
env:
  - name: MANAGEMENT_ENDPOINTS_WEB_BASE_PATH
    value: "/actuator"
```

---

### Couche 11 : JVM (Niveau machine virtuelle)

#### `JAVA_TOOL_OPTIONS`
Options JVM pour activer le traçage au niveau JVM.

**Valeurs possibles :**
- `-Djava.util.logging.config.file=...` - Configuration de logging JVM
- `-XX:+TraceClassLoading` - Trace le chargement des classes
- `-XX:+TraceClassUnloading` - Trace le déchargement des classes
- `-Djava.io.tmpdir=...` - Répertoire temporaire (peut être loggé)

**Utilisation :**
```yaml
env:
  - name: JAVA_TOOL_OPTIONS
    value: "-Djava.util.logging.config.file=/path/to/logging.properties"
```

#### `JAVA_OPTS`
Options JVM supplémentaires.

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
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

**Utilisation :**
```yaml
env:
  - name: SPRING_SHELL_INTERACTIVE_ENABLED
    value: "true"
```
