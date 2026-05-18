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

# ⚙️ CODIGOS
## productos/admin.py
<pre>
   from django.contrib import admin
   from .models import Producto
   
   @admin.register(Producto)
   class ProductoAdmin(admin.ModelAdmin):
       list_display = ('id', 'codigo', 'descripcion', 'precio')
       search_fields = ('codigo', 'descripcion')
       list_filter = ('precio',)
       ordering = ('id',)
   </pre>

## productos/app.py
<pre>
   from django.apps import AppConfig

   class ProductosConfig(AppConfig):
       default_auto_field = 'django.db.models.BigAutoField'
       name = 'productos'

</pre>

## productos/models.py
<pre>
   from django.db import models

   class Producto(models.Model):
       codigo = models.IntegerField()
       descripcion = models.CharField(max_length=200)
       precio = models.FloatField()
   
       def __str__(self):
           return self.descripcion
</pre>

## productos/serializers.py
<pre>
   from rest_framework import serializers
   from .models import Producto
   
   class ProductoSerializer(serializers.ModelSerializer):
       class Meta:
           model = Producto
           fields = '__all__'
</pre>
## productos/urls.py
<pre>
   from rest_framework import routers
   from .views import ProductoViewSet
   
   router = routers.DefaultRouter()
   router.register(r'productos', ProductoViewSet)
   
   urlpatterns = router.urls
</pre>

## productos/views.py
<pre>
   from rest_framework import viewsets
   from .models import Producto
   from .serializers import ProductoSerializer
   from rest_framework.permissions import IsAuthenticated
   
   class ProductoViewSet(viewsets.ModelViewSet):
       queryset = Producto.objects.all()
       serializer_class = ProductoSerializer
       permission_classes = [IsAuthenticated]
</pre>

## backend/urls.py
<pre>
   from django.contrib import admin
   from django.urls import path, include
   
   from rest_framework_simplejwt.views import (
       TokenObtainPairView,
       TokenRefreshView
   )
   
   urlpatterns = [
       path('admin/', admin.site.urls),
   
       # 🔐 JWT AUTH
       path('api/token/', TokenObtainPairView.as_view(), name='token_obtain_pair'),
       path('api/token/refresh/', TokenRefreshView.as_view(), name='token_refresh'),
   
       # 📦 API APP
       path('api/', include('productos.urls')),
   ]
</pre>

## backend/settings.py
<pre>
   from pathlib import Path
   
   # Build paths inside the project like this: BASE_DIR / 'subdir'.
   BASE_DIR = Path(__file__).resolve().parent.parent
   
   
   # Quick-start development settings - unsuitable for production
   # See https://docs.djangoproject.com/en/5.2/howto/deployment/checklist/
   
   # SECURITY WARNING: keep the secret key used in production secret!
   SECRET_KEY = 'django-insecure-d+voo8@i!_b8c1_(p4mog+teh!6_fnx9&$!7g7m69&5j*n(oj5'
   
   # SECURITY WARNING: don't run with debug turned on in production!
   DEBUG = True
   
   ALLOWED_HOSTS = ['*','django.arequipa.site']
   
   
   # Application definition
   
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
   
   ROOT_URLCONF = 'backend.urls'
   
   TEMPLATES = [
       {
           'BACKEND': 'django.template.backends.django.DjangoTemplates',
           'DIRS': [],
           'APP_DIRS': True,
           'OPTIONS': {
               'context_processors': [
                   'django.template.context_processors.request',
                   'django.contrib.auth.context_processors.auth',
                   'django.contrib.messages.context_processors.messages',
               ],
           },
       },
   ]
   
   WSGI_APPLICATION = 'backend.wsgi.application'
   
   
   # Database
   # https://docs.djangoproject.com/en/5.2/ref/settings/#databases
   
   DATABASES = {
       'default': {
           'ENGINE': 'django.db.backends.mysql',
           'NAME': 'DAE_lab10',
           'USER': 'django_user',
           'PASSWORD': 'Tecsup123',
           'HOST': 'localhost',
           'PORT': '3306',
       }
   }
   
   
   # Password validation
   # https://docs.djangoproject.com/en/5.2/ref/settings/#auth-password-validators
   
   AUTH_PASSWORD_VALIDATORS = [
       {
           'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
       },
       {
           'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
       },
       {
           'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
       },
       {
           'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
       },
   ]
   
   
   # Internationalization
   # https://docs.djangoproject.com/en/5.2/topics/i18n/
   
   LANGUAGE_CODE = 'es'
   TIME_ZONE = 'America/Lima'
   USE_I18N = True
   USE_TZ = True
   
   
   # Static files (CSS, JavaScript, Images)
   # https://docs.djangoproject.com/en/5.2/howto/static-files/
   
   STATIC_URL = 'static/'
   
   # Default primary key field type
   # https://docs.djangoproject.com/en/5.2/ref/settings/#default-auto-field
   
   DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
   
   CORS_ALLOW_ALL_ORIGINS = True
   
   REST_FRAMEWORK = {
       'DEFAULT_AUTHENTICATION_CLASSES': (
           'rest_framework_simplejwt.authentication.JWTAuthentication',
       ),
       'DEFAULT_PERMISSION_CLASSES': (
           'rest_framework.permissions.IsAuthenticated',
       )
   }
   
</pre>

# CHECKPOINT DE LO DESARRLLADO
Recuerda a medida que vas a avanzando vas haciendo pruebas para que ver la implementacion es correcta

## Probar el acceso al admin
<pre>
   python manage.py makemigrations
   python manage.py migrate
   python manage.py createsuperuser
   python manage.py runserver 0.0.0.0:4200
   http://IP_DEL_SERVIDOR:3000/api/productos/
</pre>
## Probar funcionamiento el API
<pre>
   python manage.py runserver 0.0.0.0:4200
   http://IP_DEL_SERVIDOR:3000/api/productos/
</pre>
