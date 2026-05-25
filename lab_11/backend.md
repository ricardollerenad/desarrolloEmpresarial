# 🚀 Tutorial Completo — Backend con Django + DRF + MySQL + Gunicorn + NGINX

> 📘 Guía paso a paso orientada a estudiantes y desarrolladores que desean aprender cómo construir una API REST profesional usando Django REST Framework.

---

# 📚 Objetivos del laboratorio

Al finalizar este tutorial podrás:

- ✅ Crear un entorno virtual profesional
- ✅ Instalar Django y Django REST Framework
- ✅ Crear una API REST
- ✅ Conectarte a MySQL
- ✅ Configurar Django Admin
- ✅ Desplegar el proyecto con Gunicorn
- ✅ Configurar NGINX como proxy reverso
- ✅ Ejecutar el backend en producción

---

# 🧩 PARTE 1 — SETUP BACKEND

---

# 🟢 Paso 1 — Instalar Dependencias

## 📌 ¿Por qué usamos un entorno virtual?

El entorno virtual permite aislar las dependencias del proyecto para evitar conflictos con otros proyectos Python instalados en el sistema.

---

## 🔹 Crear entorno virtual

```bash
python3 -m venv venv
source venv/bin/activate
```

### ✅ Explicación

| Comando | Función |
|---|---|
| `python3 -m venv venv` | Crea un entorno virtual |
| `source venv/bin/activate` | Activa el entorno virtual |

---

## 🔹 Instalar Django y DRF

```bash
pip install django djangorestframework django-cors-headers
```

### ✅ Explicación

| Librería | Función |
|---|---|
| Django | Framework backend |
| djangorestframework | Permite crear APIs REST |
| django-cors-headers | Permite conexión con frontend |

---

# 🟢 Paso 2 — Crear Proyecto Django

```bash
django-admin startproject backend
cd backend
python manage.py startapp tareas
```

---

## 📌 Explicación

### `startproject`
Crea el proyecto principal.

### `startapp`
Crea un módulo independiente dentro del proyecto.

---

# 🧠 Estructura esperada

```text
backend/
 ├── backend/
 ├── tareas/
 ├── manage.py
```

---

# 🟢 Paso 3 — Configurar settings.py

## 📄 backend/settings.py

Agregar aplicaciones:

```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'rest_framework',
    'corsheaders',
    'tareas',
]

MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

CORS_ALLOW_ALL_ORIGINS = True
```

---

# 📌 Explicación

## INSTALLED_APPS
Aquí Django registra todos los módulos que estarán disponibles.

## MIDDLEWARE
Interceptan solicitudes HTTP antes de llegar a Django.

## CORS
Permite que React o Vue puedan consumir el backend.

---

# ✅ Checkpoint 1

```bash
python manage.py runserver 0.0.0.0:8000
```

---

# 🚀 RETO 1 — API Básica de Tareas

---

# 🟢 Paso 5 — Preparar Base de datos

## 🔹 Crear base de datos + Usuario

```sql
CREATE DATABASE lab11 CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

CREATE USER 'django_rllerena'@'localhost' IDENTIFIED BY 'Tecsup123';

GRANT ALL PRIVILEGES ON lab11.* TO 'django_rllerena'@'localhost';

FLUSH PRIVILEGES;

EXIT;
```

---

# 🟢 Instalando librerías

```bash
sudo apt install default-libmysqlclient-dev build-essential pkg-config -y

pip install django mysqlclient
```

## ⚠️ Si falla mysqlclient

```bash
pip install pymysql
```

---

# 🟢 Configuración de Django con MySQL

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'django_db',
        'USER': 'django_user',
        'PASSWORD': 'ClaveSegura123!',
        'HOST': '127.0.0.1',
        'PORT': '3306',
    }
}
```

---

# 🟢 Paso 6 — Crear Modelo

## 📄 tareas/models.py

```python
from django.db import models

class Tarea(models.Model):
    titulo = models.CharField(max_length=100)
    completada = models.BooleanField(default=False)
    creada = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.titulo
```

---

# 📌 Explicación

## Modelos
Los modelos representan tablas dentro de la base de datos.

| Campo | Tipo |
|---|---|
| titulo | Texto |
| completada | Boolean |
| creada | Fecha automática |

---

# 🟢 Migraciones

```bash
python manage.py makemigrations
```

---

# 🟢 Paso 6 — Activar API REST

## 📄 serializers.py

```python
from rest_framework import serializers
from .models import Tarea

class TareaSerializer(serializers.ModelSerializer):
    class Meta:
        model = Tarea
        fields = '__all__'
```

---

## 📄 views.py

```python
from rest_framework import viewsets
from .models import Tarea
from .serializers import TareaSerializer

class TareaViewSet(viewsets.ModelViewSet):
    queryset = Tarea.objects.all().order_by('-creada')
    serializer_class = TareaSerializer
```

---

## 📄 urls.py

```python
from rest_framework.routers import DefaultRouter
from .views import TareaViewSet

router = DefaultRouter()
router.register(r'tareas', TareaViewSet)

urlpatterns = router.urls
```

---

## 📄 backend/urls.py

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('tareas.urls')),
]
```

---

## 📄 tareas/admin.py

```python
from django.contrib import admin
from .models import Tarea

admin.site.register(Tarea)
```

---

# 🔄 Realizar migraciones nuevamente

```bash
python manage.py makemigrations
python manage.py migrate
```

---

# 👤 Crear usuario administrador

```bash
python manage.py createsuperuser
```

---

# ✅ Checkpoint 2

Debemos poder ingresar al panel admin y administrar tareas.

```bash
python manage.py runserver 0.0.0.0:4200
```

---

# 📌 Endpoint esperado

```text
TU_IP/api/tareas/
```

---

# 🚀 Paso 7 — Ejecutarlo en segundo plano con Gunicorn

## 🔹 Instalar Gunicorn

```bash
pip install gunicorn
```

---

## 🔹 Probar Gunicorn

```bash
gunicorn backend.wsgi:application --bind 127.0.0.1:4200
```

---

# 🟢 Configuración producción en settings.py

```python
ALLOWED_HOSTS = ["django.arequipa.site"]

STATIC_URL = "/static/"
STATIC_ROOT = BASE_DIR / "staticfiles"

MEDIA_URL = "/media/"
MEDIA_ROOT = BASE_DIR / "media"

CORS_ALLOWED_ORIGINS = [
    "http://django.arequipa.site",
]
```

---

# 🟢 Crear servicio systemd

```bash
sudo nano /etc/systemd/system/web_django.service
```

---

## 📄 Servicio Gunicorn

```ini
[Unit]
Description=Gunicorn Django Service
After=network.target

[Service]
User=ricardo
Group=www-data
WorkingDirectory=/home/ricardo/tecsup/desarrolloEmpresarial/lab11/backend

ExecStart=/home/ricardo/tecsup/desarrolloEmpresarial/lab11/venv/bin/gunicorn backend.wsgi:application \
--bind 127.0.0.1:4200 \
--workers 3

Restart=always

[Install]
WantedBy=multi-user.target
```

---

# 🟢 Activar servicio

```bash
sudo systemctl daemon-reload
sudo systemctl start web_django.service
sudo systemctl status web_django.service
sudo systemctl enable web_django.service
```

---

# 🔍 Verificar servicio

```bash
sudo systemctl status web_django
```

---

# 🌐 Configuración NGINX

```nginx
server {
    listen 80;
    server_name django.arequipa.site;

    root /home/ricardo/tecsup/desarrolloEmpresarial/lab11/frontend/dist;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    location /api/ {
        proxy_pass http://127.0.0.1:4200;

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    location /static/ {
        alias /home/ricardo/tecsup/desarrolloEmpresarial/lab11/backend/staticfiles/;
    }

    location /media/ {
        alias /home/ricardo/tecsup/desarrolloEmpresarial/lab11/backend/media/;
    }
}
```

---

# 📌 Explicación de NGINX

| Sección | Función |
|---|---|
| `/` | Sirve React |
| `/api/` | Redirige a Django |
| `/static/` | Archivos estáticos |
| `/media/` | Archivos subidos |

---

# ✅ Checkpoint 3

Probar API desde terminal:

```bash
curl https://django.arequipa.site/api/tareas/
```

---

# 🎯 Resultado Final

Al finalizar tendrás:

✅ Backend Django funcionando  
✅ API REST funcional  
✅ MySQL conectado  
✅ Django Admin operativo  
✅ Gunicorn ejecutándose  
✅ NGINX funcionando como proxy  
✅ Arquitectura lista para producción  

---

# 🧠 Recomendaciones Profesionales

## 🔒 Seguridad
- Nunca uses `CORS_ALLOW_ALL_ORIGINS=True` en producción.
- Usa HTTPS con Certbot.
- Usa variables de entorno.

---

## 🚀 Optimización
- Usa Gunicorn + NGINX.
- Configura caché.
- Usa PostgreSQL en producción real.

---

# 📘 Autor

Tutorial académico para aprendizaje profesional de Django REST Framework.
