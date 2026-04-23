---
layout: default
title: "Java : Log4j2"
nav_order: 1
parent: "Journalisation (logging)"
---

# Java : Log4j2

Log4j2 est l’un des frameworks de logging les plus utilisés dans l’écosystème Java. Il offre de hautes performances, une grande flexibilité de configuration et une séparation claire entre logique applicative et journalisation.

---

## Concepts de base

Avant de configurer Log4j2, il est essentiel de comprendre ses concepts clés.

- **Logger** : point d’entrée utilisé par le code pour écrire des logs
- **Appender** : destination des logs (fichier, console, réseau, etc.)
- **Layout** : format de sortie des messages
- **Niveau de log** : seuil de gravité à partir duquel les messages sont enregistrés

---

## Configuration

Un fichier `log4j2.xml` simple :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
  <Appenders>
    <Console name="Console" target="SYSTEM_OUT">
      <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss} %-5level %c - %msg%n" />
    </Console>
  </Appenders>

  <Loggers>
    <Root level="info">
      <AppenderRef ref="Console" />
    </Root>
  </Loggers>
</Configuration>
```

Ce fichier configure :
- un appender console
- un format lisible
- un niveau global INFO

---

## Code Java

### Utilisation directe

```java
import org.apache.logging.log4j.LogManager;
import org.apache.logging.log4j.Logger;

public class ExampleService {
    private static final Logger LOGGER = LogManager.getLogger(ExampleService.class);

    public void process(String input) {
        LOGGER.info("Traitement de la valeur : {}", input);
    }
}
```
### Utilisation via `SLF4J`
`SLF4J` est une couche d'abstraction très populaire en Java qui uniformise l'API de journalisation dans le code tout en permettant l'utilisation transparente de la plupart des *frameworks* de journalisation. Ainsi, il est possible d'utiliser `Log4j2`, `Logback`, `java.util.Logging` et plusieurs autres implémentations tout en les découplant du code appelant. Spring Boot, entre autres, suggère fortement cette approche, qui se traduit par un code légèrement différent :

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class ExampleService {
    private static final Logger logger = LoggerFactory.getLogger(ExampleService.class);

    public void process(String input) {
        logger.info("Traitement de la valeur : {}", input);
    }
}
```

Bonnes pratiques :
- un logger par classe
- utilisation des paramètres `{}`
- éviter la concaténation de chaînes

---

## Autres exemples de configurations

### Rotation de fichiers

```xml
<RollingFile name="FileAppender"
             fileName="logs/app.log"
             filePattern="logs/app-%d{yyyy-MM-dd}-%i.log">
  <PatternLayout pattern="%d %-5level %c - %msg%n" />
  <Policies>
    <!-- Par défaut, rotation quotidienne mais on peut paramétrer avec "interval" (en jours) et "modulate" pour alignement-->
    <TimeBasedTriggeringPolicy />

    <!-- On peut aussi ajouter une rotation basée sur la taille-->
    <SizeBasedTriggeringPolicy size="10 MB"/>
  </Policies>

  <!-- On conserve un historique de 30 fichiers -->
  <DefaultRolloverStrategy max="30"/>
</RollingFile>
```


Cette configuration :
- écrit dans un fichier
- crée un nouveau fichier chaque jour OU lorsque le fichier a atteint 10MB
- évite la croissance infinie des logs en conservant seulement les 30 derniers fichiers

### Filtrage sur des marqueurs

Il est possible d'ajouter un marqueur à une entrée de log de la façon suivante :

```java
Marker audit = MarkerFactory.getMarker("AUDIT");
LOGGER.info(audit, "message avec parametre {} ", "valeurDuParametre");
```

Log4j2 peut ensuite filtrer sur le marqueur afin de le rediriger vers une cible particulière :

```xml
<Filters>
    <MarkerFilter
            marker="AUDIT"
            onMatch="ACCEPT"
            onMismatch="DENY"/>
</Filters>
```

---

## Bonnes pratiques Log4j2

- Séparer logs applicatifs et logs d’audit
- Adapter les niveaux selon l’environnement (DEV vs PROD)
- Ne jamais logger de données sensibles
- Surveiller les volumes générés
