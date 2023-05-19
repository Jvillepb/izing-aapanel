# INSTALACIÓN IZING CON AAPANEL
Cómo instalar Izing con AAPANEL, según el tutorial de "Canal eConhecimento" en YouTube
-------------------------------------------

▶️ UBUNTU 22.04

#### ▶️ COMANDOS PARA PREPARAR E INSTALAR LO NECESARIO EN EL SERVIDOR

```
timedatectl set-timezone America/Bogota && apt update && apt upgrade -y && apt install -y libgbm-dev wget unzip fontconfig locales gconf-service libasound2 libatk1.0-0 libc6 libcairo2 libcups2 libdbus-1-3 libexpat1 libfontconfig1 libgcc1 libgconf-2-4 libgdk-pixbuf2.0-0 libglib2.0-0 libgtk-3-0 libnspr4 libpango-1.0-0 libpangocairo-1.0-0 libstdc++6 libx11-6 libx11-xcb1 libxcb1 libxcomposite1 libxcursor1 libxdamage1 libxext6 libxfixes3 libxi6 libxrandr2 libxrender1 libxss1 libxtst6 ca-certificates fonts-liberation libappindicator1 libnss3 lsb-release xdg-utils python2-minimal build-essential postgresql redis-server && add-apt-repository -y ppa:rabbitmq/rabbitmq-erlang && wget -qO - https://packagecloud.io/install/repositories/rabbitmq/rabbitmq-server/script.deb.sh | sudo bash && apt install -y rabbitmq-server && rabbitmq-plugins enable rabbitmq_management && wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && apt install -y ./google-chrome-stable_current_amd64.deb && rm -rf google-chrome-stable_current_amd64.deb && wget -O install.sh http://www.aapanel.com/script/install-ubuntu_6.0_en.sh && bash install.sh aapanel && rm -rf install.sh && reboot
```


❗❗❗ La máquina se reiniciará.

#### ▶️ ENTRAR A AAPANEL PARA CONFIGURARLO + POSTGRESQL + REDIS + RABBITMQ.

##### 🔹 Después de instalar Nginx 1.21 y hacer las configuraciones en "Settings", instalar:

- PM2 Manager
- RabbitMQ
- Postgres
- Redis

##### 🔹 Instalar versión 14.21.1 de Node.
##### 🔹 Configurar versión 14.21.1 de Node en PM2 Manager.

##### 🔹 Ir a la configuración del Firewall del Sistema y abrir los puertos:
> 5432 (PostgreSQL)
> 6379 (Redis)
> 5672 (RabbitMQ)
> 8081 (Proxy Backend)

##### 🔹 Editar archivos:
➥ /etc/postgresql/14/main/postgresql.conf

##### Modificar esta línea:
```
#listen_addresses = 'localhost'
```

##### Así👇
```
listen_addresses = '*'
```
##### Ahora, dentro de

➥ /etc/postgresql/14/main/pg_hba.conf

##### Modificar esta línea:
```
host all all 127.0.0.1/32
```
##### Así👇
```
host all all 0.0.0.0/0
```
##### Dentro de ➥ /etc/redis/redis.conf

##### Modificar esta línea:
```
#requirepass [Acá va la contraseña]
```

##### Así👇
```
requirepass {Contraseña que quieras}
```

##### 🔹 Configurar PostgreSQL en el Terminal:
#### Entrar a psql
```
sudo -u postgres psql
```
##### Crear usuario postgres y su contraseña
```
ALTER USER postgres PASSWORD '{Contraseña para PostgreSQL}';
```
##### Salir
```
\q
```
##### Para salir de Postgres, también puedes usar "Ctrl + D"

##### Luego volver a entrar:
```
sudo -u postgres psql
```
##### Crear base de datos "izing"
```
CREATE DATABASE izing;
```
##### Salir
```
\q
```

🔹 Configurar RabbitMQ en el Terminal:
rabbitmqctl add_user admin 123456
rabbitmqctl set_user_tags admin administrator
rabbitmqctl set_permissions -p / admin "." "." ".*"

❗❗❗Sería bueno reiniciar aquí por prevención (Opcional)

▶️ INSTALACIÓN DE IZING:

🔹 Clonar repositorio:
git clone https://github.com/ldurans/izing.io.git /www/wwwroot

🔹 Editar .env del backend, cambiándole el nombre de ".env.example" a ".env"
Así👇

▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ 

NODE_ENV=dev
BACKEND_URL={IP o Dominio para el Backend}
FRONTEND_URL={IP o Dominio para el Frontend}

PROXY_PORT=443
PORT=8081

DB_DIALECT=postgres
DB_PORT=5432
POSTGRES_HOST=localhost
POSTGRES_USER=postgres
POSTGRES_PASSWORD={Contraseña que se puso antes al configurar PostgreSQL}
POSTGRES_DB=izing

JWT_SECRET=DPHmNRZWZ4isLF9vXkMv1QabvpcA80Rc
JWT_REFRESH_SECRET=EMPehEbrAdi7s8fGSeYzqGQbV5wrjH4i

IO_REDIS_SERVER=localhost
IO_REDIS_PASSWORD={Contraseña que se puso antes al configurar Redis}
IO_REDIS_PORT='6379'
IO_REDIS_DB_SESSION='2'

CHROME_BIN=/usr/bin/google-chrome-stable




RABBITMQ_DEFAULT_USER=admin
RABBITMQ_DEFAULT_PASS=123456
AMQP_URL='amqp://admin:123456@localhost:5672?connection_attempts=5&retry_delay=5'

# API OFICIAL (INTEGRAÇÃO EM DESENVOLVIMENTO)
API_URL_360=https://waba-sandbox.360dialog.io

# DADOS PARA UTILIZAÇÃO DO CANAL DO FACEBOOK
FACEBOOK_APP_ID=3237415623048660
FACEBOOK_APP_SECRET_KEY=3266214132b8c98ac59f3e957a5efeaaa13500

▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ 

▶️ EJECUTAR BACKEND.

❗❗❗ Asegúrese de haber creado el banco de datos anteriormente!

🔹 Ejecutar comandos dentro del Backend:

> npm install 
> npm run build 
> npx sequelize db:migrate 
> npx sequelize db:seed:all

▶️ EJECUTAR FRONTEND.

🔹 Editar .env del Frontend, cambiándole el nombre de ".env.example" a ".env"
Así👇

▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ ▼ 

URL_API={IP o Dominio para el Frontend}
FACEBOOK_APP_ID='23156312477653241'

▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ ▲ 

🔹 Ejecutar comandos:

> npm i -g @quasar/cli
> npm install
> quasar build -P -m pwa



▶️ CONFIGURAR PM2

> pm2 startup ubuntu -u root

▶️ MAPEAR INSTANCIAS DE BACKEND Y FRONTEND

🔹 Rutas de PM2 MANAGER en AAPANEL:

➥ BACKEND:
Startup file: /wwwroot/izing.io/backend/dist/server.js
Run dir: /wwwroot/izing.io/backend

🔹 Rutas al SITIO en AAPANEL:
➥ FRONTEND: /www/izing.io/frontend/dist/pwa
