# Variables d'environnement de débogage Spring

## Context :

Voici les logs Spring / Java pour tracer TOUT ce qui concerne la lecture / écriture de fichiers (File read / write), 100 % via variables d'environnement, utilisables en Spring Boot 3.x / OpenShift.

Je te mets ça par couches, du plus haut niveau Spring jusqu'au niveau JVM pur.

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
  - [Couche 3 : Java I/O (Niveau Java - Opérations de fichiers)](#couche-3--java-io-niveau-java---opérations-de-fichiers)
  - [Couche 4 : JVM (Niveau machine virtuelle)](#couche-4--jvm-niveau-machine-virtuelle)
- [Configuration complète pour tracer les opérations de fichiers](#configuration-complète-pour-tracer-les-opérations-de-fichiers)
  - [Configuration recommandée pour le débogage des fichiers](#configuration-recommandée-pour-le-débogage-des-fichiers)
  - [Configuration minimale pour le débogage des fichiers](#configuration-minimale-pour-le-débogage-des-fichiers)
  - [Configuration pour OpenShift / Kubernetes](#configuration-pour-openshift--kubernetes)
- [Notes importantes](#notes-importantes)
- [Variables supplémentaires utiles](#variables-supplémentaires-utiles)

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

#### `LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_SSL` / `logging.level.org.springframework.boot.ssl`
Définit le niveau de journalisation pour SSL/TLS dans Spring Boot.

**Valeurs possibles :** `TRACE`, `DEBUG`, `INFO`, `WARN`, `ERROR`, `OFF`

**Utilisation :**
```yaml
env:
  - name: LOGGING_LEVEL_ORG_SPRINGFRAMEWORK_BOOT_SSL
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

### Couche 3 : Java I/O (Niveau Java - Opérations de fichiers)

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

### Couche 4 : JVM (Niveau machine virtuelle)

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

```bash
# Spring Boot
export SPRING_DEBUG=true
export SPRING_PROFILES_ACTIVE=dev,debug
export SPRING_OUTPUT_ANSI_ENABLED=always

# Logging général
export LOGGING_LEVEL_ROOT=INFO
export LOGGING_LEVEL_ORG_SPRINGFRAMEWORK=DEBUG

# Logging spécifique pour les opérations de fichiers
export LOGGING_LEVEL_JAVA_IO=DEBUG
export LOGGING_LEVEL_JAVA_NIO=DEBUG
export LOGGING_LEVEL_JAVA_NIO_FILE=DEBUG
export LOGGING_LEVEL_JAVA_IO_FILEINPUTSTREAM=DEBUG
export LOGGING_LEVEL_JAVA_IO_FILEOUTPUTSTREAM=DEBUG
```

### Configuration minimale pour le débogage des fichiers

```bash
export LOGGING_LEVEL_ROOT=INFO
export LOGGING_LEVEL_JAVA_NIO_FILE=DEBUG
export LOGGING_LEVEL_JAVA_IO_FILEINPUTSTREAM=DEBUG
export LOGGING_LEVEL_JAVA_IO_FILEOUTPUTSTREAM=DEBUG
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
