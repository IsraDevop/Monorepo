# CLAUDE.md — Buenas Prácticas Frontend (Web + Mobile) · Proyecto YALA
### Curso CS2031 — Desarrollo Basado en Plataformas (DBP)

Este documento es la **guía durable** del proyecto: reúne todas las buenas prácticas del
curso (web y mobile), reorganizadas por tema y contrastadas contra la rúbrica del Proyecto 2.
Define el "cómo" se construye, independiente de la tarea concreta. Las instrucciones de una
funcionalidad específica van en su propio archivo o issue, no aquí.

**Stack de referencia:** Web → React + Vite + TypeScript · Mobile → React Native + Expo · Backend → Spring Boot.

> Convención de origen (informativa): ✅ = práctica base del curso · ➕ = añadido por rúbrica
> o necesidad del proyecto. Puedes borrar las marcas.

---

## 1. Flujo de trabajo Git (obligatorio)

- **Metodología Git Flow.** Crea **una rama por cada feature** (`feature/nombre-descriptivo`).
  Trabaja siempre fuera de `main`; integra mediante Pull Request.
- **Issues por requerimiento.** Crea un **issue por cada nuevo requerimiento/feature**, y
  **vincúlalo** con su rama y con los commits correspondientes (referencia el número de issue,
  p. ej. `#12`, para que GitHub los enlace y cierre automáticamente).
- **Sin comentarios en commits ni en PRs.** No agregues ningún comentario/descripción extra en
  los mensajes de commit ni en la descripción de los Pull Requests más allá de lo estrictamente
  necesario para el título.
- **GitHub Projects y Milestones.** Usa GitHub Projects para el tablero de flujo y Milestones
  para agrupar entregas/hitos. Cada repo tiene su propio Project donde se visualiza el avance.
- **Sigue las indicaciones de `@DOCS.md`** del proyecto.

---

## 2. Resumen del curso por módulos (contexto)

### Semana 9 — Fundamentos del Cliente (Web)
- **Arquitectura Cliente-Servidor:** peticiones HTTP/HTTPS (Request/Response), consumo de APIs REST.
- **Tecnologías base:** HTML (estructura), CSS (estilos), JavaScript (lógica + DOM), TypeScript (tipado).
- **Herramientas:** TailwindCSS para estilización rápida; Axios para consumo de APIs con promesas.
- **Asincronía:** `async/await` para no bloquear la interfaz al consumir servicios externos.

### Semana 10 — React I
- **Concepto:** librería por componentes y programación declarativa con Virtual DOM.
- **JSX/TSX:** sintaxis que combina HTML + lógica JS/TS en un componente.
- **Props:** paso de datos (valores, funciones, hijos) de padre a hijo.
- **Listas y condicionales:** `.map()` para listas; `&&` y ternarios para render condicional.
- **Eventos y routing:** `onClick/onChange/onSubmit`; `react-router-dom` para navegación web.

### Semana 11 — React II (Hooks)
- **`useState`** (estado en componentes funcionales) y **`useEffect`** (sincronización con sistemas externos).
- **Patrones de estado:** *Lifting State Up* y **Context API** (`useContext`) contra *prop drilling*.
- **Custom Hooks:** funciones propias (`useAlgo`) para encapsular y reutilizar lógica.

### Semanas 12–13 — React Native y Mobile
- **React Native:** desarrollo móvil multiplataforma (iOS/Android) con componentes nativos reales.
- **Expo:** Expo Go y Expo Router abstraen la configuración nativa.
- **Navegación:** Expo Router (basado en archivos) y React Navigation (Stack, Drawer, Bottom/Top Tabs)
  envolviendo en `NavigationContainer`.
- **Componentes core:** `View`, `Text`, `FlatList`, `ScrollView` (no etiquetas HTML). UI con
  **React Native Paper** (Material Design).
- **Seguridad:** tokens en **Expo SecureStore** (Keychain/Keystore), nunca `AsyncStorage`.

### Semana 14 — Hardware y Sensores con Expo
- **Expo Sensors:** Acelerómetro (ejes x/y/z), Magnetómetro (brújula, microteslas), Podómetro (pasos).
- **Multimedia y localización:** `ImagePicker`, `Camera`, `Location` (GPS asíncrono), `Audio`.

---

## 3. Convenciones generales

- ✅ **Tipado estricto con TypeScript.** Declara `interface`/`type` para el *request body* y la
  *response* de cada endpoint. Aprovecha el chequeo estático.
- ➕ **Nombres descriptivos en inglés** (`getUserBookings`, no `traerReservas`). La rúbrica lo evalúa.
- ✅ **PascalCase** para componentes (`FlightTable`); **camelCase** para funciones/variables.
- ➕ **Código limpio**, sin comentarios innecesarios ni código muerto.
- ✅ **Variables de entorno** para URLs base. Nunca hardcodear el backend.

```ts
// types/flight.ts — interfaces reutilizables
export interface Flight {
  id: number;
  flightNumber: string;
  airline: string;
  departure: string; // ISO date
  arrival: string;
  availableSeats: number;
}

export interface LoginRequest { email: string; password: string; }
export interface LoginResponse { token: string; }
```

---

## 4. Arquitectura y estructura de carpetas

- ✅ **Separación de capas:** API/servicios (axios) ↔ lógica de negocio (hooks/context) ↔ UI
  (componentes presentacionales). La UI nunca llama a axios directamente.
- ➕ **+10 componentes reutilizables** (web) y separación **presentacional vs. contenedor**.
- ➕ **SRP:** componentes pequeños y enfocados; un componente, una responsabilidad.
- ✅ **Carpetas lógicas** con nombres claros.

```text
src/
├── api/          # instancia de axios + interceptores
├── components/   # UI reutilizable (Navbar, SearchForm, FlightTable...)
├── context/      # estado global (AuthContext)
├── hooks/        # custom hooks (useFetch, useAuth, useForm, useDebounce)
├── pages/        # pantallas asociadas a rutas (Login, Flights, MyBookings)
├── services/     # comunicación con el backend (authService, flightService)
├── utils/        # utilidades (apiError, storage, formatters)
├── types/        # interfaces y tipos compartidos
├── App.tsx       # define rutas y estructura principal
└── main.tsx      # punto de entrada (monta React)
```

Mobile (Expo) usa el mismo criterio pero con `screens/` en lugar de `pages/`.

---

## 5. Capa de consumo de API (Axios)

- ➕ **Instancia única de axios** con `baseURL` centralizada (vía env var) y headers por defecto.
- ➕ **Interceptores:** *request* inyecta el token; *response* maneja errores globales
  (401 → cerrar sesión / refrescar token). La rúbrica lo pide en web **y** mobile.
- ➕ **Refresh tokens** para renovar el JWT sin re-login.
- ➕ **AbortController** para cancelar requests al desmontar y evitar *memory leaks* / *race conditions*.
- ✅/➕ **Los 5 métodos HTTP** según corresponda: GET, POST, PUT, DELETE, **PATCH**.
- ➕ **Sin datos hardcodeados ni en memoria** para funcionalidades core: todo sale de la API.
- ✅ **Asincronía con `async/await`** envuelta en `try/catch` (ver §10).

```ts
// api/axios.ts
import axios from "axios";

export const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL,        // env var, no hardcode (prod: https://yala.dpdns.org/api/v1/)
  headers: { "Content-Type": "application/json" },
});

// Interceptor de request: inyecta el token en cada llamada
api.interceptors.request.use((config) => {
  const token = localStorage.getItem("token");
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// Interceptor de response: manejo centralizado de errores
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response?.status === 401) {
      localStorage.removeItem("token");
      window.location.href = "/login"; // sesión expirada → al login
    }
    return Promise.reject(error);
  }
);
```

```ts
// services/flightService.ts — un service por dominio
import { api } from "../api/axios";
import type { Flight } from "../types/flight";

export const flightService = {
  // AbortController evita actualizar estado de un componente ya desmontado
  search: (query: string, signal?: AbortSignal) =>
    api.get<Flight[]>(`/flights/search?q=${query}`, { signal }),

  book: (flightId: number) => api.post("/flights/book", { flightId }),
};
```

---

## 6. Autenticación y sesión (JWT)

- ➕ **AuthContext** como estado global de sesión (token, usuario, `login()`, `logout()`).
- ➕ **Persistir el token y rehidratar el estado al montar la app.**
  👉 Resuelve el bug de **"se desloguea al recargar"**: al refrescar, el estado de React se
  reinicia, pero si lees el almacenamiento en el arranque, la sesión sobrevive.
- ✅/➕ **Logout:** elimina el token, limpia el estado y redirige a `/login`.
- ➕ **Rutas protegidas:** sin token → redirección obligatoria al login (ver §9).

```tsx
// context/AuthContext.tsx
import { createContext, useContext, useState, useEffect, ReactNode } from "react";

interface AuthState {
  token: string | null;
  isAuthenticated: boolean;
  login: (token: string) => void;
  logout: () => void;
}

const AuthContext = createContext<AuthState | undefined>(undefined);

export function AuthProvider({ children }: { children: ReactNode }) {
  const [token, setToken] = useState<string | null>(null);

  // REHIDRATACIÓN: al montar, recupera el token persistido → no se cae la sesión al recargar
  useEffect(() => {
    const saved = localStorage.getItem("token");
    if (saved) setToken(saved);
  }, []);

  const login = (newToken: string) => {
    localStorage.setItem("token", newToken);
    setToken(newToken);
  };

  const logout = () => {
    localStorage.removeItem("token");
    setToken(null);
  };

  return (
    <AuthContext.Provider value={{ token, isAuthenticated: !!token, login, logout }}>
      {children}
    </AuthContext.Provider>
  );
}

export const useAuth = () => {
  const ctx = useContext(AuthContext);
  if (!ctx) throw new Error("useAuth debe usarse dentro de <AuthProvider>");
  return ctx;
};
```

> ⚠️ **Nota de seguridad (importante para la rúbrica).** Guardar el JWT en `localStorage` es lo
> más simple pero **vulnerable a XSS**. Para máxima seguridad: **cookies httpOnly** o **token en
> memoria + refresh token**, y en **mobile usa siempre `Expo SecureStore`** (cifrado nativo),
> nunca `AsyncStorage`/`localStorage`. Elige según contexto: simplicidad vs. seguridad.

---

## 7. Hooks de React

- ✅ **Reglas de los hooks:** llámalos siempre en el nivel superior; nunca dentro de loops,
  condicionales o funciones anidadas.
- ✅ **Inmutabilidad:** nunca mutes el estado directo; usa el *setter*. Para updates basados en el
  valor previo, usa función de actualización: `setCount(c => c + 1)`.
- ✅ **`useEffect` correcto:** define bien el array de dependencias (`[]` = una vez; sin array =
  cada render; `[a]` = cuando cambia `a`). No lo uses para datos que puedes calcular en el render.
  Limpia suscripciones en el `return`.
- ✅ **`useContext`:** evita *prop drilling* cuando un estado cruza más de 3 niveles.
- ➕ **`useReducer`:** para estado complejo o con varias transiciones (formularios largos, wizards).
- ➕ **`useMemo` / `useCallback`:** memoiza cálculos costosos y funciones pasadas como props.
- ✅/➕ **Custom hooks** para lógica repetida: `useAuth`, `useFetch`, `useForm`, `useDebounce`.

```ts
// hooks/useFetch.ts — abstrae fetch + loading + error + cancelación
import { useState, useEffect } from "react";
import { api } from "../api/axios";

export function useFetch<T>(url: string) {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<string | null>(null);

  useEffect(() => {
    const controller = new AbortController();
    (async () => {
      try {
        setLoading(true);
        const res = await api.get<T>(url, { signal: controller.signal });
        setData(res.data);
      } catch (err: any) {
        if (err.name !== "CanceledError") setError("No se pudo cargar la información.");
      } finally {
        setLoading(false);
      }
    })();
    return () => controller.abort(); // cleanup: cancela si el componente se desmonta
  }, [url]);

  return { data, loading, error };
}
```

---

## 8. Componentes y JSX/TSX

- ✅ **Nomenclatura capitalizada (PascalCase)** para que React los distinga de etiquetas HTML.
- ✅ **Retorno de un solo elemento raíz.** Usa Fragments `<> ... </>` para agrupar sin nodos extra.
- ✅ **Keys únicas** en listas con `.map()`. Usa el `id` real, **no el índice** del array.
- ➕ **Props bien tipadas** con TypeScript.
- ✅ **Renderizado condicional** con `&&` y ternarios.

```tsx
interface FlightTableProps {
  flights: Flight[];
  onBook: (id: number) => void;
}

export function FlightTable({ flights, onBook }: FlightTableProps) {
  if (flights.length === 0) return <EmptyState message="No se encontraron vuelos." />;
  return (
    <table>
      <tbody>
        {flights.map((flight) => (            // key = id real, no índice
          <tr key={flight.id}>
            <td>{flight.flightNumber}</td>
            <td>{flight.airline}</td>
            <td>
              <button onClick={() => onBook(flight.id)}>Reservar</button>
            </td>
          </tr>
        ))}
      </tbody>
    </table>
  );
}
```

---

## 9. Routing y navegación (web)

- ✅/➕ **React Router** con **rutas dinámicas** (`/flights/:id`) y **query params** (`?page=1&search=lima`).
- ➕ **Rutas protegidas** (`PrivateRoute`): sin token → redirección al login.
- ➕ **Lazy loading** de componentes con `React.lazy` + `Suspense`.
- ➕ **Navegación programática** con `useNavigate`.
- ➕ **Manejo de 404** con una ruta catch-all.
- ➕ **Configuración de rutas centralizada** en `App.tsx`.

```tsx
// components/PrivateRoute.tsx
import { Navigate } from "react-router-dom";
import { useAuth } from "../context/AuthContext";

export function PrivateRoute({ children }: { children: JSX.Element }) {
  const { isAuthenticated } = useAuth();
  return isAuthenticated ? children : <Navigate to="/login" replace />;
}
```

```tsx
// App.tsx — rutas centralizadas + lazy loading + 404
import { lazy, Suspense } from "react";
import { Routes, Route } from "react-router-dom";

const Flights = lazy(() => import("./pages/Flights"));
const MyBookings = lazy(() => import("./pages/MyBookings"));

export default function App() {
  return (
    <Suspense fallback={<Spinner />}>
      <Routes>
        <Route path="/login" element={<Login />} />
        <Route path="/flights" element={<Flights />} />
        <Route
          path="/my-bookings"
          element={<PrivateRoute><MyBookings /></PrivateRoute>}
        />
        <Route path="*" element={<NotFound />} />   {/* 404 */}
      </Routes>
    </Suspense>
  );
}
```

---

## 10. Manejo de errores, estados de carga y validaciones (web)

- ✅/➕ **`try/catch` en TODAS las llamadas API.**
- ➕ **Error boundaries** para errores de **renderizado** (el `try/catch` no los atrapa).
- ➕ **Mensajes amigables al usuario** (no técnicos). El usuario nunca ve la consola.
- ➕ **Estados de carga** con indicadores visuales (spinners, skeletons).
- ➕ **Diferenciar códigos HTTP:** 400, 401, 403, 404, 409, 500 con respuestas distintas.
- ➕ **Validación client-side:** valida campos vacíos **antes** de enviar; usa una librería
  (**React Hook Form** / Formik) o validación custom, con feedback en tiempo real.
- ➕ **Feedback de éxito** (toasts/confirmaciones); p. ej. mostrar el ID de la reserva creada.
- ✅/➕ **Casos edge:** listas vacías, sin resultados, sin conexión → mensaje claro.
- ➕ **Deshabilitar botones durante el request** para evitar doble submit.
- ➕ **Retry logic** para errores recuperables (timeouts, 5xx).

```ts
// utils/apiError.ts — traduce el error a un mensaje amigable según el código HTTP
export function getApiErrorMessage(error: any): string {
  const status = error.response?.status;
  switch (status) {
    case 400: return "Datos inválidos. Revisa el formulario.";
    case 401: return "Tu sesión expiró. Inicia sesión de nuevo.";
    case 403: return "No tienes permiso para esta acción.";
    case 404: return "No se encontró el recurso solicitado.";
    case 409: return "Conflicto: el horario se superpone con otra reserva.";
    case 500: return "Error del servidor. Intenta más tarde.";
    default:  return error.message?.includes("Network")
      ? "Sin conexión. Verifica tu internet."
      : "Ocurrió un error inesperado.";
  }
}
```

```tsx
// Uso típico en una acción, con loading + doble-submit + feedback
async function handleBook(flightId: number) {
  try {
    setSubmitting(true);                 // deshabilita el botón
    const { data } = await flightService.book(flightId);
    toast.success(`Reserva creada. ID: ${data.bookingId}`);
  } catch (err) {
    toast.error(getApiErrorMessage(err));
  } finally {
    setSubmitting(false);
  }
}
// <button disabled={submitting} onClick={() => handleBook(id)}>Reservar</button>
```

```tsx
// components/ErrorBoundary.tsx — atrapa errores de renderizado
import { Component, ReactNode } from "react";

export class ErrorBoundary extends Component<{ children: ReactNode }, { hasError: boolean }> {
  state = { hasError: false };
  static getDerivedStateFromError() { return { hasError: true }; }
  render() {
    return this.state.hasError
      ? <p>Algo salió mal. Recarga la página.</p>
      : this.props.children;
  }
}
```

---

## 11. UI/UX y diseño visual (web)

- ✅/➕ **Framework CSS** (Tailwind, Material-UI, Chakra...) o CSS/SCSS bien organizado.
  Paleta coherente, tipografía legible, espaciado consistente.
- ➕ **Responsive design (mobile / tablet / desktop).** Diseña *mobile-first* y valida en los tres
  tamaños. Con Tailwind: usa breakpoints `sm: md: lg:`. **El frontend debe ser responsive tanto en web como en mobile.**
- ➕ **Accesibilidad básica:** contraste adecuado, `alt` en imágenes, navegación por teclado, labels.
- ➕ **Búsqueda con debounce**, filtros y ordenamiento en listados.
- ➕ **Confirmaciones en acciones destructivas** (eliminar, cancelar reserva).
- ➕ **Estados vacíos con CTA** (no solo "no hay datos", sino "Busca tu primer vuelo →").
- ✅ **Iconos consistentes**, animaciones sutiles, dark mode (bonus).
- ✅ **Flujos claros:** Registro → Login → Búsqueda → Reserva.
- ➕ **Librerías UI:** prioriza librerías de componentes (DaisyUI, shadcn/ui, Aceternity en web)
  para acelerar el desarrollo y mantener consistencia, en lugar de escribir todo el CSS desde cero.

```tsx
// Responsive con Tailwind (mobile-first): 1 col en móvil, 2 en tablet, 3 en desktop
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 px-4 md:px-8">
  {flights.map((f) => <FlightCard key={f.id} flight={f} />)}
</div>
```

```ts
// hooks/useDebounce.ts — evita disparar la búsqueda en cada tecla
import { useState, useEffect } from "react";

export function useDebounce<T>(value: T, delay = 400): T {
  const [debounced, setDebounced] = useState(value);
  useEffect(() => {
    const timer = setTimeout(() => setDebounced(value), delay);
    return () => clearTimeout(timer);
  }, [value, delay]);
  return debounced;
}
```

---

## 12. Paginación (web)

- ➕ Paginación en **al menos 2 endpoints** que lo requieran.
- ➕ **Controles intuitivos** (anterior / siguiente / números de página).
- ➕ **Info de paginación:** "Mostrando 1–10 de 240 resultados".
- ➕ **Tamaño de página** configurable (10 / 25 / 50 / 100).
- ➕ **Estado en query params** (`?page=2&size=25`) para URLs compartibles.
- ➕ Maneja correctamente la **respuesta paginada** del backend (`total`, `page`, `size`, `content`).

```ts
export interface PageResponse<T> {
  content: T[];
  page: number;
  size: number;
  total: number;
}
// flightService.search(query, { params: { page, size } })
```

---

## 13. Mobile (React Native / Expo)

### 13.1 Integración con backend
- ➕ Consumir **exclusivamente** el backend core (`https://yala.dpdns.org/api/v1/`); todos los métodos HTTP.
- ✅/➕ JWT seguro con **`Expo SecureStore`** (Keychain en iOS, Keystore en Android). **Nunca**
  `AsyncStorage`/`LocalStorage` para tokens.
- ➕ Headers (`Authorization`, `Content-Type`) e **interceptores** centralizados.

### 13.2 Arquitectura
- ➕ Capas separadas (services / hooks-context / UI), custom hooks, Context API o state management.
- ✅ Componentes funcionales con hooks; carpetas `screens, components, services, hooks, contexts, utils`.

### 13.3 Navegación
- ✅ **Expo Router** (basado en archivos: `_layout.tsx`, `index.tsx`, `[id].tsx`) o **React
  Navigation** (Stack, Drawer, Bottom/Top Tabs) envolviendo en `NavigationContainer`.
- ✅ **Importaciones condicionales:** carga `react-native-gesture-handler` solo en nativo, no en web.

### 13.4 Sensores y APIs externas
- ➕/✅ Implementa **≥2 sensores** con valor real (cámara/QR, GPS, acelerómetro, micrófono...).
- ➕ Consume **≥1 API externa relevante** (mapas, clima, pagos).
- ✅ **Verifica disponibilidad** con `Sensor.isAvailableAsync()` antes de suscribir. Emuladores y
  algunos dispositivos físicos pueden no tener el sensor → muestra un mensaje en la UI, no solo `console.log`.
- ✅ **Gestión de permisos explícita:** solicita y valida `granted` antes de acceder al hardware
  (`requestForegroundPermissionsAsync()` o plugins en `app.json`).
- ✅ **`try/catch`** en operaciones de hardware asíncronas (GPS, audio, cámara): pueden fallar por
  causas externas (GPS apagado, hardware ocupado, timeout).
- ✅ **Cleanup de suscripciones:** guarda la suscripción y llama `.remove()` en el `return` del
  `useEffect` para evitar llamadas fantasma y fugas de memoria al cambiar de pantalla.
- ✅ **Feedback de disponibilidad/permiso en la UI**, no en `console.log`. El usuario nunca ve la consola.
- ✅ **Interface** explícita para los datos del sensor (no tipos inline).
- ✅ **Custom hook** para sensores reutilizables (centraliza suscripción, baja y reglas de limpieza).

```ts
// hooks/useSensor.ts — abstrae la lógica común de sensores
import { useState, useEffect } from "react";

interface SensorCoordinate { x: number; y: number; z: number; }

function useSensor<T>(Sensor: any, initialValue: T) {
  const [data, setData] = useState<T>(initialValue);
  const [subscription, setSubscription] = useState<any>(null);

  function subscribe() { setSubscription(Sensor.addListener(setData)); }
  function unsubscribe() { subscription?.remove(); setSubscription(null); }

  useEffect(() => { subscribe(); return () => unsubscribe(); }, []);
  return { data, subscription, subscribe, unsubscribe };
}
```

```ts
// Verificación + permisos + cleanup + feedback en UI
import { Accelerometer } from "expo-sensors";
import * as Location from "expo-location";

interface SensorCoordinate { x: number; y: number; z: number; }

async function startAccelerometer(
  setData: (d: SensorCoordinate) => void,
  setError: (m: string) => void
) {
  const available = await Accelerometer.isAvailableAsync();
  if (!available) { setError("Sensor no disponible en este dispositivo"); return; }
  return Accelerometer.addListener(setData); // guarda la suscripción para .remove()
}

async function getLocation(setError: (m: string) => void) {
  const { status } = await Location.requestForegroundPermissionsAsync();
  if (status !== "granted") {
    setError("Permiso denegado. Actívalo en Configuración > Aplicaciones."); // UI, no consola
    return;
  }
  try {
    return await Location.getCurrentPositionAsync({});
  } catch {
    setError("No se pudo obtener la ubicación. Verifica que el GPS esté activo.");
  }
}
```

### 13.5 UI mobile
- ✅ Prioriza **React Native Paper** (Material Design) sobre `StyleSheet` manual para componentes
  comunes (`Button`, `Card`, `Surface`, `TextInput`): acelera y da consistencia/accesibilidad.

---

## 14. Deployment y seguridad

- ➕ **Backend** en cloud, desplegado en `https://yala.dpdns.org/api/v1/`, con base de datos administrada.
- ➕ **Frontend web** en **AWS Amplify**; **mobile** publicada vía Expo / TestFlight / Play Console.
- ➕ **CI/CD:** workflow de GitHub Actions que se dispara con cambios en `main` y **actualiza el
  frontend web en AWS Amplify** automáticamente.
- ➕ **Variables de entorno en producción**, nunca hardcodeadas. En Vite, las variables del cliente
  llevan prefijo `VITE_` (p. ej. `VITE_API_URL`).
- ➕ **Secrets y API keys seguros:** fuera del repo (`.gitignore` del `.env`), en el panel de
  secrets del proveedor.
- ➕ **HTTPS** en todos los servicios y **JWT funcionando end-to-end**.
- ➕ Latencia promedio < 500 ms en endpoints principales.

### ⚠️ CORS ↔ variables de entorno (fuente típica de bugs)

El error de CORS casi siempre nace de un **desajuste de URLs entre entornos**, controlado con
variables de entorno en **ambos lados**:

- **Frontend:** la URL del backend va en una env var (`VITE_API_URL`), no hardcodeada.
  - Dev: `VITE_API_URL=http://localhost:8080`
  - Prod: `VITE_API_URL=https://yala.dpdns.org/api/v1/`
- **Backend:** debe **permitir explícitamente el origen del frontend** en su config de CORS.
  - Dev: `http://localhost:5173` (puerto por defecto de Vite)
  - Prod: la URL real del frontend desplegado en AWS Amplify.
  - Si el origen permitido no coincide con dónde corre el front, el navegador **bloquea** la
    petición con error de CORS.

**La regla:** las URLs de ambos lados deben venir de env vars y **coincidir por entorno**. Front
apunta al backend correcto; backend permite el origen exacto del front. Si una de las dos está
hardcodeada o desfasada → CORS.

```bash
# .env (NO subir al repo)
VITE_API_URL=http://localhost:8080
```

```java
// Backend (Spring Boot) — orígenes desde variable de entorno, no hardcodeados
@CrossOrigin(origins = "${app.cors.allowed-origins}")
// application.properties:  app.cors.allowed-origins=${FRONTEND_URL:http://localhost:5173}
```

---

## 15. Exposición / Presentación del proyecto (pitch + demo)

La rúbrica evalúa la presentación con **5 puntos** (sección individual). Pitch tipo Apple,
**20 minutos**, PPT profesional + **demo en vivo**.

**Estructura sugerida (20 min):**
1. **Introducción (1 min):** sin perder tiempo presentando integrantes. Abre con un **gancho** que
   plantee un problema real y entretenido.
2. **Problema (2 min):** claro y conciso — por qué es relevante, a quién afecta y cómo.
3. **Solución (4 min):** nombre y objetivos de la app; **características clave** (qué hace). Apóyate
   en *mocks*; la demo cubre lo real.
4. **Arquitectura y tecnologías (2 min):** breve y directa, con **diagramas**.
5. **Demo en vivo (6 min):** con **datos ya preparados** (evita el registro en vivo; haz login con
   un usuario creado). Muestra login → dashboard → **flujo principal**. Practica para que sea fluido.
6. **Ronda de preguntas (5 min):** preguntas **técnicas** de backend y frontend, y fragmentos de
   código. **Justifica** cualquier tecnología extra a las del curso.

**Para puntaje máximo:** dominio completo del proyecto (teórico y técnico), PPT clara y profesional,
demo sin fallos mostrando todo lo prometido, presentación dinámica con gancho atractivo.

---

## Checklist rápido antes de entregar

- [ ] Git flow: rama por feature, issues vinculados a ramas/commits, sin comentarios en commits/PRs
- [ ] GitHub Projects + Milestones por repo; flujo visible; indicaciones de `@DOCS.md` seguidas
- [ ] Instancia única de axios + interceptores (request token / response 401)
- [ ] AuthContext con rehidratación al montar (no se desloguea al recargar)
- [ ] Rutas protegidas + 404 + lazy loading
- [ ] `try/catch` + error boundary + mensajes amigables + códigos HTTP diferenciados
- [ ] Loading states (spinners/skeletons), toasts de éxito, anti doble-submit
- [ ] Validación client-side (campos vacíos antes de enviar) con feedback en tiempo real
- [ ] Responsive (mobile/tablet/desktop) + accesibilidad básica + estados vacíos con CTA
- [ ] Búsqueda con debounce, filtros y paginación (≥2 endpoints, estado en query params)
- [ ] +10 componentes reutilizables, hooks (incl. useReducer/useMemo/useCallback), nombres en inglés
- [ ] Mobile: ≥2 sensores + 1 API externa, permisos + fallbacks, SecureStore, cleanup de suscripciones
- [ ] Deploy: backend en `https://yala.dpdns.org/api/v1/`, web en AWS Amplify, env vars en prod,
      CORS alineado por entorno, HTTPS, JWT end-to-end, latencia < 500 ms
- [ ] Workflow CI/CD activo en `main` que actualiza el frontend web en AWS Amplify
- [ ] Presentación: gancho, demo con datos preparados, dominio del código para Q&A
