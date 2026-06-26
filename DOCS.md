# 📋 Guía Definitiva de Buenas Prácticas y Requisitos - Proyecto YALA (CS 2031 - DBP)

Este documento centraliza los estándares de calidad, arquitectura y flujo de trabajo necesarios para alcanzar el **puntaje máximo (20/20)** en la entrega final del proyecto[cite: 1]. Define la manera estricta de construir la solución para el ecosistema Web, Mobile y Backend[cite: 3].

---

## 0. Repositorios y Entorno de Trabajo
El ecosistema se divide en un monorepo lógico con tres repositorios enlazados[cite: 3]:

*   **Frontend Web (React + Vite + TS):** [Repositorio](https://github.com/Sebas-Leo/frontend_yala.git) - Desplegado en AWS Amplify[cite: 3].
*   **Frontend Mobile (React Native + Expo):** [Repositorio](https://github.com/IsraDevop/Frontend-Mobile-Proyecto2.git) - Publicado vía Expo (EAS / Expo Go)[cite: 3].
*   **Backend (Spring Boot):** [Repositorio](https://github.com/katealva/backend-yala) - Desplegado en `https://yala.dpdns.org/api/v1/` (URL Base de Producción)[cite: 3].

**Reglas de Entorno:**
*   Nunca "hardcodear" URLs. Utilizar siempre variables de entorno (ej. `VITE_API_URL` en web)[cite: 1, 3].
*   Debe existir un workflow de CI/CD (GitHub Actions) en la rama `main` que actualice automáticamente el frontend web en AWS Amplify[cite: 3].

---

## 1. Flujo de Trabajo Git (Git Flow)
*   **Ramas por Feature:** Trabajar siempre en ramas `feature/nombre-descriptivo` y fusionar a `main` exclusivamente mediante Pull Requests[cite: 3].
*   **Gestión de Issues:** Crear un issue por cada nuevo requerimiento, vinculándolo obligatoriamente a su rama y commits (ej. `#12`) para auto-cierre[cite: 3].
*   **Limpieza de Commits:** No agregar comentarios extra en mensajes de commit ni en PRs más allá del título estricto[cite: 3].
*   **Gestión de Proyectos:** Utilizar un GitHub Project independiente por cada repositorio y Milestones para trazar el flujo (issues → en progreso → en revisión → hecho)[cite: 3].

---

## 2. Frontend Web (React + Vite + TypeScript) [5 Puntos + 2 Puntos UI/UX]

### 2.1 Arquitectura y Componentes
*   **Stack Principal:** Uso estricto de React, Vite y TypeScript con tipado fuerte (interfaces/types para request y response)[cite: 3].
*   **Modularidad:** Implementar más de 10 componentes genuinamente reutilizables[cite: 1]. Mantener separación estricta entre componentes presentacionales y contenedores (SRP)[cite: 1, 3].
*   **Estructura de Carpetas:** Organizar lógicamente en `api/`, `components/`, `context/`, `hooks/`, `pages/`, `services/`, `utils/`, y `types/`[cite: 1, 3].
*   **Hooks de React:** Emplear `useState`, `useEffect` (con limpieza en el return), `useContext` (estado global cruzado), `useReducer` (estados complejos), y memoización con `useMemo`/`useCallback`[cite: 1, 3].
*   **Custom Hooks:** Encapsular lógica repetitiva (`useAuth`, `useFetch`, `useForm`, `useDebounce`)[cite: 1, 3].

### 2.2 Integración API y Sesión
*   **Axios Centralizado:** Crear una instancia única configurada con interceptores. El interceptor *request* inyecta el token; el *response* maneja errores globales (ej. redirección en 401 y refresh tokens)[cite: 1, 3].
*   **Memory Leaks:** Usar `AbortController` para cancelar requests al desmontar componentes[cite: 1, 3].
*   **Autenticación (JWT):** Implementar un `AuthContext`. Para evitar caídas de sesión al recargar, implementar lógica de rehidratación que lea el token en el montaje de la app[cite: 1, 3].

### 2.3 Routing y UI/UX
*   **Navegación:** Emplear React Router con rutas dinámicas, query params, `PrivateRoute` (protección de rutas), lazy loading (`React.lazy` + `Suspense`) y vista global 404[cite: 1, 3].
*   **Vistas:** Implementar al menos 4 vistas bien diferenciadas, incluyendo un dashboard estructurado y listados[cite: 1].
*   **Diseño Visual:** Aplicar frameworks CSS (Tailwind, Material-UI) o librerías de componentes (DaisyUI, shadcn/ui) siguiendo el patrón *mobile-first* (`sm:`, `md:`, `lg:`) para asegurar responsividad total[cite: 1, 3].
*   **Paginación:** Requerida en al menos 2 endpoints. Mantener el estado en los query params (`?page=1&size=25`) con manejo dinámico de la respuesta del backend[cite: 1, 3].
*   **Búsqueda y Feedback:** Implementar `useDebounce` en búsquedas, mostrar Empty States con Call-To-Actions, confirmaciones para acciones destructivas y accesibilidad básica (contraste, tabs, alts)[cite: 1, 3].

### 2.4 Manejo de Errores
*   **Try/Catch y Boundaries:** `try/catch` en *todas* las peticiones asíncronas y uso de Error Boundaries para fallos de renderizado[cite: 1, 3].
*   **Feedback Visual:** Proveer loaders, skeletons y evitar el "doble submit" deshabilitando botones[cite: 1, 3].
*   **Mensajes Amigables:** Traducir los códigos HTTP (400, 401, 404, 500) a mensajes limpios para el usuario final (ej. utilidad `apiError.ts`). Nunca mostrar la consola[cite: 1, 3].

---

## 3. Frontend Mobile (React Native + Expo) [5 Puntos]

### 3.1 Integración y Arquitectura Mobile
*   **Consumo Core:** Dependencia total de la API (`https://yala.dpdns.org/api/v1/`). Ningún dato harcodeado en memoria para funcionalidades principales[cite: 2, 3].
*   **Seguridad Estricta:** Uso obligatorio de **Expo SecureStore** para almacenar JWT. Queda prohibido el uso de AsyncStorage para datos sensibles[cite: 2, 3].
*   **Navegación Nivel Producción:** Emplear Expo Router o React Navigation (`NavigationContainer`, Stack/Tabs)[cite: 3].
*   **UI Nativa:** Utilizar componentes nativos (`View`, `Text`, `FlatList`) estilizados preferentemente con React Native Paper (Material Design)[cite: 3].

### 3.2 Sensores y Hardware
*   **Integración de Valor:** Implementar de forma correcta y justificada al menos 2 sensores (`expo-camera`, `expo-location`, `expo-sensors`, etc.)[cite: 2, 3].
*   **APIs Externas:** Consumir un mínimo de una API externa relevante (ej. mapas, clima)[cite: 2, 3].
*   **Permisos y Ciclo de Vida:** 
    *   Validar disponibilidad (`Sensor.isAvailableAsync()`) antes de suscribirse[cite: 3].
    *   Manejar permisos explícitos con mensajes de UI claros[cite: 2, 3].
    *   Garantizar el cleanup (limpieza) de suscripciones en el `return` del `useEffect` para evitar llamadas fantasma y memory leaks[cite: 3].
    *   Manejar hardware en bloques `try/catch` y crear fallbacks funcionales[cite: 2, 3].

---

## 4. Deployment, Seguridad y CORS [3 Puntos]
*   **Alineación de Entornos (CORS):** El backend debe estar configurado para permitir explícitamente el origen del frontend (ej. `http://localhost:5173` en Dev, Amplify URL en Prod) mediante variables de entorno para evitar bloqueos CORS[cite: 3].
*   **Manejo de Secrets:** Nunca pushear `.env`. Utilizar los paneles de secrets de AWS/Vercel/GitHub[cite: 1, 3].
*   **Performance:** Mantener HTTPS de extremo a extremo y latencia controlada (< 500ms en endpoints core)[cite: 1, 3].

---

## 5. Exposición del Proyecto y Pitch [4 Puntos]
*   **Formato de Presentación (20 minutos):** Estilo pitch corporativo (ej. Apple) evaluado individualmente[cite: 1].
*   **Estructura Estricta:**
    *   **1 min:** Gancho inicial directo sobre la problemática real, sin perder tiempo en presentar al equipo[cite: 1, 3].
    *   **2 min:** Planteamiento del problema[cite: 1, 3].
    *   **4 min:** Solución, características y propuesta de valor[cite: 1, 3].
    *   **2 min:** Arquitectura explicada mediante diagramas claros[cite: 1, 3].
    *   **6 min:** Demo interactiva fluida. **Obligatorio:** Tener usuarios y datos ya creados (no registrar en vivo) para mostrar la aplicación directamente[cite: 1, 3].
    *   **5 min:** Ronda de preguntas técnicas por parte de los profesores donde se deberá justificar las decisiones de tecnologías extra adoptadas[cite: 1, 3].