# Despliegue backend con base datos local.
## 1. Tiempo de duración:
Tiempo estimado: 2 a 3 horas
- Clonado del proyecto: 5 min
- Configuración de archivos .env, Dockerfile y docker-compose: 30 min
- Construcción y despliegue: 30 min
- Verificación y pruebas: 1 hora
- Redacción del informe: 30 min
## 2.Fundamentos teóricos
- Docker: Herramienta de contenedorización que permite empaquetar aplicaciones con sus dependencias.
- Docker Compose: Utilidad para definir y ejecutar múltiples contenedores Docker con un solo archivo YAML.
- PostgreSQL: Sistema de gestión de bases de datos relacional y de código abierto.
- pgAdmin: Herramienta gráfica para gestionar bases de datos PostgreSQL.
- .env: Archivo que almacena variables de entorno para configuración reutilizable y segura.
- Dockerfile: Archivo que define los pasos para construir la imagen de una aplicación.
## 3. Conocimientos previos.
- Uso básico de terminal / PowerShell
- Conceptos básicos de Docker y contenedores
- Conocimientos en backend (Java, Maven, Spring Boot)
- Conocimiento de PostgreSQL
- Edición de archivos de texto (.env, .yml, Dockerfile)
## 4. Objetivos a alcanzar
- Automatizar el despliegue de una aplicación backend usando Docker Compose.
- Integrar servicios complementarios como PostgreSQL y pgAdmin.
- Usar variables de entorno para mantener una configuración flexible y segura.
- Validar la persistencia de datos mediante volúmenes en Docker.
## 5. Equipo necesario
- Computadora con:
  Docker Desktop instalado
  PowerShell o terminal
  Editor de texto (como Notepad, VS Code, Sublime, etc.)
- Conexión a internet para clonar repositorio y descargar imágenes
## 6. Material de apoyo
- Repositorio base
- Documentación oficial de Docker
- Documentación de Docker Compose
- PostgreSQL Docs
- pgAdmin Docs
## Procedimiento:
## 8. Procedimiento:
### Parte 1: Clonar repositorio desde GitHub:
```
git clone https://github.com/maguaman2/tendencias-mar22-security.git
````
### Evidencia:
<imag!![1](https://github.com/user-attachments/assets/9a9ed662-89ea-45c8-8b36-f79c22cd69ad)

```
cd tendencias-mar22-security
````
### Evidencia:
<imag!![2](https://github.com/user-attachments/assets/1fc643d0-9996-4361-8f11-b4012e0c5b49)

### Paso 2: Crear archivo docker-compose.yml con configuración:
```
notepad docker-compose.yml
````
### Evidencia:
<imag!![3](https://github.com/user-attachments/assets/2a51317e-de21-45ac-8bbc-742bc1f2413b)

```
services:
  maven-build:
    image: maven:latest
    container_name: maven-build    
    volumes:
      - .:/usr/src/mymaven
    working_dir: /usr/src/mymaven
    command: mvn clean install -DskipTests
````
### Evidencia:
<imag!![3 1](https://github.com/user-attachments/assets/742368cb-6cdc-44a0-91fb-ea2544dd77e4)

### Paso 3: El comando docker compose up sirve para levantar y ejecutar los contenedores definidos en un archivo docker-compose.yml.
```
docker compose up
````
### Evidencia:
<imag!![4](https://github.com/user-attachments/assets/87a44e5e-1ef5-4c01-bf91-910cc18e3c50)

### Paso 4: El comando docker logs maven-build sirve para ver la salida (logs) de un contenedor Docker llamado maven-build.
```
docker logs maven-build
````
### Evidencia:
<imag!![5](https://github.com/user-attachments/assets/bdaed031-6da3-43bf-86c9-25864e42a6d3)

### Evidencia:
<imag!![5 1](https://github.com/user-attachments/assets/8c54dd26-c31c-4266-bff7-a9771878ae8f)

### Paso 5: El comando ls se usa para listar los archivos y carpetas que hay en el directorio actual (o en otro que especifiques).
```
ls
````
### Evidencia:
<imag!![6](https://github.com/user-attachments/assets/3e5f7391-3541-40d4-b128-597e14d43d01)

### Paso 6: Los comandos cd target/ y ls se usan en la terminal (consola) para navegar y explorar carpetas (directorios) en un sistema de archivos.
```
cd target/
````
### Evidencia:
<imag!![7](https://github.com/user-attachments/assets/1e909dec-b3ab-4f65-8c3e-f222c7466f4b)

```
ls
````
### Paso 7: Crear archivo dockerfile con configuración:
```
notepad dockerfile
````
### Evidencia:
<imag!![8](https://github.com/user-attachments/assets/cbb31425-b145-499a-a095-4e63e52a3087)
```
# Etapa 1: Construcción con Maven
FROM maven:3.8.5-openjdk-17-slim AS build
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

# Etapa 2: Imagen final con JDK
FROM openjdk:17-jdk-slim
WORKDIR /app
COPY --from=build /app/target/*.jar app.jar
EXPOSE 8081
ENTRYPOINT ["java", "-jar", "app.jar"]
````
### Evidencia:
<imag!![8 1](https://github.com/user-attachments/assets/0154d825-b209-42d1-8c17-638e12d44113)

### Paso 8: El comando ls se usa para listar los archivos y carpetas que hay en el directorio actual (o en otro que especifiques).
```
ls
````
### Evidencia:
<imag!![9](https://github.com/user-attachments/assets/0f7a9853-b2fa-4f75-9fcb-6ad364bc2e73)

### Paso 9: El comando cd .. te mueve una carpeta hacia atrás en la estructura de directorios. 
```
cd ..
````
### Evidencia:
<imag!![0](https://github.com/user-attachments/assets/57cf2802-24a6-47d2-baf1-a2cda6d2ece5)

### Paso 10: Crear archivo docker-compose-deploy.yml con configuración:
```
notepad docker-compose-deploy.yml
````
### Evidencia:
<imag!![10](https://github.com/user-attachments/assets/bc924d06-2173-40d7-90b8-73bc97dfd904)
```
services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    image: securitybe
    container_name: securitybe
    ports:
      - "8081:8081"
    restart: always
    environment:
      - DB_SERVER=${DB_SERVER}
      - DB_PORT=${DB_PORT}
      - DB_NAME=${DB_NAME}
      - DB_USER=${DB_USER}
      - DB_PASSWORD=${DB_PASSWORD}
````

### Evidencia:
<imag!![10 1](https://github.com/user-attachments/assets/86c17431-743f-415a-b2c4-b200b7f08b48)

### Paso 11:Crear archivo .env con configuración:
```
notepad .env
````
### Evidencia:
<imag!![11](https://github.com/user-attachments/assets/e6c487ba-70f4-4b87-b22d-cb4b55b8656c)

```
DB_SERVER=postgres
DB_PORT=5432
DB_NAME=securitydb
DB_USER=admin
DB_PASSWORD=admin123
PGADMIN_DEFAULT_EMAIL=admin@admin.com
PGADMIN_DEFAULT_PASSWORD=admin123
````
### Evidencia:
<imag!![11 1](https://github.com/user-attachments/assets/12f79474-eeab-48ca-a2c0-8864d12d6cd8)

### Paso 12:  El comando ls se usa para listar los archivos y carpetas que hay en el directorio actual (o en otro que especifiques).
```
ls
````
### Evidencia:
<imag!![12](https://github.com/user-attachments/assets/f6060888-e5aa-44b0-9f2e-cee67dd0d668)

### Paso 13: Crear archivo dockerfile con configuración:
```
notepad Dockerfile
````
### Evidencia:
<imag!![13](https://github.com/user-attachments/assets/038bb6e3-1829-4b35-b701-2d7e4c386799)

```
FROM openjdk:17-alpine
ADD target/demo-0.0.1-SNAPSHOT.jar /usr/share/app.jar
ENTRYPOINT ["/usr/bin/java", "-jar", "/usr/share/app.jar"]
````
### Evidencia:
<imag!![13 1](https://github.com/user-attachments/assets/d516b876-5029-4786-af6d-2dab5e2cea93)

### Paso 14: Sirve para mover o renombrar un archivo llamado Dockerfile.
```
mv Dockerfile
````
### Evidencia:
<imag!![14](https://github.com/user-attachments/assets/1b1b6794-5a44-4728-883f-b1b66e4302cc)

### Paso 15:Ejecutar construcción y despliegue:
```
docker compose -f docker-compose-deploy.yml up --build
````
### Evidencia:
<imag!![15](https://github.com/user-attachments/assets/11524a75-449a-4ccd-976a-49d84574ec1b)

### Paso 16: Muestra una salida del proceso de construcción de una imagen Docker en la terminal.
### Evidencia:
<imag!![15 1](https://github.com/user-attachments/assets/13fbac93-db77-42bf-a53f-652ac5ab7a8a)

### Evidencia:
<imag!![15 2](https://github.com/user-attachments/assets/925da39d-5863-437e-998e-48801e29580f)

### Paso 17: erificar acceso:
- http://localhost:8081 → aplicación
- http://localhost:5050 → pgAdmin
## Resultados esperados: 
- Aplicación backend funcionando en el puerto 8081
- pgAdmin accesible desde navegador en el puerto 5050
- PostgreSQL correctamente configurado con persistencia de datos
- Comunicación exitosa entre contenedores mediante red Docker interna
### Evidencia:
<imag!![15 3](https://github.com/user-attachments/assets/efa664cf-a498-437b-915e-a6f9093ad7f9)
## Conclusión:
El uso de Docker y Docker Compose permite desplegar entornos complejos de forma sencilla y reproducible. Mediante esta práctica, se consolidaron conocimientos en contenedores, integración de servicios, automatización con archivos YAML, y despliegue de aplicaciones Java. Además, se validó la importancia de la persistencia de datos usando volúmenes, lo cual es clave en aplicaciones reales.
## Bibliografía
- Docker Docs: https://docs.docker.com/
- Docker Compose Docs: https://docs.docker.com/compose/
- PostgreSQL: https://www.postgresql.org/docs/
- pgAdmin Docs: https://www.pgadmin.org/docs/
- GitHub proyecto base: https://github.com/maguaman2/tendencias-mar22-security









