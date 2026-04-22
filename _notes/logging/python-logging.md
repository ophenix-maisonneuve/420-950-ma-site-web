---
layout: default
title: "Python : logging"
nav_order: 2
parent: "Journalisation (logging)"
---

# Python : logging

Le module **logging** fait partie de la bibliothèque standard Python. Il permet de produire des logs structurés, configurables et adaptés à différents environnements.

---

## Concepts de base

Le module `logging` repose sur plusieurs concepts clés :

- **Logger** : interface utilisée par le code
- **Handler** : destination des logs (console, fichier, etc.)
- **Formatter** : format des messages
- **Niveau de log** : importance du message

---

## Exemples

### Minimal

```python
import logging

logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s %(levelname)s %(name)s - %(message)s'
)

logging.info("Application démarrée")
```

Ce code :
- configure un logging simple
- affiche les messages INFO et plus
- définit un format standard

---

### Logger différent par module

```python
import logging

logger = logging.getLogger(__name__)

logger.debug("Message de debug")
logger.info("Message d'information")
```

Avantages :
- hiérarchie des loggers
- configuration centralisée
- meilleure lisibilité

---

### Logging vers un fichier

```python
handler = logging.FileHandler('app.log')
formatter = logging.Formatter('%(asctime)s %(levelname)s %(message)s')
handler.setFormatter(formatter)

logger.addHandler(handler)
logger.setLevel(logging.INFO)
```

### Rotation des logs

**Basée sur le temps**
```python
import logging
from logging.handlers import TimedRotatingFileHandler

logger = logging.getLogger("app")
logger.setLevel(logging.INFO)

handler = TimedRotatingFileHandler(
    filename="application.log",
    when="midnight",
    interval=1,
    backupCount=7,
    encoding="utf-8"
)

formatter = logging.Formatter(
    "%(asctime)s %(levelname)s %(name)s - %(message)s"
)
handler.setFormatter(formatter)

logger.addHandler(handler)
```

**Basée sur la taille**
```python
import logging
from logging.handlers import RotatingFileHandler

logger = logging.getLogger("app")
logger.setLevel(logging.INFO)

handler = RotatingFileHandler(
    filename="application.log",
    maxBytes=10 * 1024 * 1024,  # 10 MB
    backupCount=5
)

formatter = logging.Formatter(
    "%(asctime)s %(levelname)s %(name)s - %(message)s"
)
handler.setFormatter(formatter)

logger.addHandler(handler)
```

### Filtrage des logs
Il est possible d'utiliser un `LoggerAdapter` afin de spécialiser un logger pour qu'il ajoute un marqueur à toutes ses entrées de la façon suivante :
```python
audit_logger = logging.LoggerAdapter(
    logger,
    {"deuxieme": True}
)

audit_logger.info("Connexion échouée user=dupont ip=1.2.3.4")
```

On peut ensuite configurer le logger pour séparer les logs selon leurs marqueurs :

```python
import logging

logger = logging.getLogger("app")
logger.setLevel(logging.INFO)
formatter = logging.Formatter(
    "%(asctime)s %(levelname)s %(name)s - %(message)s"
)

# Handler pour le premier fichier de logs
app_handler = logging.FileHandler("premier.log")
app_handler.setFormatter(formatter)

# Handler pour le deuxième fichier de logs
audit_handler = logging.FileHandler("deuxieme.log")
audit_handler.setFormatter(
    logging.Formatter("%(asctime)s %(levelname)s %(message)s")
)

# Filtrage
class AuditFilter(logging.Filter):
    def filter(self, record):
        return getattr(record, "deuxieme", False)

audit_handler.addFilter(AuditFilter())

# Ajout des deux handlers
logger.addHandler(app_handler)
logger.addHandler(audit_handler)
```


---

## Bonnes pratiques Python

- Un logger par module
- Configuration dans un fichier central
- Ne jamais utiliser `print()` en production
- Ne pas logger d’informations sensibles
