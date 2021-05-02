---
author: "Francisco Ruiz"
title: "URL Shortener"
date: "Mar 10, 2021 9:42 AM"
tags: ["Java", "DDD", "Clean Architecture", "Domain Events", "TDD", "Tests","MySQL", "Redis"]
ShowToc: false
ShowBreadCrumbs: false
---
# 🎯 Objetivos

- Dada una URL larga, mi servicio me tiene que devolver una URL corta.
- Puedan obtenerse estadísticas de las URLs que utilizan este servicio.
- Puedan manejarse solicitudes a gran escala.
- Puedan borrarse las URLs cortas necesarias.
- Y lógicamente, que el usuario navegue hacia la URL larga cuando ingresa una url
corta válida en su navegador

# 🤔 Problemas que se nos presentan

- Términos a gran escalabilidad → 50.000 peticiones por segundo

# 🏗️ Arquitectura

## 🧿 Clean Architecture

- **Capa de Infraesructura**: La capa más externa, será la encargada de tocar Input/Output y conectar con servicios externos al sistema (Controllers, acceso a BD…)
- **Capa de Aplicación**: Será la que contenga los distintos casos de uso de la aplicación (entendiendo los casos de uso desde el punto de vista de los usuarios) y que comunique la capa anterior con la de Dominio
- **Capa de Dominio**: Recoge nuestras entidades

![Clean Architecture](/url-shortener/6.png)

## 🕐 CQRS

El enfoque principal que la gente usa para interactuar con un sistema de información es tratarlo como un almacén de datos CRUD

**Pros:**

- A medida que nuestro software crece y es mas sofisticado, este patron nos abstrae cada vez mas del Domino y solo nos comunicamos con la capa de aplicación

**Contras:**

- Complejo de implementar

 

![CQRS](/url-shortener/1.png)

## 🤩 Testing

### Estrategia de tests

Así, el flujo de una petición normal podría ser:

1. Entrada de la petición al Controller
2. El Controller invocaría al Application Service
3. El Application Service orquesta llamadas a diferentes servicios de dominio, modelos de dominio, repositorios…
4. Las interfaces de repositorios, buses… son implementadas por la infraestructura (Ej. repositorio en MySQL)

![Tests strategies](/url-shortener/5.png)

- **Test Unitario**: Testean el caso de uso (1 test por caso de uso) desde el servicio de aplicación hasta las interfaces, no incluirá por tanto las implementaciones de infraestructura
- **Test de Integración**: Testean la interacción (integración) con la implementación de la infraestructura (Ej. repositorio en MySQL)
- **Test de Aceptación**: Testea el flujo complejo desde el protocolo de comunicación usado por los usuarios

**Pirámide de tests**

![Tests](/url-shortener/2.png)

### Patrones de diseño aplicados a tests

- ObjectMother
- Fakes
- Mocks

## 🙅‍♂️ Use cases

**Bounded Context**: Core

```bash
.
├── analytics
│   ├── application
│   │   ├── AnalyticResponse.java
│   │   ├── AnalyticsResponse.java
│   │   ├── create
│   │   │   ├── AnalyticCreator.java
│   │   │   ├── CreateAnalyticCommandHandler.java
│   │   │   └── CreateAnalyticCommand.java
│   │   └── search_by_criteria
│   │       ├── AnalyticsByCriteriaSearcher.java
│   │       ├── SearchAnalyticsByCriteriaQueryHandler.java
│   │       └── SearchAnalyticsByCriteriaQuery.java
│   ├── domain
│   │   ├── AnalyticId.java
│   │   ├── Analytic.java
│   │   ├── AnalyticLatitude.java
│   │   ├── AnalyticLongitude.java
│   │   ├── AnalyticNotExist.java
│   │   └── AnalyticRepository.java
│   └── infrastructure
│       └── persistence
│           ├── hibernate
│           │   └── Analytic.hbm.xml
│           └── HibernateAnalyticRepository.java
├── localization
│   ├── application
│   │   ├── find_by_ip_address
│   │   │   ├── FindLocationByIpAddressQueryHandler.java
│   │   │   ├── FindLocationByIpAddressQuery.java
│   │   │   └── LocationByIpAddressFinder.java
│   │   └── LocalizationResponse.java
│   ├── domain
│   │   ├── IpAddressLocation.java
│   │   ├── Localization.java
│   │   └── LocationDomainByIpAddressFinder.java
│   └── infrastructure
│       └── GeoIpService.java
├── shared
│   └── instructure
│       └── persistence
│           ├── CoreHibernateConfiguration.java
│           └── CoreRedisConfiguration.java
├── social_medias
│   └── domain
│       ├── SocialMediaId.java
│       └── SocialMedia.java
└── urls
    ├── application
    │   ├── create
    │   │   ├── CreateUrlCommandHandler.java
    │   │   ├── CreateUrlCommand.java
    │   │   └── UrlCreator.java
    │   ├── delete
    │   │   ├── DeleteUrlCommandHandler.java
    │   │   ├── DeleteUrlCommand.java
    │   │   └── UrlDeletor.java
    │   ├── find
    │   │   ├── FindUrlByIdQueryHandler.java
    │   │   ├── FindUrlByIdQuery.java
    │   │   └── UrlFinder.java
    │   ├── find_all
    │   │   ├── AllUrlsSearcher.java
    │   │   ├── FindAllUrlsQueryHandler.java
    │   │   └── FindAllUrlsQuery.java
    │   ├── identifier_generate
    │   │   ├── GenerateIdentifierQueryHandler.java
    │   │   ├── GenerateIdentifierQuery.java
    │   │   └── IdentifierGenerator.java
    │   ├── IdentifierResponse.java
    │   ├── update
    │   │   ├── UpdateUrlCommandHandler.java
    │   │   ├── UpdateUrlCommand.java
    │   │   └── UrlUpdater.java
    │   ├── UrlResponse.java
    │   └── UrlsResponse.java
    ├── domain
    │   ├── AlgorithmIdentifierGenerator.java
    │   ├── UrlDomainFinder.java
    │   ├── UrlExists.java
    │   ├── UrlId.java
    │   ├── Url.java
    │   ├── UrlNotAvailable.java
    │   ├── UrlNotExist.java
    │   ├── UrlNotValid.java
    │   ├── UrlRepository.java
    │   └── UrlUri.java
    └── infrastructure
        ├── persistence
        │   ├── hibernate
        │   │   └── Url.hbm.xml
        │   ├── HibernateUrlRepository.java
        │   └── RedisUrlRepository.java
        └── UuidAlgorithmIdentifierGenerator.java
```

# 🔑 Identificadores de recursos desde fuera

## Posibles responsables para generar el identificador

### Repositories 🥉

**Contras:**

- **Delegamos la responsabilidad de gestionar los identificadores a un componente de infraestructura**. Desde el momento en el que movemos esto a infraestructura, sería un aspecto más a tener en cuenta a la hora de cambiar este componente. Con lo cuál, dificultaría la transición de un adaptador a otro.
- **Representaciones de recursos inconsistentes en cliente**. El cliente nos hace la petición sin el identificador, ya que es el backend quien general el ID. Esto implica que el cliente permita que el ID sea nulo en ciertos momentos.
- **Complejidad adicional en testing**. Desde el momento en el que la generación de identificadores no es determinista, no podemos comparar que todos los atributos de nuestros recursos sean iguales. Lo que deberemos hacer será comprobar que todos los atributos, a excepción del identificador, coinciden con los del recurso que le hemos pedido al sistema que cree.

### Application Services / Use cases 🥈

**Pros:**

- Ahora ya no tenemos el primero de los contras anteriores. **Podemos cambiar la infraestructura manteniendo la lógica de gestión de identificadores**.

**Contras:**

- **Seguimos teniendo los problemas que analizábamos antes en términos de inconsistencia en clientes, y complejidad de testing**. No obstante, sí que es cierto que en este caso podríamos encapsular la lógica de generación de IDs en un servicio para poder hacer un doble de test y que éste fuera determinista. No obstante, estaríamos añadiendo una complejidad tanto al testing como al código de producción que nos podemos ahorrar.

### Clients 🥇

**Pros:**

- **Mantenimiento y cambiabilidad**: Podemos cambiar cualquier componente de infraestructura que ello no implicará tener que replicar la lógica de gestión de identificadores, ni tenerla duplicada entre todos los adaptadores.
- **Testing**: Ya podemos comparar la entidad que le pedimos crear a nuestro sistema de forma íntegra (sin obviar el atributo de ID).
- **Integridad de representación de recursos en cliente**: El cliente va a poder establecer el atributo del identificador como obligatorio (no null) ya que en todo momento va a disponer de éste.

# 🥳 Soluciones a algunos de nuestros problemas

## 🏭 Infrastructure

- Java - Spring Boot
- MySQL

## 💨 Optimizando rendimiento - Cache en el servidor

A la hora de plantearnos mejorar el rendimiento de nuestra aplicación una de las opciones con las que contamos es la del caché de cliente (por ej: ETag) o en el lado del servidor. 

A nivel de Servidor también contamos con distintas alternativas que podríamos diferenciar en base a dónde las ubicaríamos y, por supuesto, cada una de ellas nos ofrecerá diferentes ventajas e inconvenientes

1. **Caché delante del Controller**
2. **Caché dentro del Application Service** 🥉 → Esta alternativa podría ser llevar la caché dentro del propio application service. Tergiversamos el Business logic? Y la S de SOLID?
3. **Caché delante del Application Service** 🥈 **** → Podremos llevar nuestra gestión de la caché a un **Middleware** entre el controller y el Application Service donde se tendrá cacheado el resultado
4. **Caché en la capa de Infraestructura** 🥇 → Aqui supondría llevar esta lógica a la propia capa de infraestructura

En cuanto a infraestructura → **REDIS** al rescate

![MySQL and REDIS 1](/url-shortener/3.png)

![MySQL and REDIS 2](/url-shortener/4.png)

# 🌀 Futuras iteraciones y mejoras

- Limpiando deuda técnica: Separando Bounded Contexts: Analytics → Eventos de Dominio y Bus de eventos
- Logging y Monitoring
- Continuous Deployment
