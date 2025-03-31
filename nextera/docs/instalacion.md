# Proyecto FarmaOffice | Guía de instalación

## Introducción

Bienvenido a esta guía para instalar todos los proyectos del universo **FarmaOffice**.

Actualmente tenemos dos grandes conglomerados de proyectos:

### FarmaOffice (docker-environment)

- **farmaoffice**: CMS FarmaOffice y FarmaOfficeGO
- **farmaoffice-public**: E-commerce, webs públicas de farmacias
- **farmaoffice-communications**: Herramienta de mail marketing
- Diversos proyectos de backend

### Nextera

- **Nextera**: Nuevo CMS para farmaoffice-public
- **PAD**: Punto físico de asistencia en farmacia
- **APP**: Aplicación móvil
- **Extractores**: Extracción de datos de farmacias

> **IMPORTANTE**: Lee todo el documento antes de comenzar.

---

## 1. Pasos previos

Antes de seguir, completa los pasos del **Manual del Desarrollador**:

🔗 [Manual del Desarrollador](gprogramador.md)

---

## 2. Preparar entorno de proyectos

### Crear carpeta de proyectos

Se recomienda centralizar los proyectos en una carpeta:

```bash
mkdir -p ~/Projects
```

---

## 3. Levantar contenedores de bases de datos

Descargar y descomprimir el ZIP necesario desde:

🔗 [Descargar archivos](https://drive.google.com/file/d/1IOS-6-xaURcawroFqCCkRMVzxlLPMMeb/view)

Crear un archivo `.env` con:

```ini
MYSQL_USER="test"
MYSQL_PASSWORD="root"
MYSQL_DATABASE="test"
MYSQL_ROOT_PASSWORD="root"
```

Crear la red de Docker:

```bash
docker network create mysql
```

Construir y levantar los contenedores:

```bash
docker-compose build
docker-compose up -d
```

Acceder a **phpMyAdmin**:

- **MySQL 5**: [http://0.0.0.0:8085/](http://0.0.0.0:8085/)
- **MySQL 8**: [http://0.0.0.0:8088/](http://0.0.0.0:8088/)

Crear esquemas en **MySQL 5**:

- `farmaoffice`: Pide un dump a un compañero.
- `farmaoffice_testing`: Extraer esquema desde `farmaoffice` o usar esquemas de **Nextera**.

---

## 4. Docker Environment (Proyectos pre-Nextera)

### Clonar docker-environment

```bash
git clone git@bitbucket.org:bibloos/docker-environment.git
```

> **Nota**: Cambia a la rama `devDuque`.

### Levantar Docker

```bash
docker network create farmaoffice
docker-compose build
docker-compose up -d
```

Comprobar contenedores activos:

```bash
docker ps
```

---

## 5. Configuración SQL para proyectos

Ejemplo de configuración en **api-farmaoffice** (`database.php`):

```php
$db['default'] = array(
   'hostname' => 'mysql5',
   'username' => 'root',
   'password' => 'root',
   'database' => 'farmaofficego',
   'dbdriver' => 'mysqli'
);
```

Ejemplo en **farmaoffice-public** (`.env`):

```ini
DB_PORT=3305
DB_HOST=mysql5
DB_DATABASE=farmaofficego
DB_USERNAME=root
DB_PASSWORD=root
```

---

## 6. Automatización con `h.bash`

Ejecutar clonación de proyectos:

```bash
./h.bash clone --force-clone
```

Instalar dependencias:

```bash
./h.bash composer
./h.bash npm
```

Configurar hosts:

```bash
sudo ./h.bash hosts
```

Importar bases de datos:

```bash
./h.bash database
```

---

## 7. Configurar Nextera

### Clonar frontend y backend

🔗 [Nextera Frontend](https://bitbucket.org/bibloos/nextera-frontend/src/main/)

Modificar `.env` en **Nextera Backend**:

```ini
DB_CONNECTION=mysql
DB_HOST=mysql8
DB_PORT=3306
DB_DATABASE=nextera_gateway
DB_USERNAME=root
DB_PASSWORD=root
```

Ejecutar:

```bash
docker-compose build
docker-compose up -d nombre-contenedor
docker exec -ti nombre-contenedor bash
composer install
npm install
npm run build
php artisan key:generate
php artisan migrate
php artisan db:seed
```

---

## 8. Generación de usuarios y tokens

Para generar un usuario API en **Nextera Manager**:

1. Ir a `config/fortify.php` y habilitar `Feature::registration()`.
2. Registrarse en `localhost:PUERTO`.
3. Generar un **API Token** con permisos completos.

### Generación de clientes oAuth

Ejecutar:

```bash
php artisan passport:keys
php artisan passport:client
php artisan passport:client --password
```

---

## 9. Troubleshooting

### Problema con GULP en `farmaoffice-public`

🔗 [Solución en Wiki](http://wiki.farmaoffice.com/index.php?title=Webs_de_Farmaoffice#Problemes_compilaci.C3.B3_.28yargs.2C_versi.C3.B3_Node....29_.28Fedefarma_2024.29)

### Error de conexión con `api-farmaoffice`

- Verificar configuración en **Docker Networks**.
- Desconectar **VPN de Fedefarma** y reiniciar `docker-environment`.

---

## 10. Acceder al Backend de FarmaOffice

📌 **URL de login**: [http://www.farmaoffice.test/fedefarma/login](http://www.farmaoffice.test/fedefarma/login)

Usuario: `canalfarmaciaonline_fo`

Contraseña: Consulta con el equipo.

---

¡Listo! Ahora puedes trabajar con **FarmaOffice** 🚀
