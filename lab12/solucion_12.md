# 📖 Tutorial Paso a Paso: Dashboard de Rick & Morty con React y Vite
¡Bienvenido al laboratorio! En esta guía aprenderás a construir una aplicación web estructurada, escalable y profesional utilizando React, Vite, React Router DOM y Bootstrap.Al finalizar, tendrás un sistema con arquitectura limpia capaz de consumir APIs externas, manejar estados globales de carga y filtrar información en tiempo real.

## 🛠️ PASO 1: Creación y Configuración del Proyecto
Vite es una herramienta moderna que ofrece un entorno de desarrollo ultra rápido. A diferencia de Create React App (CRA), Vite no instala las dependencias por defecto, por lo que debemos inicializarlas manualmente.Abre tu terminal y ejecuta los siguientes comandos:bash# 1. Crear el proyecto base con la plantilla de React en JavaScript
```bash
npm create vite@latest frontend -- --template react
cd frontend
npm install
npm install axios react-router-dom react-bootstrap bootstrap
```
Probar el codigo para hacerlo 
```bash
npm run dev -- --host 0.0.0.0
```

## 📂 PASO 2: Arquitectura del Proyecto
Para mantener el código limpio y mantenible a medida que el proyecto crece, utilizaremos una Arquitectura por Capas. Crea las siguientes carpetas dentro del directorio src/:textsrc/
<pre>
📂Frontend
│
├── assets/          # 📦 Archivos estáticos (Imágenes, logos, iconos)
│
├── components/      # 🧩 Componentes UI reutilizables (Piezas pequeñas, no páginas completas)
│   ├── Reto1Usuarios.jsx
│   ├── Reto2Productos.jsx
│   └── Reto3Dashboard.jsx
│
├── pages/           # 📄 Vistas/Pantallas completas del sistema que representan rutas
│   ├── Home.jsx
│   ├── Reto1Page.jsx
│   ├── Reto2Page.jsx
│   └── Reto3Page.jsx
│
├── layout/          # 🏗️ Estructura visual general y fija (Header, Sidebar, Footer)
│   ├── MainLayout.jsx   
│   ├── Header.jsx       
│   ├── Sidebar.jsx      
│   ├── Footer.jsx       
│   └── layout.css       
│
├── routes/          # 🚦 Gestión de rutas del sistema con React Router
│   └── AppRouter.jsx
│
├── services/        # 🌐 Centralización de llamadas HTTP (Lógica de consumo de APIs)
│   └── apiRickAndMorty.js 
│
├── hooks/           # 🎣 Custom Hooks (Separación de la lógica de negocio de la UI)
│   └── useCharacters.js
│
├── App.jsx          # 🚀 Componente raíz de la aplicación (Conector principal)
├── main.jsx         # 📌 Punto de entrada al DOM de React
└── index.css        # 🎨 Estilos globales de la aplicación
</pre>

```bash
    main.jsx --> App.jsx
    App.jsx --> AppRouter.jsx
    AppRouter.jsx --> MainLayout.jsx
    MainLayout.jsx --> Header & Sidebar & Footer
    MainLayout.jsx --> Pages
    Pages --> Components
    Components --> Hooks
    Hooks --> Services
```
## 💻 PASO 3: Implementación del Código

Sigue el orden establecido para construir la infraestructura de la aplicación antes de programar las interfaces de usuario.
📌 Capa de Configuración Global
src/main.jsx >> Punto de partida. Aquí importamos los estilos de Bootstrap para que estén disponibles en toda la app.jsx

### app.jsx
```javascript
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "bootstrap/dist/css/bootstrap.min.css"; // Estilos listos de Bootstrap
import "./index.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```
Se encarga únicamente de renderizar nuestro enrutador general.jsximport AppRouter from "./routes/AppRouter";

### /routes/AppRouter
```bash
function App() {
  return <AppRouter />;
}
export default App;
```
Establece qué página se debe mostrar en el navegador según la URL actual. Envuelve el contenido dentro del diseño base 

### src/layout/MainLayout
```javascript
import { BrowserRouter, Routes, Route } from "react-router-dom";
import MainLayout from "../layout/MainLayout";
import Home from "../pages/Home";
import Reto1Page from "../pages/Reto1Page";
import Reto2Page from "../pages/Reto2Page";
import Reto3Page from "../pages/Reto3Page";

export default function AppRouter() {
  return (
    <BrowserRouter>
      <MainLayout>
        <Routes>
          <Route path="/" element={<Home />} />
          <Route path="/reto1" element={<Reto1Page />} />
          <Route path="/reto2" element={<Reto2Page />} />
          <Route path="/reto3" element={<Reto3Page />} />
        </Routes>
      </MainLayout>
    </BrowserRouter>
  );
}
```

### src/layout/Header
Estructura contenedora que define la distribución del espacio usando componentes fijos y dinámicos (children).jsx

```javascript
import Header from "./Header";
import Sidebar from "./Sidebar";
import Footer from "./Footer";
import "./layout.css";

function MainLayout({ children }) {
  return (
    <div className="app-container">
      <Header />
      <div className="body-container">
        <Sidebar />
        <main className="content">
          {children}
        </main>
      </div>
      <Footer />
    </div>
  );
}

export default MainLayout;
Usa el código con precaución.src/layout/Header.jsxjsxfunction Header() {
  return (
    <header className="header">
      <h2>🛸 Rick & Morty Space Dashboard</h2>
    </header>
  );
}

export default Header;
```

### src/layout/Sidebar.jsx 
Utiliza el componente Link de react-router-dom para navegar entre pantallas sin recargar la página del navegador.jsx

```javascript
import { Link } from "react-router-dom";

function Sidebar() {
  return (
    <aside className="sidebar">
      <h4>Menú de Retos</h4>
      <ul>
        <li><Link to="/">🏠 Inicio</Link></li>
        <li><Link to="/reto1">👥 Reto 1: Personajes</Link></li>
        <li><Link to="/reto2">📦 Reto 2: Productos</Link></li>
        <li><Link to="/reto3">📊 Reto 3: Dashboard</Link></li>
      </ul>
    </aside>
  );
}

export default Sidebar;
```
### src/layout/Footer.jsx
```javascript
function Footer() {
  return (
    <footer className="footer">
      <p>© 2026 - Programación Frontend Avanzada | Proyecto React Vite</p>
    </footer>
  );
}

export default Footer;
```

### src/layout/layout.css
Estilos de estructuración mediante CSS Flexbox para asegurar pantallas adaptables de alto completo.css
```css
app-container {
  height: 100vh;
  display: flex;
  flex-direction: column;
}

.header {
  background: #111827;
  color: #10b981;
  padding: 15px;
  text-align: center;
  border-bottom: 2px solid #059669;
}

.body-container {
  display: flex;
  flex: 1;
  overflow: hidden;
}

.sidebar {
  width: 240px;
  background: #1f2937;
  color: white;
  padding: 20px;
}

.sidebar h4 {
  color: #9ca3af;
  font-size: 1.1rem;
  margin-bottom: 20px;
}

.sidebar ul {
  list-style: none;
  padding: 0;
}

.sidebar ul li {
  margin: 15px 0;
}

.sidebar a {
  color: #e5e7eb;
  text-decoration: none;
  font-weight: 500;
  transition: color 0.2s;
}

.sidebar a:hover {
  color: #10b981;
}

.content {
  flex: 1;
  padding: 25px;
  background: #f3f4f6;
  overflow-y: auto;
}

.footer {
  background: #111827;
  color: #9ca3af;
  text-align: center;
  padding: 10px;
  font-size: 0.9rem;
}
```
## 🌐 Capa de Abstracción de Datos (Servicios y Custom Hooks)

Para evitar acumular lógica dentro de los componentes visuales, separamos las responsabilidades. El servicio se encarga de HTTP, y el hook del estado de React

### .src/services/apiRickAndMorty.js

Centraliza la comunicación con la API de Rick & Morty mediante Axios.javascript

```javascript
import axios from "axios";

const BASE_URL = "https://rickandmortyapi.com/api";

export const getCharacters = async (name = "") => {
  try {
    const url = name 
      ? `${BASE_URL}/character/?name=${name}` 
      : `${BASE_URL}/character`;
    
    const response = await axios.get(url);
    return response.data.results || [];
  } catch (error) {
    console.error("Error fetching characters:", error.message);
    return []; // Retorna un array vacío si no encuentra coincidencias o hay error
  }
};
```
### src/hooks/useCharacters.js

Gestiona el ciclo de vida del estado de carga, almacenamiento de datos y control de peticiones.javascript
```javascript
import { useState, useEffect } from "react";
import { getCharacters } from "../services/apiRickAndMorty";

export function useCharacters() {
  const [personajes, setPersonajes] = useState([]);
  const [loading, setLoading] = useState(true);
  const [search, setSearch] = useState("");

  const fetchCharactersData = async (nombreQuery = "") => {
    setLoading(true);
    const data = await getCharacters(nombreQuery);
    setPersonajes(data);
    setLoading(false);
  };

  // Ejecución inicial al montar el hook
  useEffect(() => {
    fetchCharactersData();
  }, []);

  // Función manejadora para cuando el usuario escribe en el buscador
  const handleSearch = (text) => {
    setSearch(text);
    fetchCharactersData(text);
  };

  return {
    personajes,
    loading,
    search,
    handleSearch
  };
}
```
## 📄 Capa de Páginas (Vistas de la App)Las páginas actúan como orquestadoras de los retos específicos.
### src/pages/Home.jsxjsx
```javascript
import { Alert } from "react-bootstrap";

function Home() {
  return (
    <div>
      <h1>🚀 Panel de Control Principal</h1>
      <p className="text-muted">Bienvenido al entorno de desarrollo práctico.</p>
      <Alert variant="info">
        Selecciona un reto en el menú lateral para inicializar los módulos de datos de la API.
      </Alert>
    </div>
  );
}

export default Home;
```
## src/pages/Reto1Page.jsxjsx
```javascript
import Reto1Usuarios from "../components/Reto1Usuarios";

function Reto1Page() {
  return (
    <div>
      <h1 className="mb-4">Módulo de Personajes</h1>
      <Reto1Usuarios />
    </div>
  );
}

export default Reto1Page;
```
## src/pages/Reto2Page.jsxjsx
```javascript
import Reto2Productos from "../components/Reto2Productos";

function Reto2Page() {
  return (
    <div>
      <h1>Reto 2: Gestión de Inventario / Productos</h1>
      <Reto2Productos />
    </div>
  );
}

export default Reto2Page;
```

## src/pages/Reto3Page.jsxjsx
```javascript
import Reto3Dashboard from "../components/Reto3Dashboard";

function Reto3Page() {
  return (
    <div>
      <h1>Reto 3: Panel Métrico / Dashboard Avanzado</h1>
      <Reto3Dashboard />
    </div>
  );
}

export default Reto3Page;
```
## 🧩 Capa de Componentes UI Reutilizables

Aquí se diseña y renderiza la interfaz final usando los datos procesados por nuestros Hooks.

## src/components/Reto1Usuarios.jsx
Implementa la UI limpia usando las clases y elementos de cuadrícula (Row, Col) de React Bootstrap.jsx

```javascript
import React from "react";
import { useCharacters } from "../hooks/useCharacters";
import { Card, Row, Col, Spinner, Form, Container, Badge } from "react-bootstrap";

function Reto1Usuarios() {
  const { personajes, loading, search, handleSearch } = useCharacters();

  return (
    <Container className="p-0">
      {/* SECCIÓN DEL BUSCADOR */}
      <Form className="mb-4">
        <Form.Group controlId="searchForm">
          <Form.Label className="fw-bold">🔎 Filtrar por Nombre:</Form.Label>
          <Form.Control
            type="text"
            placeholder="Escribe el nombre de un personaje (ej: Rick, Morty, Summer...)"
            value={search}
            onChange={(e) => handleSearch(e.target.value)}
          />
        </Form.Group>
      </Form>

      {/* INDICADOR DE CARGA */}
      {loading && (
        <div className="d-flex justify-content-center my-5">
          <Spinner animation="border" variant="success" role="status" />
          <span className="ms-2 my-auto text-muted">Buscando en el multiverso...</span>
        </div>
      )}

      {/* RENDERIZADO DE RESULTADOS */}
      {!loading && (
        <Row>
          {personajes.length > 0 ? (
            personajes.map((p) => (
              <Col md={6} lg={4} key={p.id} className="mb-4">
                <Card className="h-100 shadow-sm">
                  <Card.Img variant="top" src={p.image} alt={p.name} />
                  <Card.Body>
                    <Card.Title className="fw-bold">{p.name}</Card.Title>
                    <Card.Text className="mb-1">
                      <strong>Especie:</strong> {p.species}
                    </Card.Text>
                    <div>
                      <strong>Estado: </strong>
                      <Badge bg={p.status === "Alive" ? "success" : p.status === "Dead" ? "danger" : "secondary"}>
                        {p.status}
                      </Badge>
                    </div>
                  </Card.Body>
                </Card>
              </Col>
            ))
          ) : (
            <Col className="text-center my-4">
              <p className="text-danger fw-bold">❌ No se encontraron personajes que coincidan con la búsqueda.</p>
            </Col>
          )}
        </Row>
      )}
    </Container>
  );
}

export default Reto1Usuarios;
```

```javascript
## src/components/Reto2Productos.jsx
Plantilla base lista para desarrollo del alumno.jsx


function Reto2Productos() {
  return (
    <div className="p-3 bg-white border rounded">
      <h3>Módulo en construcción</h3>
      <p className="text-muted">Implementa aquí tu lógica de administración para el Reto 2.</p>
    </div>
  );
}

export default Reto2Productos;
```
## src/components/Reto3Dashboard.jsx
Plantilla base lista para desarrollo del alumno.jsx
```javascript
function Reto3Dashboard() {
  return (
    <div className="p-3 bg-white border rounded">
      <h3>Módulo en construcción</h3>
      <p className="text-muted">Implementa aquí tus gráficos y contadores analíticos para el Reto 3.</p>
    </div>
  );
}

export default Reto3Dashboard;
```
# ⚙ Ponerlo en PRODUCCION

Dentro del directorio raiz escribe lo siguiente:
```bash
sudo npm run build
```
Carga ahora o modifica tu archivo de NGINX

### sudo nano /etc/nginx/conf.d/TU_NOMBRE_DE_ARCHIVO.conf
```NGINX
server {
    listen 80;
    listen [::]:80;
    server_name <<DOMINIO>>; 

    root <<RUTA>>/frontend/dist;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ /index.html;
    }

    location /assets/ {
        expires 1y;
        add_header Cache-Control "public, no-transform";
    }
}
```

Comprueba la configuracion y reinicia el sistema
```bash
sudo nginx -t
sudo systemctl restart nginx
```

Una vez comprobado que funciona, habilita el HTTPS con Letsencrypt 
```Bash
sudo apt install -y python3 python3-pip
sudo apt install -y certbot
sudo certbot --nginx --agree-tos --redirect --hsts --staple-ocsp --email tu_correo@gmail.com -d TU_DOMINIO
sudo nginx -t
sudo systemctl restart nginx
```

