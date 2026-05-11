# 🚀 Tutorial Completo: Backend Django + API REST + Frontend React

Este tutorial te guiará paso a paso para construir una aplicación fullstack utilizando:

- ⚙️ Backend con Django + API REST
- 🗄️ Base de datos MySQL
- 🎨 Frontend con React
- 💅 Estilos con Bootstrap

El objetivo final será mostrar una lista de estudiantes desde una API creada en Django y consumirla desde React.

---

# 📋 Requisitos Previos

Antes de comenzar, asegúrate de tener instalado:

- Ubuntu/Linux
- Node.js 20+
- npm 10+
- Python 3
- MySQL Server

---

# 🧩 Arquitectura del Proyecto

```text
Proyecto Fullstack
│
├── backend/      → API REST con Django
│
└── frontend/     → Aplicación React
```

---

# ⚙️ RETO 1 — Backend con Django REST Framework

---

# 1️⃣ Actualizar el sistema

Ejecuta:

```bash
sudo apt update && sudo apt upgrade -y
```

---

# 2️⃣ Verificar versiones de Node y npm

```bash
node -v
npm -v
```

Versiones esperadas:

```bash
Node.js → 20.x
npm     → 10.x
```

---

# 3️⃣ Instalar Python y herramientas necesarias

```bash
sudo apt install python3 python3-pip python3-venv -y
```

---

# 4️⃣ Crear carpeta del backend

```bash
mkdir backend
cd backend
```

---

# 5️⃣ Crear entorno virtual de Python

## 📌 ¿Por qué usar un entorno virtual?

Permite aislar las dependencias del proyecto para evitar conflictos con otros proyectos Python.

### Crear entorno virtual

```bash
python3 -m venv venv
```

### Activarlo

```bash
source venv/bin/activate
```

Cuando esté activo verás algo similar:

```bash
(venv) usuario@server:~/backend$
```

---

# 6️⃣ Instalar Django y dependencias

```bash
pip install django djangorestframework django-cors-headers
```

## 📦 Librerías instaladas

| Librería | Función |
|---|---|
| Django | Framework backend |
| djangorestframework | Crear APIs REST |
| django-cors-headers | Permitir conexión desde React |

---

# 7️⃣ Crear proyecto Django

```bash
django-admin startproject backend
```

Entrar al proyecto:

```bash
cd backend
```

---

# 8️⃣ Crear aplicación API

```bash
python manage.py startapp api
```

---

# 9️⃣ Configurar Django

Editar:

```bash
backend/settings.py
```

---

## Agregar aplicaciones instaladas

Busca:

```python
INSTALLED_APPS = [
```

Y agrega:

```python
'rest_framework',
'corsheaders',
'api',
```

Debe quedar así:

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
    'api',
]
```

---

## Configurar middleware CORS

Busca:

```python
MIDDLEWARE = [
```

Y agrega:

```python
'corsheaders.middleware.CorsMiddleware',
```

Preferiblemente al inicio:

```python
MIDDLEWARE = [
    'corsheaders.middleware.CorsMiddleware',

    'django.middleware.security.SecurityMiddleware',
    ...
]
```

---

## Permitir peticiones desde cualquier origen

Al final de `settings.py` agrega:

```python
CORS_ALLOW_ALL_ORIGINS = True
```

> ⚠️ Solo recomendado para desarrollo.

---

# 🗄️ 1️⃣0️⃣ Configurar Base de Datos MySQL

Entrar a MySQL:

```bash
sudo mysql
```

Ejecutar:

```sql
CREATE DATABASE lab_9 CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

CREATE USER 'lab_user'@'localhost' IDENTIFIED BY 'Lab2026Secure!';

GRANT ALL PRIVILEGES ON lab_9.* TO 'lab_user'@'localhost';

FLUSH PRIVILEGES;

EXIT;
```

---

# 1️⃣1️⃣ Instalar driver MySQL para Django

```bash
pip install mysqlclient
```

---

# 1️⃣2️⃣ Configurar conexión MySQL en Django

Editar:

```bash
backend/settings.py
```

Busca:

```python
DATABASES = {
```

Reemplaza por:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'lab_9',
        'USER': 'lab_user',
        'PASSWORD': 'Lab2026Secure!',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
```

---

# 🧱 1️⃣3️⃣ Crear modelo Estudiante

Editar:

```bash
api/models.py
```

Agregar:

```python
from django.db import models

class Estudiante(models.Model):
    nombre = models.CharField(max_length=100)
    carrera = models.CharField(max_length=100)

    def __str__(self):
        return self.nombre
```

---

# 1️⃣4️⃣ Crear migraciones

```bash
python manage.py makemigrations
python manage.py migrate
```

---

# 🔄 1️⃣5️⃣ Crear Serializer

## 📌 ¿Qué es un Serializer?

Convierte objetos Python en JSON para enviarlos por la API.

Crear archivo:

```bash
api/serializers.py
```

Contenido:

```python
from rest_framework import serializers
from .models import Estudiante

class EstudianteSerializer(serializers.ModelSerializer):
    class Meta:
        model = Estudiante
        fields = '__all__'
```

---

# 🌐 1️⃣6️⃣ Crear Vista API

Editar:

```bash
api/views.py
```

Agregar:

```python
from rest_framework.decorators import api_view
from rest_framework.response import Response
from .models import Estudiante
from .serializers import EstudianteSerializer

@api_view(['GET'])
def estudiantes(request):
    datos = Estudiante.objects.all()
    serializer = EstudianteSerializer(datos, many=True)
    return Response(serializer.data)
```

---

# 🔗 1️⃣7️⃣ Crear rutas API

Crear archivo:

```bash
api/urls.py
```

Contenido:

```python
from django.urls import path
from .views import estudiantes

urlpatterns = [
    path('estudiantes/', estudiantes),
]
```

---

# 1️⃣8️⃣ Registrar rutas principales

Editar:

```bash
backend/urls.py
```

Contenido:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('api.urls')),
]
```

---

# 👤 1️⃣9️⃣ Crear usuario administrador

```bash
python manage.py createsuperuser
```

Sigue las instrucciones en pantalla.

---

# 🛠️ 2️⃣0️⃣ Registrar modelo en Admin

Editar:

```bash
api/admin.py
```

Agregar:

```python
from django.contrib import admin
from .models import Estudiante

admin.site.register(Estudiante)
```

---

# 2️⃣1️⃣ Ejecutar migraciones nuevamente

```bash
python manage.py makemigrations
python manage.py migrate
```

---

# ▶️ 2️⃣2️⃣ Ejecutar servidor Django

```bash
python manage.py runserver 0.0.0.0:3000
```

---

# ✅ Verificar Backend

Abrir:

```text
http://IP_VPS:3000/admin
```

Inicia sesión y crea algunos estudiantes.

Luego prueba:

```text
http://IP_VPS:3000/api/estudiantes/
```

Deberías ver un JSON similar a:

```json
[
  {
    "id": 1,
    "nombre": "Juan",
    "carrera": "Ingeniería"
  }
]
```

---

# 🎨 RETO 2 — Frontend con React

---

# 1️⃣ Crear proyecto React

Salir del backend:

```bash
cd ..
```

Crear carpeta frontend:

```bash
mkdir frontend
cd frontend
```

Crear aplicación React:

```bash
npx create-react-app frontend
```

Entrar al proyecto:

```bash
cd frontend
```

---

# 2️⃣ Instalar Bootstrap

```bash
npm install bootstrap
```

---

# 3️⃣ Importar Bootstrap

Editar:

```bash
src/index.js
```

Agregar:

```javascript
import 'bootstrap/dist/css/bootstrap.min.css';
```

---

# 4️⃣ Crear aplicación React

Editar:

```bash
src/App.js
```

Reemplazar contenido por:

```javascript
import { useEffect, useState } from 'react';

function App() {

  const [datos, setDatos] = useState([]);

  useEffect(() => {

    fetch('http://IP_VPS:3000/api/estudiantes/')
      .then(res => res.json())
      .then(data => setDatos(data));

  }, []);

  return (
    <div className="container mt-5">

      <h1 className="mb-4">
        Lista de Estudiantes
      </h1>

      <table className="table table-bordered table-hover">

        <thead className="table-dark">
          <tr>
            <th>ID</th>
            <th>Nombre</th>
            <th>Carrera</th>
          </tr>
        </thead>

        <tbody>
          {datos.map(item => (
            <tr key={item.id}>
              <td>{item.id}</td>
              <td>{item.nombre}</td>
              <td>{item.carrera}</td>
            </tr>
          ))}
        </tbody>

      </table>

    </div>
  );
}

export default App;
```

---

# ▶️ 5️⃣ Ejecutar React

Desde la carpeta `frontend` ejecutar:

```bash
npm start -- --host 0.0.0.0
```

---

# ✅ Verificar Frontend

Abrir:

```text
http://IP_VPS:3000
```

Deberías visualizar:

- Tabla Bootstrap
- Datos obtenidos desde Django
- Lista de estudiantes desde la API

---

# 🧪 Flujo Completo de la Aplicación

```text
MySQL
   ↓
Django Models
   ↓
Django REST API
   ↓
React Frontend
   ↓
Usuario final
```

---

# 📌 Buenas Prácticas Recomendadas

## ✅ Backend

- Usar variables de entorno para passwords
- No usar `CORS_ALLOW_ALL_ORIGINS=True` en producción
- Utilizar `.env`

---

## ✅ Frontend

- Separar componentes
- Usar Axios en lugar de fetch
- Implementar manejo de errores

---

# 🚀 Próximos pasos recomendados

Puedes mejorar el proyecto agregando:

- ✏️ CRUD completo
- 🔐 Autenticación JWT
- 📄 Paginación
- 🔎 Búsqueda de estudiantes
- ☁️ Deploy con Docker
- 🔥 Nginx + Gunicorn
- 📦 CI/CD con GitHub Actions

---

# 🏁 Resultado Final

Al finalizar tendrás:

✅ API REST funcional  
✅ Base de datos MySQL conectada  
✅ Frontend React consumiendo datos  
✅ Arquitectura fullstack moderna  
✅ Proyecto listo para escalar
