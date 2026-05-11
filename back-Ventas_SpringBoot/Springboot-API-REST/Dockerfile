# ETAPA 1: Construcción (Maven)
FROM maven:3.9-eclipse-temurin-21-alpine AS build-stage
WORKDIR /app
# Copiamos el pom.xml y descargamos dependencias primero (Optimización de capas)
COPY pom.xml .
RUN mvn dependency:go-offline -B
# Copiamos el código fuente y compilamos
COPY src ./src
RUN mvn clean package -DskipTests

# ETAPA 2: Producción (JRE ligero y seguro)
FROM eclipse-temurin:21-jre-alpine
# Aplicando principio de mínimo privilegio: usuario no-root
RUN addgroup -S innovatech && adduser -S scott -G innovatech
USER scott

WORKDIR /app
# Copiamos el .jar generado en la etapa anterior
COPY --from=build-stage /app/target/*.jar app.jar

# Spring Boot usará el puerto que le pasemos por defecto o por properties
CMD ["java", "-jar", "app.jar"]
