# 📦 LAB10 — React + Django REST + JWT + VPS Deploy

---

# 🚀 Arquitectura del Proyecto

<pre>
React (Frontend)
   ↓ Axios HTTP
Django REST Framework (Backend)
   ↓
JWT Authentication (SimpleJWT)
   ↓
MySQL / MariaDB
</pre>

## ⚙️ PASO 0 — SETUP DEL SERVIDOR
<pre>
mkdir lab10
cd lab10
mkdir backend
cd backend

python3 -m venv venv
source venv/bin/activate

sudo apt update
sudo apt install python3 python3-pip python3-venv nginx git curl -y

curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
sudo apt install nodejs -y

node -v
npm -v
</pre>

## 🧠 PASO 1 — BACKEND (DJANGO REST)
<pre>
pip install django djangorestframework django-cors-headers gunicorn mysqlclient

django-admin startproject backend
cd backend

python manage.py startapp productos
</pre>

### 🐬 BASE DE DATOS (MySQL)
<pre>
sudo apt install default-libmysqlclient-dev build-essential -y
mysql -u root -p
</pre>
Crear la base de datos y el usuario 
<pre>
CREATE DATABASE DAE_lab10;
CREATE USER 'django_user'@'localhost' IDENTIFIED BY 'Tecsup123';
GRANT ALL PRIVILEGES ON DAE_lab10.* TO 'django_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
</pre>
### ⚙️ DJANGO SETTINGS
<pre>
ALLOWED_HOSTS = ['tu_dominio']

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'rest_framework',
    'corsheaders',
    'productos',
]

CORS_ALLOW_ALL_ORIGINS = True

LANGUAGE_CODE = 'es'
TIME_ZONE = 'America/Lima'
USE_TZ = True
</pre>
