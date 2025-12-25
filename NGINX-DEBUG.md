# Variables d'environnement de débogage Nginx

Ce document répertorie toutes les variables d'environnement disponibles pour activer le débogage et le traçage dans Nginx, notamment pour les Ingress Controllers, le cache, le proxy et autres fonctionnalités.

![Nginx](https://img.shields.io/badge/Nginx-Supported-success?logo=nginx)
![Kubernetes](https://img.shields.io/badge/Kubernetes-Supported-blue?logo=kubernetes)
![OpenShift](https://img.shields.io/badge/OpenShift-Supported-orange?logo=redhatopenshift)
![Ingress Controller](https://img.shields.io/badge/Ingress%20Controller-Supported-blue)
![Proxy](https://img.shields.io/badge/Proxy-Supported-orange)
![Cache](https://img.shields.io/badge/Cache-Supported-green)
![YAML](https://img.shields.io/badge/Format-YAML-blue?logo=YAML)

## Context

Ce document couvre les variables d'environnement pour déboguer Nginx dans différents contextes :
- **Nginx Ingress Controller** (Kubernetes/OpenShift)
- **Nginx en tant que reverse proxy**
- **Nginx avec cache**
- **Nginx avec load balancing**
- **Nginx avec SSL/TLS**

**Objectif :** Fournir un guide complet pour configurer le logging et le débogage Nginx via des variables d'environnement, utilisables directement dans Kubernetes/OpenShift.

---

## Sommaire

- [Nginx Ingress Controller](#nginx-ingress-controller)
  - [Logging général](#logging-général)
  - [Cache](#cache)
  - [Proxy](#proxy)
  - [SSL/TLS](#ssltls)
  - [Rate Limiting](#rate-limiting)
  - [Load Balancing](#load-balancing)
- [Nginx Standalone](#nginx-standalone)
- [Configuration complète pour le débogage](#configuration-complète-pour-le-débogage)
- [Notes importantes](#notes-importantes)

---

## Nginx Ingress Controller

### Logging général

#### `NGINX_ERROR_LOG_LEVEL` / `error_log_level`
Définit le niveau de journalisation des erreurs Nginx.

**Valeurs possibles :**
- `debug` - Le plus verbeux (affiche tout)
- `info` - Niveau info
- `notice` - Notifications
- `warn` - Avertissements
- `error` - Erreurs uniquement
- `crit` - Erreurs critiques uniquement
- `alert` - Alertes
- `emerg` - Urgences uniquement

**Usage :**
- Contrôle le niveau de verbosité des logs d'erreur Nginx
- Le niveau `debug` est très verbeux mais utile pour le débogage approfondi
- Utile pour identifier les problèmes de configuration et d'exécution

**Exemple :**
```yaml
env:
  - name: NGINX_ERROR_LOG_LEVEL
    value: "debug"
```

#### `NGINX_ACCESS_LOG` / `access_log`
Active ou désactive les logs d'accès Nginx.

**Valeurs possibles :**
- `true` - Active les logs d'accès
- `false` - Désactive les logs d'accès

**Usage :**
- Active ou désactive l'enregistrement des requêtes HTTP
- Les logs d'accès contiennent des informations sur chaque requête (IP, méthode, URL, statut, etc.)
- Utile pour analyser le trafic et déboguer les problèmes de routage

**Exemple :**
```yaml
env:
  - name: NGINX_ACCESS_LOG
    value: "true"
```

#### `NGINX_LOG_FORMAT` / `log_format`
Définit le format personnalisé des logs d'accès.

**Valeurs possibles :**
- Format personnalisé (ex. `'$remote_addr - $remote_user [$time_local] "$request" $status $body_bytes_sent "$http_referer" "$http_user_agent" $request_time'`)
- Format étendu avec plus de détails

**Usage :**
- Permet de personnaliser les informations enregistrées dans les logs d'accès
- Peut inclure des variables Nginx comme `$request_time`, `$upstream_response_time`, etc.
- Utile pour obtenir plus de détails sur les requêtes

**Exemple :**
```yaml
env:
  - name: NGINX_LOG_FORMAT
    value: '$remote_addr - $remote_user [$time_local] "$request" $status $body_bytes_sent "$http_referer" "$http_user_agent" $request_time $upstream_response_time'
```

### Cache

#### `NGINX_CACHE_ENABLED` / `enable-cache`
Active ou désactive le cache Nginx.

**Valeurs possibles :**
- `true` - Active le cache
- `false` - Désactive le cache

**Usage :**
- Active ou désactive le système de cache Nginx
- Le cache permet d'améliorer les performances en stockant les réponses
- Utile pour déboguer les problèmes de cache

**Exemple :**
```yaml
env:
  - name: NGINX_CACHE_ENABLED
    value: "true"
```

#### `NGINX_CACHE_PATH` / `cache-path`
Définit le chemin du répertoire de cache.

**Valeurs possibles :**
- Chemin du répertoire (ex. `/var/cache/nginx`)

**Usage :**
- Spécifie où Nginx stocke les fichiers en cache
- Utile pour vérifier l'utilisation du cache et déboguer les problèmes de stockage

**Exemple :**
```yaml
env:
  - name: NGINX_CACHE_PATH
    value: "/var/cache/nginx"
```

#### `NGINX_CACHE_MAX_SIZE` / `cache-max-size`
Définit la taille maximale du cache.

**Valeurs possibles :**
- Taille avec unité (ex. `1g`, `500m`, `10g`)

**Usage :**
- Limite la taille totale du cache
- Utile pour contrôler l'utilisation du disque
- Peut affecter les performances si trop petit

**Exemple :**
```yaml
env:
  - name: NGINX_CACHE_MAX_SIZE
    value: "1g"
```

#### `NGINX_CACHE_INACTIVE` / `cache-inactive`
Définit la durée d'inactivité avant suppression du cache.

**Valeurs possibles :**
- Durée avec unité (ex. `60m`, `1h`, `24h`)

**Usage :**
- Définit combien de temps un élément peut rester dans le cache sans être utilisé
- Utile pour gérer l'expiration automatique du cache

**Exemple :**
```yaml
env:
  - name: NGINX_CACHE_INACTIVE
    value: "60m"
```

#### `NGINX_CACHE_DEBUG` / `cache-debug`
Active le mode debug pour le cache.

**Valeurs possibles :**
- `true` - Active le debug du cache
- `false` - Désactive le debug du cache

**Usage :**
- Active des logs détaillés sur les opérations de cache
- Trace les hits, misses, et les opérations de stockage
- Utile pour comprendre le comportement du cache

**Exemple :**
```yaml
env:
  - name: NGINX_CACHE_DEBUG
    value: "true"
```

### Proxy

#### `NGINX_PROXY_CONNECT_TIMEOUT` / `proxy-connect-timeout`
Définit le timeout pour la connexion au serveur upstream.

**Valeurs possibles :**
- Durée avec unité (ex. `60s`, `5m`)

**Usage :**
- Définit le temps maximum pour établir une connexion avec le serveur backend
- Utile pour déboguer les problèmes de timeout de connexion

**Exemple :**
```yaml
env:
  - name: NGINX_PROXY_CONNECT_TIMEOUT
    value: "60s"
```

#### `NGINX_PROXY_SEND_TIMEOUT` / `proxy-send-timeout`
Définit le timeout pour l'envoi de requêtes au serveur upstream.

**Valeurs possibles :**
- Durée avec unité (ex. `60s`, `5m`)

**Usage :**
- Définit le temps maximum pour envoyer une requête au serveur backend
- Utile pour déboguer les problèmes de timeout d'envoi

**Exemple :**
```yaml
env:
  - name: NGINX_PROXY_SEND_TIMEOUT
    value: "60s"
```

#### `NGINX_PROXY_READ_TIMEOUT` / `proxy-read-timeout`
Définit le timeout pour la lecture de réponses du serveur upstream.

**Valeurs possibles :**
- Durée avec unité (ex. `60s`, `5m`)

**Usage :**
- Définit le temps maximum pour lire une réponse du serveur backend
- Utile pour déboguer les problèmes de timeout de lecture

**Exemple :**
```yaml
env:
  - name: NGINX_PROXY_READ_TIMEOUT
    value: "60s"
```

#### `NGINX_PROXY_BUFFER_SIZE` / `proxy-buffer-size`
Définit la taille des buffers pour les réponses proxy.

**Valeurs possibles :**
- Taille avec unité (ex. `4k`, `8k`, `16k`)

**Usage :**
- Contrôle la taille des buffers utilisés pour lire les réponses du serveur backend
- Peut affecter les performances et la mémoire
- Utile pour déboguer les problèmes de buffer overflow

**Exemple :**
```yaml
env:
  - name: NGINX_PROXY_BUFFER_SIZE
    value: "8k"
```

#### `NGINX_PROXY_BUFFERS` / `proxy-buffers`
Définit le nombre et la taille des buffers pour les réponses proxy.

**Valeurs possibles :**
- Format : `nombre taille` (ex. `8 8k`, `4 16k`)

**Usage :**
- Contrôle le nombre et la taille des buffers pour les réponses
- Utile pour optimiser les performances avec de grandes réponses

**Exemple :**
```yaml
env:
  - name: NGINX_PROXY_BUFFERS
    value: "8 8k"
```

#### `NGINX_PROXY_BUFFERING` / `proxy-buffering`
Active ou désactive le buffering des réponses proxy.

**Valeurs possibles :**
- `on` - Active le buffering
- `off` - Désactive le buffering

**Usage :**
- Contrôle si Nginx bufferise les réponses du serveur backend
- Le buffering peut améliorer les performances mais peut retarder les réponses
- Utile pour déboguer les problèmes de streaming

**Exemple :**
```yaml
env:
  - name: NGINX_PROXY_BUFFERING
    value: "on"
```

#### `NGINX_PROXY_PASS_HEADERS` / `proxy-pass-headers`
Active le passage de tous les headers au serveur upstream.

**Valeurs possibles :**
- `true` - Passe tous les headers
- `false` - Ne passe que les headers par défaut

**Usage :**
- Contrôle quels headers sont transmis au serveur backend
- Utile pour déboguer les problèmes de headers manquants

**Exemple :**
```yaml
env:
  - name: NGINX_PROXY_PASS_HEADERS
    value: "true"
```

### SSL/TLS

#### `NGINX_SSL_VERIFY` / `ssl-verify`
Active ou désactive la vérification SSL pour les connexions upstream.

**Valeurs possibles :**
- `on` - Active la vérification SSL
- `off` - Désactive la vérification SSL

**Usage :**
- Contrôle si Nginx vérifie les certificats SSL des serveurs upstream
- Désactiver peut être utile pour le développement mais dangereux en production
- Utile pour déboguer les problèmes de certificats SSL

**Exemple :**
```yaml
env:
  - name: NGINX_SSL_VERIFY
    value: "on"
```

#### `NGINX_SSL_VERIFY_DEPTH` / `ssl-verify-depth`
Définit la profondeur de vérification de la chaîne de certificats.

**Valeurs possibles :**
- Nombre entier (ex. `1`, `2`, `3`)

**Usage :**
- Contrôle la profondeur de vérification de la chaîne de certificats
- Utile pour déboguer les problèmes de chaîne de certificats

**Exemple :**
```yaml
env:
  - name: NGINX_SSL_VERIFY_DEPTH
    value: "2"
```

#### `NGINX_SSL_PROTOCOLS` / `ssl-protocols`
Définit les protocoles SSL/TLS autorisés.

**Valeurs possibles :**
- Liste de protocoles (ex. `TLSv1.2 TLSv1.3`, `TLSv1.3`)

**Usage :**
- Contrôle quels protocoles SSL/TLS sont acceptés
- Utile pour déboguer les problèmes de négociation SSL/TLS

**Exemple :**
```yaml
env:
  - name: NGINX_SSL_PROTOCOLS
    value: "TLSv1.2 TLSv1.3"
```

### Rate Limiting

#### `NGINX_RATE_LIMIT_ENABLED` / `rate-limit-enabled`
Active ou désactive le rate limiting.

**Valeurs possibles :**
- `true` - Active le rate limiting
- `false` - Désactive le rate limiting

**Usage :**
- Active ou désactive la limitation de débit
- Utile pour protéger contre les abus et déboguer les problèmes de limitation

**Exemple :**
```yaml
env:
  - name: NGINX_RATE_LIMIT_ENABLED
    value: "true"
```

#### `NGINX_RATE_LIMIT_ZONE_SIZE` / `rate-limit-zone-size`
Définit la taille de la zone de rate limiting.

**Valeurs possibles :**
- Taille avec unité (ex. `10m`, `50m`)

**Usage :**
- Définit la taille de la zone mémoire pour stocker les informations de rate limiting
- Utile pour gérer la mémoire et déboguer les problèmes de rate limiting

**Exemple :**
```yaml
env:
  - name: NGINX_RATE_LIMIT_ZONE_SIZE
    value: "10m"
```

#### `NGINX_RATE_LIMIT_RATE` / `rate-limit-rate`
Définit le taux de limitation (requêtes par seconde).

**Valeurs possibles :**
- Taux avec unité (ex. `10r/s`, `100r/m`)

**Usage :**
- Définit le nombre maximum de requêtes autorisées par période
- Utile pour contrôler le débit et déboguer les problèmes de limitation

**Exemple :**
```yaml
env:
  - name: NGINX_RATE_LIMIT_RATE
    value: "10r/s"
```

### Load Balancing

#### `NGINX_UPSTREAM_KEEPALIVE` / `upstream-keepalive`
Active ou désactive les connexions keepalive vers les serveurs upstream.

**Valeurs possibles :**
- Nombre de connexions (ex. `32`, `64`)

**Usage :**
- Contrôle le nombre de connexions keepalive vers les serveurs backend
- Améliore les performances en réutilisant les connexions
- Utile pour déboguer les problèmes de connexions

**Exemple :**
```yaml
env:
  - name: NGINX_UPSTREAM_KEEPALIVE
    value: "32"
```

#### `NGINX_UPSTREAM_KEEPALIVE_TIMEOUT` / `upstream-keepalive-timeout`
Définit le timeout pour les connexions keepalive.

**Valeurs possibles :**
- Durée avec unité (ex. `60s`, `5m`)

**Usage :**
- Définit combien de temps une connexion keepalive peut rester ouverte
- Utile pour optimiser les performances et déboguer les problèmes de connexions

**Exemple :**
```yaml
env:
  - name: NGINX_UPSTREAM_KEEPALIVE_TIMEOUT
    value: "60s"
```

#### `NGINX_UPSTREAM_KEEPALIVE_REQUESTS` / `upstream-keepalive-requests`
Définit le nombre maximum de requêtes par connexion keepalive.

**Valeurs possibles :**
- Nombre entier (ex. `100`, `1000`)

**Usage :**
- Définit combien de requêtes peuvent être servies par une connexion keepalive
- Utile pour équilibrer la charge et déboguer les problèmes de connexions

**Exemple :**
```yaml
env:
  - name: NGINX_UPSTREAM_KEEPALIVE_REQUESTS
    value: "100"
```

---

## Nginx Standalone

### Variables pour Nginx en mode standalone (non Ingress Controller)

#### `NGINX_DEBUG_CONNECTIONS` / `debug_connection`
Active le debug pour des connexions spécifiques.

**Valeurs possibles :**
- Adresse IP ou CIDR (ex. `127.0.0.1`, `192.168.1.0/24`)

**Usage :**
- Active le mode debug uniquement pour les connexions provenant d'adresses spécifiques
- Utile pour déboguer sans générer trop de logs
- Réduit l'impact sur les performances

**Exemple :**
```yaml
env:
  - name: NGINX_DEBUG_CONNECTIONS
    value: "127.0.0.1"
```

#### `NGINX_WORKER_PROCESSES` / `worker_processes`
Définit le nombre de processus worker Nginx.

**Valeurs possibles :**
- Nombre ou `auto` (ex. `4`, `auto`)

**Usage :**
- Contrôle le nombre de processus worker Nginx
- `auto` utilise le nombre de CPU disponibles
- Utile pour optimiser les performances

**Exemple :**
```yaml
env:
  - name: NGINX_WORKER_PROCESSES
    value: "auto"
```

#### `NGINX_WORKER_CONNECTIONS` / `worker_connections`
Définit le nombre maximum de connexions simultanées par worker.

**Valeurs possibles :**
- Nombre entier (ex. `1024`, `2048`)

**Usage :**
- Contrôle le nombre de connexions simultanées que chaque worker peut gérer
- Utile pour optimiser les performances et déboguer les problèmes de connexions

**Exemple :**
```yaml
env:
  - name: NGINX_WORKER_CONNECTIONS
    value: "1024"
```

---

## Configuration complète pour le débogage

### Configuration recommandée pour le débogage Nginx Ingress Controller

```yaml
env:
  # ============================================
  # Logging général
  # ============================================
  - name: NGINX_ERROR_LOG_LEVEL
    value: "debug"
  - name: NGINX_ACCESS_LOG
    value: "true"
  - name: NGINX_LOG_FORMAT
    value: '$remote_addr - $remote_user [$time_local] "$request" $status $body_bytes_sent "$http_referer" "$http_user_agent" $request_time $upstream_response_time $upstream_addr'

  # ============================================
  # Cache
  # ============================================
  - name: NGINX_CACHE_ENABLED
    value: "true"
  - name: NGINX_CACHE_PATH
    value: "/var/cache/nginx"
  - name: NGINX_CACHE_MAX_SIZE
    value: "1g"
  - name: NGINX_CACHE_INACTIVE
    value: "60m"
  - name: NGINX_CACHE_DEBUG
    value: "true"

  # ============================================
  # Proxy
  # ============================================
  - name: NGINX_PROXY_CONNECT_TIMEOUT
    value: "60s"
  - name: NGINX_PROXY_SEND_TIMEOUT
    value: "60s"
  - name: NGINX_PROXY_READ_TIMEOUT
    value: "60s"
  - name: NGINX_PROXY_BUFFER_SIZE
    value: "8k"
  - name: NGINX_PROXY_BUFFERS
    value: "8 8k"
  - name: NGINX_PROXY_BUFFERING
    value: "on"
  - name: NGINX_PROXY_PASS_HEADERS
    value: "true"

  # ============================================
  # SSL/TLS
  # ============================================
  - name: NGINX_SSL_VERIFY
    value: "on"
  - name: NGINX_SSL_VERIFY_DEPTH
    value: "2"
  - name: NGINX_SSL_PROTOCOLS
    value: "TLSv1.2 TLSv1.3"

  # ============================================
  # Rate Limiting
  # ============================================
  - name: NGINX_RATE_LIMIT_ENABLED
    value: "true"
  - name: NGINX_RATE_LIMIT_ZONE_SIZE
    value: "10m"
  - name: NGINX_RATE_LIMIT_RATE
    value: "10r/s"

  # ============================================
  # Load Balancing
  # ============================================
  - name: NGINX_UPSTREAM_KEEPALIVE
    value: "32"
  - name: NGINX_UPSTREAM_KEEPALIVE_TIMEOUT
    value: "60s"
  - name: NGINX_UPSTREAM_KEEPALIVE_REQUESTS
    value: "100"
```

### Configuration minimale pour le débogage

```yaml
env:
  # Logging essentiel
  - name: NGINX_ERROR_LOG_LEVEL
    value: "debug"
  - name: NGINX_ACCESS_LOG
    value: "true"
  
  # Cache debug
  - name: NGINX_CACHE_DEBUG
    value: "true"
  
  # Proxy timeouts pour voir les problèmes
  - name: NGINX_PROXY_CONNECT_TIMEOUT
    value: "60s"
  - name: NGINX_PROXY_READ_TIMEOUT
    value: "60s"
```

---

## Notes importantes

### Variables d'environnement vs Configuration Nginx

**Important :** Nginx utilise principalement des fichiers de configuration (`nginx.conf`) plutôt que des variables d'environnement. Les variables d'environnement listées ici sont généralement utilisées par :

1. **Nginx Ingress Controller** (Kubernetes/OpenShift) - qui convertit les variables d'environnement en configuration Nginx
2. **Scripts de démarrage** - qui génèrent la configuration Nginx à partir des variables d'environnement
3. **ConfigMaps Kubernetes** - qui peuvent référencer des variables d'environnement

### Nginx Ingress Controller spécifique

Pour le **NGINX Ingress Controller** officiel, certaines variables peuvent être configurées via :

1. **ConfigMap** - pour la configuration globale
2. **Annotations Ingress** - pour la configuration par ingress
3. **Variables d'environnement du Pod** - pour certaines options

### Exemple avec ConfigMap pour NGINX Ingress Controller

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: nginx-config
  namespace: ingress-nginx
data:
  error-log-level: "debug"
  enable-access-log: "true"
  proxy-connect-timeout: "60s"
  proxy-read-timeout: "60s"
  enable-cache: "true"
  cache-path: "/var/cache/nginx"
```

### Exemple avec Annotations Ingress

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/proxy-connect-timeout: "60"
    nginx.ingress.kubernetes.io/proxy-read-timeout: "60"
    nginx.ingress.kubernetes.io/enable-cache: "true"
spec:
  # ...
```

### Logs Nginx

Les logs Nginx sont généralement disponibles dans :
- **Erreurs :** `/var/log/nginx/error.log` (ou via `kubectl logs` pour les pods)
- **Accès :** `/var/log/nginx/access.log` (ou via `kubectl logs` pour les pods)

Pour les Ingress Controllers, utilisez :
```bash
# Voir les logs du Ingress Controller
kubectl logs -n ingress-nginx <ingress-controller-pod-name>

# Suivre les logs en temps réel
kubectl logs -f -n ingress-nginx <ingress-controller-pod-name>
```

### Performance

⚠️ **Attention :** Le niveau de log `debug` est très verbeux et peut :
- Consommer beaucoup d'espace disque
- Affecter les performances
- Générer des milliers de lignes par seconde

Utilisez-le uniquement pour le débogage et désactivez-le en production.

---

## Références

- [Nginx Documentation](https://nginx.org/en/docs/)
- [NGINX Ingress Controller](https://kubernetes.github.io/ingress-nginx/)
- [Nginx Configuration Guide](https://nginx.org/en/docs/http/ngx_http_core_module.html)

