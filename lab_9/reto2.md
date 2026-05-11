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

Editar el archivo:

```bash
src/index.js
```

---

## 📌 ¿Qué debes modificar?

Buscar:

```javascript
import React from 'react';
import ReactDOM from 'react-dom/client';
import './index.css';
import App from './App';
```

---

## ✅ Agregar Bootstrap

Debajo de los imports agregar:

```javascript
import 'bootstrap/dist/css/bootstrap.min.css';
```

---

## ✅ Resultado final esperado

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

# 4️⃣ Crear aplicación React

Editar:

```bash
src/App.js
```

---

# ❌ Eliminar todo el contenido anterior

Debes borrar completamente el contenido que viene por defecto.

---

# ✅ Reemplazar por el siguiente código

```javascript
import { useEffect, useState } from 'react';

function App() {

  const [datos, setDatos] = useState([]);

  useEffect(() => {

    fetch('http://IP_VPS:8000/api/estudiantes/')
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

# 📌 IMPORTANTE — Cambiar la IP del servidor

Buscar esta línea:

```javascript
fetch('http://IP_VPS:8000/api/estudiantes/')
```

---

## ✅ ¿Qué debe modificar el alumno?

Debe reemplazar:

```text
IP_VPS
```

por:

- la IP de su servidor
- localhost
- o la IP de su máquina virtual

---

## ✅ Ejemplos

### Si trabaja localmente:

```javascript
fetch('http://127.0.0.1:8000/api/estudiantes/')
```

o

```javascript
fetch('http://localhost:8000/api/estudiantes/')
```

---

### Si usa una VPS:

```javascript
fetch('http://192.168.1.50:8000/api/estudiantes/')
```

---

# 5️⃣ Configurar CORS en Django

React y Django trabajan en puertos diferentes.

Por ello Django bloqueará las peticiones si no se configura CORS.

---

# ✅ Instalar django-cors-headers

Desde el backend ejecutar:

```bash
pip install django-cors-headers
```

---

# 6️⃣ Modificar `settings.py`

Editar:

```bash
backend/settings.py
```

---

## ✅ Agregar en INSTALLED_APPS

Buscar:

```python
INSTALLED_APPS = [
```

Agregar:

```python
'corsheaders',
```

---

## ✅ Resultado esperado

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

# 7️⃣ Agregar Middleware

Buscar:

```python
MIDDLEWARE = [
```

Agregar al inicio:

```python
'corsheaders.middleware.CorsMiddleware',
```

---

## ✅ Resultado esperado

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

# 8️⃣ Permitir conexión desde React

Al final de `settings.py` agregar:

```python
CORS_ALLOW_ALL_ORIGINS = True
```

---

# ⚠️ IMPORTANTE

Esto solo debe usarse en desarrollo.

En producción se recomienda:

```python
CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",
]
```

---

# ▶️ 9️⃣ Ejecutar Django

Desde la carpeta backend:

```bash
python manage.py runserver 0.0.0.0:8000
```

---

# ▶️ 🔟 Ejecutar React

Desde la carpeta frontend ejecutar:

```bash
npm start
```

---

# ✅ Verificar Frontend

Abrir:

```text
http://localhost:3000
```

o:

```text
http://IP_VPS:3000
```

---

# ✅ Resultado esperado

Deberías visualizar:

- Tabla Bootstrap
- Datos obtenidos desde Django
- Lista de estudiantes desde la API
- Información almacenada en MySQL

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
- Validar datos con serializers
- Usar HTTPS en producción

---

## ✅ Frontend

- Separar componentes
- Usar Axios en lugar de fetch
- Implementar manejo de errores
- Crear carpetas:
  - components
  - services
  - pages

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
- 📱 Diseño responsive

---

# 🏁 Resultado Final

Al finalizar tendrás:

✅ API REST funcional  
✅ Base de datos MySQL conectada  
✅ Frontend React consumiendo datos  
✅ Arquitectura fullstack moderna  
✅ Proyecto listo para escalar  
✅ Comunicación React + Django funcionando
