# 🔐 Tutorial Completo: Login JWT con React + Django REST Framework

## 📌 Introducción

En este tutorial aprenderás a implementar autenticación JWT usando:

- React
- Axios
- Django REST Framework
- SimpleJWT
- LocalStorage
- Bootstrap

El sistema permitirá:

- Login de usuarios
- Protección de endpoints
- Manejo de JWT
- Logout
- Consumo autenticado de APIs
- Persistencia de sesión

---

# 🧠 Arquitectura JWT

```plaintext
Usuario
   ↓
React Login
   ↓
Axios POST /api/token/
   ↓
Django REST Framework + SimpleJWT
   ↓
Generación JWT
   ↓
React guarda token en LocalStorage
   ↓
Axios envía Bearer Token
   ↓
API protegida
```

---

# 📦 1. Crear Login.js

Archivo:

```plaintext
src/components/Login.js
```

Código:

```javascript
import React, { useState } from "react";
import axios from "axios";

export default function Login({ setToken }) {

  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");

  const login = async (e) => {
    e.preventDefault();

    try {
      const res = await axios.post("/api/token/", {
        username,
        password
      });

      setToken(res.data.access);
      localStorage.setItem("token", res.data.access);

    } catch (error) {
      alert("Error en login");
    }
  };

  return (
    <div className="container mt-5" style={{ maxWidth: "400px" }}>

      <h3>Login</h3>

      <form onSubmit={login}>

        <input
          className="form-control mb-2"
          placeholder="Usuario"
          onChange={(e) => setUsername(e.target.value)}
        />

        <input
          type="password"
          className="form-control mb-2"
          placeholder="Password"
          onChange={(e) => setPassword(e.target.value)}
        />

        <button className="btn btn-primary w-100">
          Ingresar
        </button>

      </form>

    </div>
  );
}
```

---

# 📦 2. Actualizar App.js (Primera versión)

Archivo:

```plaintext
src/App.js
```

Código:

```javascript
import React, { useEffect, useState } from "react";
import axios from "axios";

import "bootstrap/dist/css/bootstrap.min.css";

import Navbar from "./components/Navbar";
import Sidebar from "./components/Sidebar";
import ProductTable from "./components/ProductTable";
import ProductForm from "./components/ProductForm";
import Loader from "./components/Loader";
import SearchBar from "./components/SearchBar";
import StatsCards from "./components/StatsCards";
import Login from "./components/Login";

export default function App() {

  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [search, setSearch] = useState("");

  // 🔥 NUEVO: estado del sidebar
  const [sidebarOpen, setSidebarOpen] = useState(true);

  // const token = localStorage.getItem("token");
  const [token, setToken] = useState(localStorage.getItem("token"));

  if (!token) {
    return <Login setToken={setToken} />;
  }

  const API_URL = "http://161.132.51.98:4200/api/productos/";

  const headers = {
    Authorization: `Bearer ${token}`
  };

  const fetchProducts = async () => {
    try {
      setLoading(true);
      const res = await axios.get(API_URL);
      setProducts(res.data);
    } catch (error) {
      console.log(error);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    fetchProducts();
  }, []);

  const addProduct = async (product) => {
    await axios.post(API_URL, product);
    fetchProducts();
  };

  const deleteProduct = async (id) => {
    await axios.delete(`${API_URL}${id}/`);
    fetchProducts();
  };

  const filteredProducts = products.filter((p) =>
    p.descripcion.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div className="d-flex">

      {/* 🔥 SIDEBAR CONDICIONAL */}
      {sidebarOpen && (
        <div style={{ width: "250px", minHeight: "100vh", position: "fixed" }}>
          <Sidebar />
        </div>
      )}

      {/* CONTENIDO */}
      <div style={{ marginLeft: sidebarOpen ? "250px" : "0px", width: "100%" }}>

        {/* 🔥 NAVBAR RECIBE BOTÓN */}
        <Navbar toggleSidebar={() => setSidebarOpen(!sidebarOpen)} />

        <div className="container mt-4">

          <h3>📊 Dashboard de Productos</h3>

          <SearchBar onSearch={setSearch} />

          <StatsCards products={products} />

          <ProductForm onAdd={addProduct} />

          {loading ? (
            <Loader />
          ) : (
            <ProductTable
              products={filteredProducts}
              onDelete={deleteProduct}
            />
          )}

        </div>

      </div>
    </div>
  );
}
```

---

# 🔐 3. Activar JWT en Django

Instalar SimpleJWT:

```bash
pip install djangorestframework-simplejwt
```

---

# 📦 4. Configurar settings.py

Archivo:

```plaintext
backend/settings.py
```

Agregar:

```python
REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework_simplejwt.authentication.JWTAuthentication',
    ),
    'DEFAULT_PERMISSION_CLASSES': (
        'rest_framework.permissions.IsAuthenticated',
    )
}
```

---

# 📦 5. Proteger Views con JWT

Archivo:

```plaintext
productos/views.py
```

Código:

```python
from rest_framework import viewsets
from .models import Producto
from .serializers import ProductoSerializer
from rest_framework.permissions import IsAuthenticated

class ProductoViewSet(viewsets.ModelViewSet):
    queryset = Producto.objects.all()
    serializer_class = ProductoSerializer
    permission_classes = [IsAuthenticated]
```

---

# 📦 6. Configurar URLs JWT

Archivo:

```plaintext
backend/url.py
```

Código:

```python
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
```

---

# 📦 7. Mejorar Navbar con Logout

Archivo:

```plaintext
src/components/Navbar.js
```

Código:

```javascript
import React from "react";

export default function Navbar({ toggleSidebar, onLogout }) {
  return (
    <nav className="navbar navbar-dark bg-primary px-3 shadow-sm">

      {/* Botón menú */}
      <button
        className="btn btn-light btn-sm"
        onClick={toggleSidebar}
      >
        ☰ Menu
      </button>

      {/* Título */}
      <span className="navbar-brand ms-3">
        🛒 Dashboard Productos
      </span>

      {/* Espaciador */}
      <div className="ms-auto d-flex gap-2">

        {/* Logout */}
        <button
          className="btn btn-outline-light btn-sm"
          onClick={onLogout}
        >
          Logout
        </button>

      </div>

    </nav>
  );
}
```

---

# 📦 8. Actualizar App.js (Versión Final JWT)

Archivo:

```plaintext
src/App.js
```

Código:

```javascript
import React, { useEffect, useState } from "react";
import axios from "axios";

import "bootstrap/dist/css/bootstrap.min.css";

import Navbar from "./components/Navbar";
import Sidebar from "./components/Sidebar";
import ProductTable from "./components/ProductTable";
import ProductForm from "./components/ProductForm";
import Loader from "./components/Loader";
import SearchBar from "./components/SearchBar";
import StatsCards from "./components/StatsCards";
import Login from "./components/Login";

export default function App() {

  // 🔐 AUTH
  const [token, setToken] = useState(localStorage.getItem("token"));

  // 📊 DATA
  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [search, setSearch] = useState("");

  // 📌 UI
  const [sidebarOpen, setSidebarOpen] = useState(true);

  const API_URL = "/api/productos/";

  // 🔐 headers con JWT
  const authHeaders = {
    headers: {
      Authorization: `Bearer ${token}`
    }
  };

  // 📥 GET productos
  const fetchProducts = async () => {
    try {
      setLoading(true);
      const res = await axios.get(API_URL, authHeaders);
      setProducts(res.data);
    } catch (error) {
      console.log(error);
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    if (token) {
      fetchProducts();
    }
  }, [token]);

  // ➕ ADD
  const addProduct = async (product) => {
    await axios.post(API_URL, product, authHeaders);
    fetchProducts();
  };

  // ❌ DELETE
  const deleteProduct = async (id) => {
    await axios.delete(`${API_URL}${id}/`, authHeaders);
    fetchProducts();
  };

  // 🔍 SEARCH
  const filteredProducts = products.filter((p) =>
    p.descripcion.toLowerCase().includes(search.toLowerCase())
  );

  // 🚪 LOGOUT
  const logout = () => {
    localStorage.removeItem("token");
    setToken(null);
  };

  // 🔐 SI NO HAY TOKEN → LOGIN
  if (!token) {
    return <Login setToken={setToken} />;
  }

  return (
    <div className="d-flex">

      {/* SIDEBAR */}
      {sidebarOpen && (
        <div
          style={{
            width: "250px",
            minHeight: "100vh",
            position: "fixed"
          }}
        >
          <Sidebar />
        </div>
      )}

      {/* CONTENIDO */}
      <div
        style={{
          marginLeft: sidebarOpen ? "250px" : "0px",
          width: "100%"
        }}
      >

        {/* NAVBAR */}
        <Navbar
          toggleSidebar={() => setSidebarOpen(!sidebarOpen)}
          onLogout={logout}
        />

        <div className="container mt-4">

          <h3 className="mb-3">📊 Dashboard de Productos</h3>

          <SearchBar onSearch={setSearch} />

          <StatsCards products={products} />

          <ProductForm onAdd={addProduct} />

          {loading ? (
            <Loader />
          ) : (
            <ProductTable
              products={filteredProducts}
              onDelete={deleteProduct}
            />
          )}

        </div>

      </div>
    </div>
  );
}
```

---

# 🔥 9. Flujo Completo JWT

## Login

```plaintext
Usuario ingresa credenciales
        ↓
React envía POST /api/token/
        ↓
Django valida usuario
        ↓
SimpleJWT genera ACCESS TOKEN
        ↓
React guarda token en LocalStorage
```

---

## Consumo API Protegida

```plaintext
React obtiene token
        ↓
Axios agrega:
Authorization: Bearer TOKEN
        ↓
Django valida JWT
        ↓
Devuelve datos protegidos
```

---

# ✅ 10. Crear Usuario Django

Crear usuario administrador:

```bash
python manage.py createsuperuser
```

Ingresar:

```plaintext
username
email
password
```

---

# 🚀 11. Ejecutar Proyecto

## Backend Django

```bash
python manage.py runserver 0.0.0.0:8000
```

## Frontend React

```bash
npm start
```

---

# 🔎 12. Probar JWT Manualmente

## Obtener token

```bash
curl -X POST http://localhost:8000/api/token/ \
-H "Content-Type: application/json" \
-d '{"username":"admin","password":"123456"}'
```

Respuesta:

```json
{
  "refresh": "TOKEN_REFRESH",
  "access": "TOKEN_ACCESS"
}
```

---

# 🔐 Consumir endpoint protegido

```bash
curl http://localhost:8000/api/productos/ \
-H "Authorization: Bearer TOKEN_ACCESS"
```

---

# 📌 Recomendaciones

## Mejoras futuras

- Refresh Token automático
- Axios Interceptors
- Protected Routes
- Roles y permisos
- Multiusuario
- Session timeout
- Context API
- Redux Toolkit
- Docker
- Nginx
- HTTPS

---

# 🎯 Conclusión

Ahora tienes un sistema completo con:

- Login JWT
- React Authentication
- Django REST Framework protegido
- APIs seguras
- Persistencia de sesión
- Logout
- Bearer Token Authentication

Este patrón es utilizado en:

- ERP
- Ecommerce
- Sistemas administrativos
- SaaS
- Dashboards empresariales
- APIs modernas
