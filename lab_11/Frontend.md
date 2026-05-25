# ⚛️ Tutorial Completo — Frontend con React + Vite + Bootstrap + Axios

> 📘 Guía profesional para aprender a construir un frontend moderno usando React conectado a una API Django REST Framework.

---

# 🎯 Objetivos del laboratorio

Al finalizar este tutorial podrás:

- ✅ Crear un proyecto React con Vite
- ✅ Consumir APIs REST con Axios
- ✅ Organizar componentes profesionalmente
- ✅ Usar Bootstrap para UI moderna
- ✅ Implementar CRUD visual de tareas
- ✅ Construir el frontend para producción
- ✅ Entender el flujo completo React → API → Django

---

# 🚀 PARTE 2 — SETUP FRONTEND

---

# 🟢 Paso 1 — Crear Proyecto React

> 📌 Este paso se realiza desde el mismo nivel donde se encuentra el backend.

---

## 🔹 Crear proyecto con Vite

```bash
npm create vite@latest frontend

cd frontend

npm install

npm install axios bootstrap react-icons
```

---

# 📌 Elegir:

```text
React
JavaScript
```

---

# 🧠 Explicación

| Herramienta | Función |
|---|---|
| Vite | Bundler ultrarrápido |
| React | Librería frontend |
| Axios | Consumir APIs |
| Bootstrap | Estilos rápidos |
| react-icons | Íconos modernos |

---

# 🟢 Paso 2 — Instalar Dependencias

```bash
npm install axios bootstrap react-icons
```

---

# 📌 ¿Por qué usamos estas librerías?

## Axios
Permite hacer peticiones HTTP al backend.

## Bootstrap
Permite crear interfaces modernas rápidamente.

## React Icons
Permite usar íconos profesionales fácilmente.

---

# ✅ Checkpoint 1 — React funcionando

```bash
npm run dev -- --host 0.0.0.0 --port 3000
```

---

# 📌 Explicación del comando

| Parte | Función |
|---|---|
| npm run dev | Inicia React |
| --host 0.0.0.0 | Permite acceso externo |
| --port 3000 | Define puerto |

---

# 🟢 Importar Bootstrap

## 📄 src/main.jsx

```jsx
import 'bootstrap/dist/css/bootstrap.min.css'
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App.jsx'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
)
```

---

# 📌 Explicación

## main.jsx
Es el punto de entrada principal de React.

Aquí:

- React inicia
- Se monta App.jsx
- Bootstrap se carga globalmente

---

# 🧠 Estructura de carpetas

```text
src/
 ├── components/
 │     ├── TareaForm.jsx
 │     ├── TareaItem.jsx
 │     ├── TareaList.jsx
 ├── App.jsx
 ├── api.js
```

---

# 📌 Explicación Arquitectónica

| Archivo | Función |
|---|---|
| App.jsx | Controlador principal |
| api.js | Configuración Axios |
| components | Componentes reutilizables |

---

# 🟢 Paso 3 — Conectar React con Django API

## 📄 src/api.js

```jsx
import axios from "axios";

const API = axios.create({
  baseURL: "http://IP_DE_TU_VPS:8000/api/",
});

export default API;
```

---

# 📌 Explicación

## axios.create()

Crea una instancia personalizada de Axios.

Esto evita repetir URLs constantemente.

---

# 🟢 Paso 4 — Configuración del código

# 🧪 Primera prueba de conexión

## 📄 App.jsx

```jsx
import { useEffect, useState } from "react";
import API from "./api";

function App() {
  const [tareas, setTareas] = useState([]);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  const cargarTareas = async () => {
    try {
      setLoading(true);

      const res = await API.get("tareas/");
      console.log("📦 API Response:", res.data);

      setTareas(res.data);
      setError(null);
    } catch (err) {
      console.error(err);
      setError("❌ Error conectando con el backend");
    } finally {
      setLoading(false);
    }
  };

  useEffect(() => {
    cargarTareas();
  }, []);

  return (
    <div className="container py-5">

      {/* HEADER */}
      <div className="text-center mb-4">
        <h1 className="fw-bold text-primary">
          🧪 TEST REACT + DJANGO
        </h1>

        <p className="text-muted">
          Verificación de conexión frontend - backend
        </p>
      </div>

      {/* ALERTAS */}
      {loading && (
        <div className="alert alert-info text-center">
          ⏳ Cargando tareas desde la API...
        </div>
      )}

      {error && (
        <div className="alert alert-danger text-center">
          {error}
        </div>
      )}

      {/* RESUMEN */}
      <div className="card shadow-sm mb-4">
        <div className="card-body text-center">
          <h5 className="card-title">
            📊 Estado del sistema
          </h5>

          <p className="mb-0">
            Total de tareas:
            <span className="badge bg-primary ms-2">
              {tareas.length}
            </span>
          </p>
        </div>
      </div>

      {/* LISTA */}
      <div className="card shadow-sm">
        <div className="card-header bg-dark text-white">
          📋 Tareas desde Django API
        </div>

        <ul className="list-group list-group-flush">
          {tareas.map((t) => (
            <li
              key={t.id}
              className="list-group-item d-flex justify-content-between align-items-center"
            >
              <span>
                {t.completada ? "✅" : "❌"} {t.titulo}
              </span>

              <span
                className={
                  t.completada
                    ? "badge bg-success"
                    : "badge bg-warning text-dark"
                }
              >
                {t.completada ? "Completada" : "Pendiente"}
              </span>
            </li>
          ))}
        </ul>

        {tareas.length === 0 && !loading && (
          <div className="p-3 text-center text-muted">
            No hay tareas registradas
          </div>
        )}
      </div>

      {/* BOTÓN REFRESH */}
      <div className="text-center mt-4">
        <button
          className="btn btn-outline-primary"
          onClick={cargarTareas}
        >
          🔄 Recargar datos
        </button>
      </div>
    </div>
  );
}

export default App;
```

---

# 📌 Explicación del flujo React

## useEffect()
Ejecuta funciones automáticamente cuando el componente carga.

## useState()
Permite guardar estados dinámicos.

## API.get()
Consume datos del backend.

---

# 🟢 Lógica principal del sistema

## 📄 App.jsx

```jsx
import { useEffect, useState } from "react";
import API from "./api";

import TareaForm from "./components/TareaForm";
import TareaList from "./components/TareaList";

function App() {
  const [tareas, setTareas] = useState([]);
  const [filtro, setFiltro] = useState("todas");

  const cargarTareas = async () => {
    const res = await API.get("tareas/");
    setTareas(res.data);
  };

  useEffect(() => {
    cargarTareas();
  }, []);

  const crearTarea = async (titulo) => {
    await API.post("tareas/", {
      titulo,
      completada: false,
    });

    cargarTareas();
  };

  const eliminarTarea = async (id) => {
    await API.delete(`tareas/${id}/`);
    cargarTareas();
  };

  const toggleTarea = async (tarea) => {
    await API.put(`tareas/${tarea.id}/`, {
      ...tarea,
      completada: !tarea.completada,
    });

    cargarTareas();
  };

  const tareasFiltradas = tareas.filter((t) => {
    if (filtro === "completadas") return t.completada;
    if (filtro === "pendientes") return !t.completada;
    return true;
  });

  return (
    <div className="container mt-4">

      <h2 className="text-center fw-bold text-primary">
        🟢 Gestor de Tareas
      </h2>

      {/* FORM */}
      <TareaForm crearTarea={crearTarea} />

      {/* FILTROS */}
      <div className="d-flex gap-2 justify-content-center my-3">
        <button
          onClick={() => setFiltro("todas")}
          className="btn btn-primary"
        >
          Todas
        </button>

        <button
          onClick={() => setFiltro("pendientes")}
          className="btn btn-warning"
        >
          Pendientes
        </button>

        <button
          onClick={() => setFiltro("completadas")}
          className="btn btn-success"
        >
          Completadas
        </button>
      </div>

      {/* LISTA */}
      <TareaList
        tareas={tareasFiltradas}
        eliminarTarea={eliminarTarea}
        toggleTarea={toggleTarea}
      />
    </div>
  );
}

export default App;
```

---

# 📌 Arquitectura React utilizada

```text
App.jsx
   ↓
TareaForm.jsx
   ↓
TareaList.jsx
   ↓
TareaItem.jsx
```

---

# 🟢 Componente Formulario

## 📄 components/TareaForm.jsx

```jsx
import { useState } from "react";

function TareaForm({ crearTarea }) {

  const [titulo, setTitulo] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();

    if (!titulo.trim()) return;

    crearTarea(titulo);

    setTitulo("");
  };

  return (
    <form
      onSubmit={handleSubmit}
      className="d-flex gap-2 mb-4"
    >
      <input
        className="form-control shadow-sm"
        value={titulo}
        onChange={(e) => setTitulo(e.target.value)}
        placeholder="✍️ Nueva tarea"
      />

      <button className="btn btn-primary">
        ➕ Agregar
      </button>
    </form>
  );
}

export default TareaForm;
```

---

# 📌 Explicación UX/UI

| Mejora | Beneficio |
|---|---|
| Sombras | Mejor profundidad |
| Íconos | Mayor comprensión |
| Espaciado | Mejor lectura |

---

# 🟢 Lista de tareas

## 📄 components/TareaList.jsx

```jsx
import TareaItem from "./TareaItem";

function TareaList({
  tareas,
  eliminarTarea,
  toggleTarea
}) {

  return (
    <div className="mt-3">
      {tareas.map((t) => (
        <TareaItem
          key={t.id}
          tarea={t}
          eliminarTarea={eliminarTarea}
          toggleTarea={toggleTarea}
        />
      ))}
    </div>
  );
}

export default TareaList;
```

---

# 🟢 Item individual

## 📄 components/TareaItem.jsx

```jsx
function TareaItem({
  tarea,
  eliminarTarea,
  toggleTarea
}) {

  return (
    <div
      className={`d-flex justify-content-between align-items-center p-3 border rounded mt-2 shadow-sm ${
        tarea.completada
          ? "bg-success text-white"
          : "bg-light"
      }`}
    >

      <div>
        <input
          type="checkbox"
          checked={tarea.completada}
          onChange={() => toggleTarea(tarea)}
          className="me-2"
        />

        {tarea.titulo}
      </div>

      <button
        className="btn btn-danger btn-sm"
        onClick={() => eliminarTarea(tarea.id)}
      >
        🗑️ Eliminar
      </button>
    </div>
  );
}

export default TareaItem;
```

---

# 🎨 Mejoras UX/UI aplicadas

## ✔️ Estados visuales
- Verde = completado
- Gris = pendiente

## ✔️ Botones visuales
- Íconos
- Hover de Bootstrap
- Separación clara

## ✔️ Jerarquía visual
- Cards
- Bordes redondeados
- Sombras suaves

---

# ✅ Checkpoint 2 — Build producción

```bash
npm run build
```

---

# 📌 ¿Qué hace este comando?

Genera una carpeta optimizada llamada:

```text
dist/
```

Esta carpeta será servida por NGINX.

---

# 🔐 Verificar permisos Linux

```bash
namei -l /home/ricardo/tecsup/desarrolloEmpresarial/lab11/frontend/dist/index.html
```

---

# 📌 Explicación

NGINX utiliza el usuario:

```text
www-data
```

Por eso necesitamos permisos de lectura.

---

# 🟢 Cambiar permisos

```bash
sudo chown usuario:www-data -R /usuario

sudo chown usuario:www-data -R /usuario
```

---

# 🧠 Flujo completo de React

```text
index.html
   ↓
main.jsx
   ↓
App.jsx
   ↓
Componentes
```

---

# 📌 Explicación técnica

| Archivo | Función |
|---|---|
| index.html | Punto inicial |
| main.jsx | Arranca React |
| App.jsx | Lógica principal |
| Components | UI reutilizable |

---

# 🚀 Resultado Final

Al finalizar tendrás:

✅ Frontend React profesional  
✅ Conexión con Django REST API  
✅ CRUD funcional  
✅ Bootstrap integrado  
✅ Arquitectura basada en componentes  
✅ Proyecto listo para producción  

---

# 🧠 Recomendaciones Profesionales

## 🔍 Debugging
Usa siempre:

```text
F12 → Console → Network
```

Para verificar:
- APIs
- Errores
- Respuestas HTTP

---

# 🛡️ Buenas prácticas

- Verifica URLs
- Verifica CORS
- Revisa puertos
- Usa componentes reutilizables
- Mantén código modular

---

# 🎓 Conclusión

Ahora ya entiendes:

✅ Cómo React consume APIs  
✅ Cómo separar componentes  
✅ Cómo manejar estados  
✅ Cómo desplegar frontend moderno  

---

# 📘 Autor

Tutorial académico profesional orientado a React + Django REST Framework.
