# api-gateway-grupo10

> **EduSmart Campus – Microservicios con Spring Cloud** · Laboratorio T2 (Fila B) · Grupo 10
> Java 25 · Spring Boot 4.1.1 · Spring Cloud 2025.1.3 · Maven

**Punto de entrada único** con Spring Cloud Gateway (Pregunta 1). Se registra en Eureka y enruta con balanceo de carga (`lb://`), sin URLs fijas.

| Dato | Valor |
|---|---|
| Puerto | `8070` |
| Nombre en Eureka | `API-GATEWAY-GRUPO10` |
| Dependencias | Reactive Gateway (`spring-cloud-starter-gateway-server-webflux`), Eureka Discovery Client |

## Rutas

| Ruta entrante | Destino |
|---|---|
| `http://localhost:8070/api/v1/matriculas/**` | `lb://MATRICULA-SERVICE-GRUPO10` |
| `http://localhost:8070/api/v1/cursos/**` | `lb://CURSO-SERVICE-GRUPO10` |

```properties
spring.cloud.gateway.server.webflux.routes[0].id=matricula-service
spring.cloud.gateway.server.webflux.routes[0].uri=lb://MATRICULA-SERVICE-GRUPO10
spring.cloud.gateway.server.webflux.routes[0].predicates[0]=Path=/api/v1/matriculas/**

spring.cloud.gateway.server.webflux.routes[1].id=curso-service
spring.cloud.gateway.server.webflux.routes[1].uri=lb://CURSO-SERVICE-GRUPO10
spring.cloud.gateway.server.webflux.routes[1].predicates[0]=Path=/api/v1/cursos/**
```

- Desde Spring Cloud 2025 el prefijo es `spring.cloud.gateway.server.webflux.*`.
- No se usa `StripPrefix`: los controllers ya exponen el path completo `/api/v1/...`.

## Ejecutar

```bash
./mvnw spring-boot:run      # levantarlo al final; esperar ~30 s a que Eureka propague los registros
```

Si responde **503**, el servicio destino aún no aparece en Eureka: esperar unos segundos.
