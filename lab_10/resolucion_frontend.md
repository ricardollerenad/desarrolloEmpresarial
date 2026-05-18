# ⚛️ Tutorial Completo: Dashboard React + Django REST Framework

## 📌 Introducción

En este tutorial aprenderás a crear un dashboard moderno usando:

- React
- Material UI
- Bootstrap
- Axios
- Django REST Framework
- MariaDB / SQLite

El sistema permitirá:

- Crear productos
- Listar productos
- Eliminar productos
- Buscar productos
- Mostrar métricas
- Consumir una API REST desde React

---

# 📦 1. Crear Proyecto React

Ubícate en la raíz del proyecto:

```bash
cd /raiz_del_proyecto
```

Crear aplicación React:

```bash
npx create-react-app frontend
```

Ingresar al proyecto:

```bash
cd frontend
```

---

# 📦 2. Instalar Dependencias

## Instalar Material UI

```bash
npm install @mui/material @emotion/react @emotion/styled
```

## Instalar Axios y React Router

```bash
npm install axios react-router-dom
```

## Instalar Axios (opcional si ya fue instalado)

```bash
npm install axios
```

## Instalar Bootstrap

```bash
npm install bootstrap
```

---

# 📁 3. Estructura del Proyecto

Crear la siguiente estructura:

```plaintext
src/
│
├── components/
├── pages/
├── services/
├── layouts/
├── router/
└── App.js
```

---

# 📦 4. Configurar Axios

Crear el archivo:

```plaintext
src/services/api.js
```

Contenido:

```javascript
import axios from 'axios'

export default axios.create({
    baseURL: 'http://django.arequipa.site/api/'
})
```

---

# ✅ CHECKPOINT: Verificar React funcionando

Iniciar React:

```bash
HOST=0.0.0.0 npm start
```

Si todo está correcto, React iniciará en:

```plaintext
http://localhost:3000
```

---

# 📦 5. Crear Componentes

Crear los siguientes archivos:

```plaintext
src/components/Navbar.js
src/components/Sidebar.js
src/components/ProductTable.js
src/components/ProductForm.js
src/components/Loader.js
src/components/SearchBar.js
src/components/StatsCards.js
```

---

# 📌 Función de cada componente

| Componente | Función |
|---|---|
| Navbar | Barra de navegación |
| Sidebar | Menú lateral |
| SearchBar | Filtro de búsqueda |
| StatsCards | Métricas |
| ProductForm | Crear productos |
| ProductTable | Leer y eliminar productos |
| Loader | Estado de carga |

---

# 🧠 Arquitectura General

```plaintext
React UI
   ↓
Axios fetch / API Django
   ↓
Django REST Framework
   ↓
MariaDB / SQLite
```

---

# 📦 6. Navbar.js

Archivo:

```plaintext
src/components/Navbar.js
```

Código:

```javascript
import React from "react";

export default function Navbar({ toggleSidebar }) {
  return (
    <nav className="navbar navbar-dark bg-primary px-3">

      {/* 🔥 BOTÓN MENU */}
      <button className="btn btn-light btn-sm" onClick={toggleSidebar}>
        ☰ Menu
      </button>

      <span className="navbar-brand ms-3">
        🛒 Sistema de Productos
      </span>

      <button className="btn btn-outline-light btn-sm">
        Logout
      </button>

    </nav>
  );
}
```

---

# 📦 7. Sidebar.js

Archivo:

```plaintext
src/components/Sidebar.js
```

Código base sugerido:

```javascript
import React from "react";

export default function Sidebar() {
  return (
    <div className="bg-dark text-white p-3" style={{ minHeight: "100vh" }}>
      <h4>📂 Menú</h4>

      <ul className="list-unstyled mt-4">
        <li className="mb-3">📊 Dashboard</li>
        <li className="mb-3">📦 Productos</li>
        <li className="mb-3">⚙️ Configuración</li>
      </ul>
    </div>
  );
}
```

---

# 📦 8. ProductTable.js (READ + DELETE)

Archivo:

```plaintext
src/components/ProductTable.js
```

Código:

```javascript
import React from "react";

export default function ProductTable({ products, onDelete }) {
  return (
    <table className="table table-striped table-hover">

      <thead className="table-dark">
        <tr>
          <th>Código</th>
          <th>Descripción</th>
          <th>Precio</th>
          <th>Acciones</th>
        </tr>
      </thead>

      <tbody>
        {products.map((p) => (
          <tr key={p.id}>
            <td>{p.codigo}</td>
            <td>{p.descripcion}</td>
            <td>S/. {p.precio}</td>
            <td>
              <button
                className="btn btn-danger btn-sm"
                onClick={() => onDelete(p.id)}
              >
                Eliminar
              </button>
            </td>
          </tr>
        ))}
      </tbody>

    </table>
  );
}
```

---

# 📦 9. ProductForm.js (CREATE)

Archivo:

```plaintext
src/components/ProductForm.js
```

Código:

```javascript
import React, { useState } from "react";

export default function ProductForm({ onAdd }) {
  const [form, setForm] = useState({
    codigo: "",
    descripcion: "",
    precio: ""
  });

  const handleChange = (e) => {
    setForm({ ...form, [e.target.name]: e.target.value });
  };

  const submit = (e) => {
    e.preventDefault();
    onAdd(form);
    setForm({ codigo: "", descripcion: "", precio: "" });
  };

  return (
    <form onSubmit={submit} className="row g-2">

      <div className="col-md-3">
        <input
          name="codigo"
          className="form-control"
          placeholder="Código"
          value={form.codigo}
          onChange={handleChange}
        />
      </div>

      <div className="col-md-5">
        <input
          name="descripcion"
          className="form-control"
          placeholder="Descripción"
          value={form.descripcion}
          onChange={handleChange}
        />
      </div>

      <div className="col-md-2">
        <input
          name="precio"
          className="form-control"
          placeholder="Precio"
          value={form.precio}
          onChange={handleChange}
        />
      </div>

      <div className="col-md-2">
        <button className="btn btn-success w-100">
          Agregar
        </button>
      </div>

    </form>
  );
}
```

---

# 📦 10. Loader.js

Archivo:

```plaintext
src/components/Loader.js
```

Código:

```javascript
import React from "react";
import { CircularProgress, Box } from "@mui/material";

export default function Loader() {
  return (
    <Box style={{ textAlign: "center", marginTop: "20px" }}>
      <CircularProgress />
    </Box>
  );
}
```

---

# 📦 11. SearchBar.js

Archivo:

```plaintext
src/components/SearchBar.js
```

Código:

```javascript
import React from "react";
import { TextField } from "@mui/material";

export default function SearchBar({ onSearch }) {
  return (
    <TextField
      label="Buscar producto"
      fullWidth
      onChange={(e) => onSearch(e.target.value)}
    />
  );
}
```

---

# 📦 12. StatsCards.js

Archivo:

```plaintext
src/components/StatsCards.js
```

Código:

```javascript
import React from "react";
import { Card, CardContent, Typography, Grid } from "@mui/material";

export default function StatsCards({ products }) {
  return (
    <Grid container spacing={2}>
      <Grid item xs={4}>
        <Card>
          <CardContent>
            <Typography>Total Productos</Typography>
            <Typography variant="h4">{products.length}</Typography>
          </CardContent>
        </Card>
      </Grid>

      <Grid item xs={4}>
        <Card>
          <CardContent>
            <Typography>Precio Promedio</Typography>
            <Typography variant="h4">
              {products.length > 0
                ? (
                    products.reduce((a, b) => a + parseFloat(b.precio), 0) /
                    products.length
                  ).toFixed(2)
                : 0}
            </Typography>
          </CardContent>
        </Card>
      </Grid>
    </Grid>
  );
}
```

---

# 📦 13. Configurar App.js

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

export default function App() {

  const [products, setProducts] = useState([]);
  const [loading, setLoading] = useState(true);
  const [search, setSearch] = useState("");

  // 🔥 NUEVO: estado del sidebar
  const [sidebarOpen, setSidebarOpen] = useState(true);

  const API_URL = "http://161.132.51.98:4200/api/productos/";

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

# 🚀 14. Ejecutar Proyecto

Iniciar React:

```bash
npm start
```

---

# ✅ Resultado Final

El dashboard tendrá:

- Sidebar dinámico
- Navbar
- CRUD básico
- Consumo API REST
- Loader
- Filtro de búsqueda
- Métricas
- Diseño responsive
- Material UI + Bootstrap

---

# 📌 Recomendaciones

## Mejoras futuras

- JWT Authentication
- Protected Routes
- Redux Toolkit
- Context API
- Paginación
- Dark Mode
- Charts
- Docker
- Nginx
- Deploy en VPS

---

# 🎯 Conclusión

Ahora tienes un frontend profesional en React conectado a Django REST Framework usando Axios y Material UI.

Este proyecto sirve como base para:

- ERP
- Inventarios
- Ecommerce
- Sistemas administrativos
- Dashboards empresariales
