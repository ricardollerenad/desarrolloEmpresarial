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
