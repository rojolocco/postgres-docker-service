# Servicio PostgreSQL

Este directorio contiene la configuración necesaria para levantar un servicio de PostgreSQL utilizando Docker Compose. Está preparado para entornos de desarrollo y producción, con persistencia de datos y seguridad básica.

---

## **Estructura de Archivos**

```plaintext
postgres/
├── .env.dev                     # Variables de entorno para desarrollo
├── .env.prod                    # Variables de entorno para producción
├── docker-compose.dev.yml       # Docker Compose para desarrollo
├── docker-compose.yml           # Docker Compose para producción
├── data/                        # Carpeta para persistencia de datos (creada automáticamente)
```

---

## **Requisitos Previos**

1. Docker y Docker Compose instalados.
2. Una red externa llamada `caddy_network` creada previamente:

   ```bash
   docker network create caddy_network
   ```

---

## **Configuración de Variables de Entorno**

Define las credenciales y ajustes del servicio en los archivos `.env.dev` y `.env.prod`.

### `.env.dev` (Desarrollo)

```plaintext
POSTGRES_USER=dev_user
POSTGRES_PASSWORD=dev_password
POSTGRES_DB=dev_db
```

### `.env.prod` (Producción)

```plaintext
POSTGRES_USER=prod_user
POSTGRES_PASSWORD=prod_password
POSTGRES_DB=prod_db
```

> **Nota:** Asegúrate de que estos archivos estén protegidos y excluidos del control de versiones mediante `.gitignore`.

---

## **Uso**

### **Levantar el servicio en desarrollo**

```bash
docker-compose -f docker-compose.dev.yml up -d
```

### **Levantar el servicio en producción**

```bash
docker-compose -f docker-compose.yml up -d
```

### **Detener el servicio**

```bash
docker-compose down
```

### **Verificar el estado de los contenedores**

```bash
docker ps
```

---

## **Conexión a PostgreSQL**

Puedes conectarte al servicio desde herramientas externas o aplicaciones usando las siguientes credenciales:

- **Host**: `localhost` (o el nombre del servicio, si estás en la red `caddy_network`).
- **Puerto**: `5432`
- **Usuario**: Definido en `.env.dev` o `.env.prod`.
- **Contraseña**: Definida en `.env.dev` o `.env.prod`.
- **Base de datos**: Definida en `.env.dev` o `.env.prod`.

---

## **Persistencia de Datos**

Los datos de PostgreSQL se almacenan en un volumen llamado `postgres_data`. Esto asegura que los datos no se pierdan al detener o reiniciar los contenedores.

### **Ubicación del volumen:**

El volumen es gestionado por Docker y no está directamente accesible desde el sistema de archivos.

Para eliminar los datos almacenados:

```bash
docker volume rm postgres_postgres_data
```

---

## **Solución de Problemas**

### Error: `password authentication failed for user`

- Asegúrate de que las credenciales en los archivos `.env` coincidan con las utilizadas en tu aplicación.
- Verifica si el usuario existe en la base de datos. Si no, créalo manualmente:

  ```sql
  CREATE ROLE <usuario> WITH LOGIN PASSWORD '<contraseña>';
  CREATE DATABASE <base_de_datos> OWNER <usuario>;
  ```

### Error: `Role does not exist`

- Elimina el volumen y reinicia el contenedor para aplicar las variables de entorno:

  ```bash
  docker-compose down
  docker volume rm postgres_postgres_data
  docker-compose up -d
  ```

---

## **Contribución**

Si deseas mejorar este servicio o agregar nuevas funcionalidades, sigue estos pasos:

1. Haz un fork de este repositorio.
2. Realiza tus cambios en una rama nueva.
3. Envía un pull request con tus mejoras.

---

## **Licencia**

Este proyecto está bajo la licencia MIT. Consulta el archivo `LICENSE` para más detalles.
