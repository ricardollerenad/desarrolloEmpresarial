# 🛒 TUTORIAL COMPLETO — TIENDA VIRTUAL FULLSTACK
# Django + React + MySQL + Bootstrap + Debian 12 VPS

---

# 📚 Tecnologías Utilizadas

Este proyecto implementará:

- ✅ Backend con Django
- ✅ API REST con Django REST Framework
- ✅ Frontend con React
- ✅ Base de datos MySQL
- ✅ Bootstrap 5
- ✅ VPS Debian 12
- ✅ Arquitectura FullStack moderna

---

# 🧱 Arquitectura del Proyecto

```text
Usuario
   ↓
React Frontend
   ↓
Django REST API
   ↓
MySQL
```

---

# 📦 Estructura Final del Proyecto

```text
tienda-virtual/
│
├── backend/
│   ├── tienda/
│   ├── api/
│   ├── manage.py
│
├── frontend/
│   ├── src/
│   ├── public/
│
└── README.md
```

---

# 🖥️ PARTE 1 — CONFIGURAR VPS DEBIAN 12

---

# 1️⃣ Actualizar servidor

```bash
sudo apt update && sudo apt upgrade -y
```

---

# 2️⃣ Instalar herramientas necesarias

```bash
sudo apt install python3 python3-pip python3-venv git curl nodejs npm mysql-server -y
```

---

# 3️⃣ Verificar versiones

```bash
python3 --version
node -v
npm -v
mysql --version
```

---

# 🛢️ PARTE 2 — CONFIGURAR MYSQL

---

# 1️⃣ Ingresar a MySQL

```bash
sudo mysql
```

---

# 2️⃣ Crear base de datos

```sql
CREATE DATABASE tienda_db;
```

---

# 3️⃣ Crear usuario

```sql
CREATE USER 'tienda_user'@'localhost' IDENTIFIED BY '123456';
```

---

# 4️⃣ Dar permisos

```sql
GRANT ALL PRIVILEGES ON tienda_db.* TO 'tienda_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

# ⚙️ PARTE 3 — CREAR BACKEND DJANGO

---

# 1️⃣ Crear carpeta principal

```bash
mkdir tienda-virtual
cd tienda-virtual
```

---

# 2️⃣ Crear carpeta backend

```bash
mkdir backend
cd backend
```

---

# 3️⃣ Crear entorno virtual

```bash
python3 -m venv venv
```

---

# 4️⃣ Activar entorno virtual

```bash
source venv/bin/activate
```

---

# 5️⃣ Instalar dependencias

```bash
pip install django djangorestframework mysqlclient django-cors-headers pillow
```

---

# 📌 Explicación de librerías

| Librería | Función |
|---|---|
| django | Backend |
| djangorestframework | API REST |
| mysqlclient | Conexión MySQL |
| django-cors-headers | Permitir React |
| pillow | Manejo de imágenes |

---

# 6️⃣ Crear proyecto Django

```bash
django-admin startproject tienda .
```

---

# 7️⃣ Crear aplicación API

```bash
python manage.py startapp api
```

---

# 🛠️ PARTE 4 — CONFIGURAR DJANGO

---

# 1️⃣ Editar archivo

```text
backend/tienda/settings.py
```

---

# 2️⃣ Buscar

```python
INSTALLED_APPS = [
```

Agregar:

```python
'rest_framework',
'corsheaders',
'api',
```

---

# ✅ Resultado esperado

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

# 3️⃣ Configurar Middleware

Buscar:

```python
MIDDLEWARE = [
```

Agregar al inicio:

```python
'corsheaders.middleware.CorsMiddleware',
```

---

# ✅ Resultado esperado

```python
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
```

---

# 4️⃣ Configurar MySQL

Buscar:

```python
DATABASES = {
```

Reemplazar por:

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'tienda_db',
        'USER': 'tienda_user',
        'PASSWORD': '123456',
        'HOST': 'localhost',
        'PORT': '3306',
    }
}
```

---

# 5️⃣ Permitir React

Agregar al final:

```python
CORS_ALLOW_ALL_ORIGINS = True
```

---

# 🛍️ PARTE 5 — CREAR MODELO DE PRODUCTOS

---

# 1️⃣ Editar archivo

```text
api/models.py
```

---

# 2️⃣ Reemplazar contenido por:

```python
from django.db import models

class Producto(models.Model):

    nombre = models.CharField(max_length=200)

    descripcion = models.TextField()

    precio = models.DecimalField(
        max_digits=10,
        decimal_places=2
    )

    imagen = models.URLField()

    stock = models.IntegerField()

    creado = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.nombre
```

---

# 🧩 PARTE 6 — CREAR SERIALIZER

---

# 1️⃣ Crear archivo

```text
api/serializers.py
```

---

# 2️⃣ Agregar código

```python
from rest_framework import serializers
from .models import Producto

class ProductoSerializer(serializers.ModelSerializer):

    class Meta:
        model = Producto
        fields = '__all__'
```

---

# 🌐 PARTE 7 — CREAR API CRUD

---

# 1️⃣ Editar archivo

```text
api/views.py
```

---

# 2️⃣ Reemplazar contenido por:

```python
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework import status

from .models import Producto
from .serializers import ProductoSerializer


@api_view(['GET', 'POST'])
def productos(request):

    if request.method == 'GET':

        productos = Producto.objects.all()

        serializer = ProductoSerializer(
            productos,
            many=True
        )

        return Response(serializer.data)

    elif request.method == 'POST':

        serializer = ProductoSerializer(
            data=request.data
        )

        if serializer.is_valid():
            serializer.save()

            return Response(
                serializer.data,
                status=status.HTTP_201_CREATED
            )

        return Response(
            serializer.errors,
            status=status.HTTP_400_BAD_REQUEST
        )


@api_view(['GET', 'PUT', 'DELETE'])
def producto_detalle(request, id):

    try:
        producto = Producto.objects.get(id=id)

    except Producto.DoesNotExist:

        return Response(
            {'error': 'Producto no encontrado'},
            status=status.HTTP_404_NOT_FOUND
        )

    if request.method == 'GET':

        serializer = ProductoSerializer(producto)

        return Response(serializer.data)

    elif request.method == 'PUT':

        serializer = ProductoSerializer(
            producto,
            data=request.data
        )

        if serializer.is_valid():
            serializer.save()

            return Response(serializer.data)

        return Response(
            serializer.errors,
            status=status.HTTP_400_BAD_REQUEST
        )

    elif request.method == 'DELETE':

        producto.delete()

        return Response(
            {'mensaje': 'Producto eliminado'},
            status=status.HTTP_204_NO_CONTENT
        )
```

---

# 🔗 PARTE 8 — CONFIGURAR URLS

---

# 1️⃣ Crear archivo

```text
api/urls.py
```

---

# 2️⃣ Agregar código

```python
from django.urls import path
from . import views

urlpatterns = [

    path(
        'productos/',
        views.productos
    ),

    path(
        'productos/<int:id>/',
        views.producto_detalle
    ),
]
```

---

# 3️⃣ Editar archivo

```text
tienda/urls.py
```

---

# 4️⃣ Reemplazar contenido por:

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [

    path('admin/', admin.site.urls),

    path('api/', include('api.urls')),
]
```

---

# 🗄️ PARTE 9 — CREAR TABLAS MYSQL

---

# 1️⃣ Crear migraciones

```bash
python manage.py makemigrations
```

---

# 2️⃣ Migrar base de datos

```bash
python manage.py migrate
```

---

# 3️⃣ Crear superusuario

```bash
python manage.py createsuperuser
```

---

# 🚀 PARTE 10 — EJECUTAR DJANGO

---

```bash
python manage.py runserver 0.0.0.0:8000
```

---

# ✅ Verificar API

Abrir:

```text
http://IP_VPS:8000/api/productos/
```

---

# 🎨 PARTE 11 — CREAR FRONTEND REACT

---

# 1️⃣ Volver a carpeta principal

```bash
cd ..
```

---

# 2️⃣ Crear frontend

```bash
npx create-react-app frontend
```

---

# 3️⃣ Entrar al proyecto

```bash
cd frontend
```

---

# 4️⃣ Instalar Bootstrap

```bash
npm install bootstrap
```

---

# 5️⃣ Editar archivo

```text
src/index.js
```

---

# 6️⃣ Agregar Bootstrap

```javascript
import 'bootstrap/dist/css/bootstrap.min.css';
```

---

# ✅ Resultado esperado

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';

import 'bootstrap/dist/css/bootstrap.min.css';

const root = ReactDOM.createRoot(document.getElementById('root'));

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

# 🛒 PARTE 12 — CREAR TIENDA VIRTUAL

---

# 1️⃣ Editar archivo

```text
src/App.js
```

---

# 2️⃣ Eliminar todo y reemplazar por:

```javascript
import { useEffect, useState } from 'react';

function App() {

  const [productos, setProductos] = useState([]);

  useEffect(() => {

    fetch('http://IP_VPS:8000/api/productos/')
      .then(res => res.json())
      .then(data => setProductos(data));

  }, []);

  return (

    <div className="container mt-5">

      <h1 className="text-center mb-5">
        🛒 Tienda Virtual
      </h1>

      <div className="row">

        {productos.map(producto => (

          <div
            className="col-md-4 mb-4"
            key={producto.id}
          >

            <div className="card h-100 shadow">

              <img
                src={producto.imagen}
                className="card-img-top"
                alt={producto.nombre}
                style={{
                  height: '250px',
                  objectFit: 'cover'
                }}
              />

              <div className="card-body">

                <h5 className="card-title">
                  {producto.nombre}
                </h5>

                <p className="card-text">
                  {producto.descripcion}
                </p>

                <h4 className="text-success">
                  S/. {producto.precio}
                </h4>

                <p>
                  Stock: {producto.stock}
                </p>

                <button className="btn btn-primary w-100">
                  Comprar
                </button>

              </div>

            </div>

          </div>

        ))}

      </div>

    </div>
  );
}

export default App;
```

---

# ⚠️ IMPORTANTE

Buscar:

```javascript
http://IP_VPS:8000/api/productos/
```

Reemplazar:

```text
IP_VPS
```

por la IP pública de tu VPS.

---

# ✅ Ejemplo

```javascript
fetch('http://192.168.1.50:8000/api/productos/')
```

---

# ▶️ PARTE 13 — EJECUTAR REACT

---

```bash
npm start
```

---

# ✅ Abrir aplicación

```text
http://IP_VPS:3000
```

---

# 🧪 PARTE 14 — AGREGAR PRODUCTOS DESDE ADMIN

---

# 1️⃣ Editar archivo

```text
api/admin.py
```

---

# 2️⃣ Agregar código

```python
from django.contrib import admin
from .models import Producto

admin.site.register(Producto)
```

---

# 3️⃣ Reiniciar servidor

```bash
python manage.py runserver 0.0.0.0:8000
```

---

# 4️⃣ Abrir admin

```text
http://IP_VPS:8000/admin
```

---

# 5️⃣ Agregar productos

Ejemplo:

| Campo | Valor |
|---|---|
| nombre | Laptop Gamer |
| descripcion | Ryzen 7 + RTX |
| precio | 4500 |
| imagen | https://imagen.com/laptop.jpg |
| stock | 10 |

---

# 🎉 RESULTADO FINAL

Tendrás:

✅ Tienda virtual FullStack  
✅ Backend Django REST  
✅ Frontend React  
✅ MySQL conectado  
✅ Bootstrap responsive  
✅ Cards modernas  
✅ CRUD API funcional  
✅ VPS Debian 12 funcionando  
✅ Arquitectura profesional FullStack  

---

# 📚 BUENAS PRÁCTICAS RECOMENDADAS

## ✅ Backend

- Utilizar variables de entorno `.env`
- No usar:
  
```python
CORS_ALLOW_ALL_ORIGINS = True
```

en producción.

- Usar HTTPS
- Validar datos
- Utilizar JWT

---

## ✅ Frontend

- Separar componentes
- Usar Axios
- Implementar manejo de errores
- Crear estructura:

```text
src/
├── components/
├── pages/
├── services/
├── styles/
```

---

# 🚀 MEJORAS FUTURAS

Puedes agregar:

- 🔐 Login JWT
- 🛒 Carrito de compras
- 💳 Pasarela de pago
- 📦 Gestión de pedidos
- 📄 Paginación
- 🔎 Buscador
- ☁️ Docker
- 🔥 Nginx + Gunicorn
- 📦 GitHub Actions
- 📱 Responsive Design
- ⭐ Favoritos
- 🧾 Facturación electrónica

---

# 🏁 CONCLUSIÓN

Con este proyecto tendrás una arquitectura moderna FullStack:

```text
React
   ↓
Django REST API
   ↓
MySQL
```

Lista para:

✅ Escalar  
✅ Implementar seguridad  
✅ Migrar a Docker  
✅ Desplegar en producción  
✅ Convertirse en un eCommerce profesional
