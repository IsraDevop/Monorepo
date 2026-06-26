# 🃏 YALA — Frontend Tasks (Segunda Fase)
> **Proyecto:** CS 2031 - DBP · Segunda Fase · **20% nota final**  
> **Alcance de este doc:** Features pendientes de Frontend Web + Mobile  
> **Rúbrica cubierta:** Secciones 3 (Web, 5pts) · 4 (UI/UX, 2pts)

**Estados:** `🔴 Pendiente` · `🟡 En progreso` · `🟢 Listo` · `⚪ Bloqueado`  
**Asignar responsable** en cada tarea con `@nombre`

---

## 🖥️ FRONTEND WEB

---

### WEB-01 · Landing Page
**Estado:** 🔴 Pendiente | **Prioridad:** 🔥 Alta | **Responsable:** —  
**Rúbrica:** §4.3 Vistas y Funcionalidades (+1 vista diferenciada) · §4.1 Diseño Visual

**Contexto:**  
Primera impresión de Yala. Debe comunicar qué es la plataforma de subastas de cartas, generar confianza y convertir visitantes en registros. Es una vista pública (no requiere auth).

**Checklist:**
- [ ] Hero section: headline, subtítulo y CTA principal de registro (ver WEB-03)
- [ ] Sección "¿Cómo funciona?" — pasos de la subasta (3-4 pasos visuales)
- [ ] Sección de subastas destacadas en vivo (consumir endpoint de subastas activas)
- [ ] Sección de categorías de cartas disponibles
- [ ] Footer: links legales, redes sociales, contacto
- [ ] SEO básico: `<title>`, meta description, Open Graph tags
- [ ] Responsive completo (mobile, tablet, desktop) — requerido por rúbrica §4.1
- [ ] Estado vacío si no hay subastas activas (§3.4)

**Criterios de aceptación:**
- La landing carga en menos de 2s (imágenes optimizadas)
- Sin datos hardcodeados: subastas vienen del backend
- Visible sin login

---

### WEB-02 · Chatbot — Asistente de Subastas Yala
**Estado:** 🔴 Pendiente | **Prioridad:** 🔥 Alta | **Responsable:** —  
**Rúbrica:** §4.3 Vistas (+1 vista/componente diferenciado) · §3.1 Integración API · §4.2 UX

**Contexto:**  
Widget de chat flotante con IA entrenada en el dominio de Yala: reglas de subastas, tipos de cartas (Pokémon, Magic, Yu-Gi-Oh, etc.), cómo pujar, qué pasa si ganas/pierdes, etc.

**Checklist:**
- [ ] Widget flotante (bottom-right), togglable con botón
- [ ] Diseño del chat: burbujas de usuario vs. bot, timestamps, avatar del bot
- [ ] Integración con API de IA (definir: Claude API, OpenAI, o backend propio con RAG)
- [ ] Base de conocimiento del dominio Yala:
  - [ ] Cómo funciona una subasta en Yala
  - [ ] Tipos de cartas y cómo se valoran
  - [ ] Reglas de puja (outbid, tiempo extendido, precio de reserva)
  - [ ] Qué pasa cuando ganas (pago, envío, retiro presencial)
  - [ ] FAQs de soporte
- [ ] Historial de conversación en la sesión (no persistente entre sesiones)
- [ ] Indicador de "escribiendo..." mientras espera respuesta
- [ ] Fallback si la IA no puede responder: "Contacta a soporte en [email]"
- [ ] AbortController para cancelar requests si el usuario cierra el chat (§3.1)

**Criterios de aceptación:**
- El chatbot responde en menos de 5s
- No expone errores técnicos al usuario (§3.4)
- Funciona en mobile (responsive)

---

### WEB-03 · CTA "Crear Cuenta" — Alta Visibilidad
**Estado:** 🔴 Pendiente | **Prioridad:** 🔥 Alta | **Responsable:** —  
**Rúbrica:** §4.1 Diseño Visual · §4.2 Experiencia de Usuario

**Contexto:**  
El botón de registro es el KPI principal de conversión. Actualmente pasa desapercibido. Necesita rediseño para destacar en header y landing.

**Checklist:**
- [ ] Nuevo estilo del botón: gradiente/color de acento, tamaño mayor al actual
- [ ] Microanimación: efecto pulse suave o glow en hover para llamar atención
- [ ] Badge opcional: "¡Gratis!" o "Regístrate ahora"
- [ ] Contraste WCAG AA mínimo verificado (§4.2 accesibilidad)
- [ ] Ubicación estratégica:
  - [ ] Header fijo — siempre visible durante scroll
  - [ ] Hero de la landing (CTA principal)
  - [ ] Al final de sección "Cómo funciona"
- [ ] En mobile: botón full-width en hero, en header queda en hamburguesa (ver WEB-04)

**Criterios de aceptación:**
- En cualquier viewport el CTA es lo primero que llama la atención visual
- Click lleva al flujo de registro sin pasos innecesarios

---

### WEB-04 · Menú Hamburguesa
**Estado:** 🔴 Pendiente | **Prioridad:** 🔥 Alta | **Responsable:** —  
**Rúbrica:** §4.1 Responsive Design · §4.2 Navegación intuitiva

**Contexto:**  
El header actual muestra demasiados botones en pantallas medianas/pequeñas. Hay que reemplazarlo por un menú hamburguesa limpio.

**Checklist:**
- [ ] Auditar ítems del menú actual y definir jerarquía (qué queda fuera, qué va dentro)
- [ ] Breakpoint de activación: `< 1024px` (tablet y mobile)
- [ ] Implementar drawer lateral o dropdown animado
- [ ] Animación de apertura/cierre (≤ 300ms, `ease-in-out`)
- [ ] Cerrar al hacer click fuera del menú o en un link
- [ ] Accesibilidad:
  - [ ] `aria-expanded` en el botón hamburguesa
  - [ ] `aria-label="Abrir menú de navegación"`
  - [ ] Cierre con tecla `Escape`
  - [ ] Focus trap dentro del menú cuando está abierto
- [ ] CTA "Crear Cuenta" siempre visible en el header, fuera del hamburguesa
- [ ] En desktop (≥ 1024px): menú horizontal normal, hamburguesa oculto

**Criterios de aceptación:**
- El header en mobile se ve limpio: logo + hamburguesa + CTA
- Ningún botón se rompe o se superpone en ningún breakpoint

---

### WEB-05 · Validación de Identidad con DNI y Reconocimiento Facial
**Estado:** ⚪ Bloqueado — requiere decisión técnica | **Prioridad:** Media | **Responsable:** —  
**Rúbrica:** §3.1 Integración API · §3.4 Manejo de errores y estados · §4.3 Vistas

> ⚠️ **BLOQUEANTE:** Antes de desarrollar, el equipo debe responder:
> 1. ¿Tenemos acceso a la API de RENIEC o usamos un proveedor tercero (AWS Rekognition, Azure Face API)?
> 2. ¿El backend ya tiene un endpoint `/verify-identity` o lo hay que crear?
> 3. ¿Es un flujo en tiempo real o diferido (revisión posterior)?

**Contexto:**  
Verificación de identidad del postor antes de participar en una subasta. Combina OCR del DNI + comparación facial (foto DNI vs. selfie en tiempo real).

**Checklist — Investigación (hacer primero):**
- [ ] Confirmar proveedor de biometría y obtener credenciales de prueba
- [ ] Documentar el flujo con el backend (¿qué envía el front, qué devuelve el back?)
- [ ] Definir en qué momento del flujo se pide verificación (¿al registrarse? ¿al hacer primera puja?)

**Checklist — Implementación (post-decisión):**
- [ ] Vista/modal de verificación de identidad (nueva vista → suma para §4.3)
- [ ] **Paso 1 — Captura de DNI:**
  - [ ] Componente de upload de imagen o captura con cámara web
  - [ ] Preview de la imagen capturada con opción de reintentar
  - [ ] Guía visual de encuadre (overlay con forma del DNI)
- [ ] **Paso 2 — Captura facial:**
  - [ ] Acceso a cámara con `getUserMedia`
  - [ ] Guía de encuadre (óvalo centrado)
  - [ ] Captura de frame en el momento indicado
- [ ] **Paso 3 — Verificación:**
  - [ ] Envío seguro al backend (multipart o base64)
  - [ ] Estado de carga: "Verificando identidad..." con spinner (§3.4)
  - [ ] Resultado aprobado: badge verificado en perfil
  - [ ] Resultado rechazado: mensaje claro + opción de reintentar
- [ ] Manejo de errores:
  - [ ] Cámara no disponible o permisos denegados
  - [ ] Timeout de la API (AbortController — §3.1)
  - [ ] DNI no legible (instruir al usuario a mejorar iluminación)
- [ ] No almacenar imágenes en localStorage (datos sensibles)

**Criterios de aceptación:**
- El flujo completo funciona en Chrome y Firefox desktop
- Mensajes de error son entendibles por un usuario no técnico

---

### WEB-06 · Sensor de Sonido al Ganar Subasta
**Estado:** 🔴 Pendiente | **Prioridad:** Baja | **Responsable:** —  
**Rúbrica:** §4.2 Experiencia de Usuario (momento de deleite)

**Contexto:**  
Efecto de sonido celebratorio cuando el usuario gana una subasta. Se dispara desde el evento en tiempo real (WebSocket/SSE).

**Checklist:**
- [ ] Seleccionar efecto de sonido (fanfare corto, ≤ 3s, formato `.mp3` + `.ogg` para compatibilidad)
- [ ] Implementar reproducción:
  ```js
  // Respetar autoplay policy: solo funciona post-interacción del usuario
  const audio = new Audio('/sounds/win.mp3');
  audio.play().catch(() => { /* silenciar si el browser bloquea */ });
  ```
- [ ] Añadir opción de silenciar en configuración de usuario (guardar en `localStorage`)
- [ ] Acompañar con animación visual: confetti o banner "¡Ganaste! 🎉"
- [ ] Disparar el evento desde el listener de WebSocket/SSE de subasta ganada
- [ ] Asegurarse de que solo se reproduce una vez por victoria (flag de control)

> ⚠️ **Nota técnica:** Los navegadores bloquean autoplay de audio si el usuario no ha interactuado con la página. Verificar que el usuario haya hecho al menos un click antes de intentar reproducir.

**Criterios de aceptación:**
- El sonido se reproduce en Chrome y Safari tras interacción previa
- Si el browser bloquea el audio, no hay errores en consola ni se rompe la UI

---

### WEB-07 · IA para la Web — Funcionalidades Inteligentes
**Estado:** 🔴 Pendiente | **Prioridad:** Media | **Responsable:** —  
**Rúbrica:** §3.1 Integración API · §4.2 UX · §4.3 Vistas

**Contexto:**  
Más allá del chatbot (WEB-02), la IA puede potenciar otras partes de la plataforma. Definir qué features tienen más impacto para el MVP.

**Opciones a evaluar (elegir 1-2 para el MVP):**

| Feature | Complejidad | Impacto UX | Impacto Rúbrica |
|---|---|---|---|
| Recomendaciones de subastas personalizadas | Media | Alto | §4.2, §4.3 |
| Estimación de precio justo de una carta | Baja | Alto | §4.3 vista nueva |
| Detección de imagen de carta (catalogar automáticamente) | Alta | Medio | §3.1 |
| Resumen automático de descripción de subasta | Baja | Medio | §4.2 |

**Checklist (para la opción elegida):**
- [ ] Definir qué feature(s) de IA se implementarán
- [ ] Diseñar el componente/vista donde aparece (nueva vista suma para §4.3)
- [ ] Integrar con API de IA vía backend (no exponer API keys en frontend)
- [ ] AbortController para cancelar si el usuario navega (§3.1)
- [ ] Estado de carga con skeleton mientras la IA procesa (§3.4)
- [ ] Fallback si la IA no responde: mostrar datos sin recomendación

---

### WEB-08 · Tienda / Perfil de Vendedor
**Estado:** 🔴 Pendiente | **Prioridad:** Media | **Responsable:** —  
**Rúbrica:** §4.3 Vistas (+1 vista diferenciada) · §3.1 Integración API

**Contexto:**  
Página pública del vendedor/subastador dentro de Yala. Funciona como su "tienda" personal en la plataforma.

**Checklist:**
- [ ] **Sección de información básica:**
  - [ ] Foto de perfil, nombre/alias, descripción breve
  - [ ] Fecha de registro en Yala
  - [ ] Badge de identidad verificada (si completó WEB-05)
- [ ] **Redes sociales** (todos opcionales):
  - [ ] Instagram, Facebook, TikTok, Twitter/X
  - [ ] Links con ícono + handle, abren en nueva pestaña
- [ ] **Local presencial** (completamente opcional):
  - [ ] Campo de dirección (texto libre)
  - [ ] Mapa embebido de Google Maps (si se ingresó dirección)
  - [ ] Horario de atención
- [ ] **Estadísticas públicas:**
  - [ ] N° de subastas realizadas
  - [ ] Calificación promedio (estrellas)
  - [ ] Artículos vendidos exitosamente
- [ ] **Subastas del vendedor:**
  - [ ] Grid de subastas activas (con paginación — suma para §4.4)
  - [ ] Tab de historial de subastas finalizadas
- [ ] **Acciones:**
  - [ ] Botón "Seguir" (si el backend lo soporta)
  - [ ] Botón "Contactar" (abre chat o email)
- [ ] Manejo de perfil propio vs. perfil ajeno (ocultar botón "Seguir" en el propio)
- [ ] Estado vacío si el vendedor no tiene subastas aún

**Criterios de aceptación:**
- La vista es accesible desde `/tienda/:userId` o similar
- Redes sociales y local son opcionales: si no están configurados, no se muestran (no "Sin datos")

---

## 📱 FRONTEND MOBILE

---

### MOB-01 · Menú Hamburguesa
**Estado:** 🔴 Pendiente | **Prioridad:** 🔥 Alta | **Responsable:** —

**Contexto:**  
El header mobile actual muestra demasiados botones. Reemplazar con Drawer Navigation limpio.

**Checklist:**
- [ ] Usar `DrawerNavigator` de React Navigation o implementación custom
- [ ] Auditar y priorizar ítems del menú actual
- [ ] Gesto de swipe desde el borde izquierdo para abrir (estándar mobile)
- [ ] Animación nativa con `Reanimated` o `Animated` API
- [ ] Header resultante: solo logo + ícono hamburguesa + CTA registro (si es usuario no logueado)
- [ ] Accesibilidad: `accessibilityRole="button"`, `accessibilityLabel="Abrir menú"`
- [ ] Cerrar drawer al seleccionar una opción de navegación
- [ ] Indicador de ruta activa dentro del drawer

**Criterios de aceptación:**
- Header limpio en iOS y Android
- El drawer abre/cierra con animación fluida (≥ 60fps)
- Sin botones apilados o cortados en ningún tamaño de pantalla

---

### MOB-02 · Validación de Identidad con DNI y Reconocimiento Facial
**Estado:** ⚪ Bloqueado — misma decisión que WEB-05 | **Prioridad:** Media | **Responsable:** —

> ⚠️ **Bloqueante:** Resolver WEB-05 primero. La lógica de backend es compartida; solo cambia la implementación del cliente.

**Contexto:**  
Misma funcionalidad que en web pero aprovechando la cámara nativa del dispositivo, que da mejor calidad y experiencia para captura de DNI y selfie.

**Checklist:**
- [ ] Instalar y configurar `expo-camera` o `react-native-vision-camera`
- [ ] Solicitar permisos de cámara con flujo de onboarding si se deniegan
- [ ] **Pantalla 1 — Captura de DNI:**
  - [ ] Overlay con forma rectangular del DNI como guía
  - [ ] Flash automático si hay poca luz (`torch` mode)
  - [ ] Botón de captura y preview con opción de reintentar
- [ ] **Pantalla 2 — Selfie / Captura facial:**
  - [ ] Cámara frontal por defecto
  - [ ] Overlay oval centrado como guía
  - [ ] Liveness detection si el proveedor lo soporta (parpadeo o movimiento de cabeza)
- [ ] **Pantalla 3 — Procesando:**
  - [ ] Animación de carga mientras el backend valida
  - [ ] Timeout con mensaje de error si tarda más de 15s
- [ ] **Pantalla 4 — Resultado:**
  - [ ] Aprobado: animación de check + navegar al siguiente paso
  - [ ] Rechazado: mensaje específico (foto borrosa / datos no coinciden) + reintentar
- [ ] Manejo de errores:
  - [ ] Cámara ocupada por otra app
  - [ ] Permisos denegados permanentemente (redirigir a Settings del SO)
  - [ ] Sin conexión a internet

**Criterios de aceptación:**
- Funciona en iOS 15+ y Android 10+
- El flujo completo (DNI + selfie + resultado) toma menos de 60s en condiciones normales

---

### MOB-03 · Sensor de Sonido al Ganar Subasta
**Estado:** 🔴 Pendiente | **Prioridad:** Baja | **Responsable:** —

**Contexto:**  
Feedback auditivo y háptico cuando el usuario gana una subasta. Se dispara desde push notification o WebSocket.

**Checklist:**
- [ ] Instalar `expo-av` para reproducción de audio
- [ ] Audio: mismo archivo que en web para consistencia de marca
- [ ] Verificar modo silencio del dispositivo:
  - iOS: respetar silent switch (no reproducir si está en silencio)
  - Android: usar `AudioManager.STREAM_NOTIFICATION`
- [ ] Vibración háptica con `expo-haptics`:
  ```js
  await Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success);
  ```
- [ ] Animación celebratoria en pantalla (lottie de confetti o animación custom)
- [ ] Opción de desactivar sonido en configuración de la app
- [ ] Funciona cuando la app está en **foreground** (WebSocket activo)
- [ ] Para app en **background**: manejar desde push notification con sonido custom

**Criterios de aceptación:**
- En iOS con silent switch OFF: suena + vibra
- En iOS con silent switch ON: vibra, no suena (comportamiento esperado por usuarios)
- No hay crash si el archivo de audio no carga

---

## 📌 Decisiones Pendientes — Bloqueantes del Equipo

| ID | Decisión | Impacto | Responsable | Deadline |
|----|----------|---------|-------------|----------|
| D-01 | Proveedor de biometría para DNI (AWS Rekognition / Azure / FacePhi / otro) | WEB-05, MOB-02 | — | — |
| D-02 | ¿El backend tiene endpoint `/verify-identity`? ¿Qué recibe y qué devuelve? | WEB-05, MOB-02 | — | — |
| D-03 | Qué feature de IA se implementa en WEB-07 (ver tabla de opciones) | WEB-07 | — | — |
| D-04 | ¿El chatbot usa API externa (Claude/OpenAI) o backend propio con RAG? | WEB-02 | — | — |
| D-05 | Breakpoints de diseño: ¿el equipo usa Tailwind? ¿Hay Figma actualizado? | WEB-01 a 08 | — | — |

---

## 🎯 Alineación con Rúbrica DBP

> Mapa de features pendientes → puntos de evaluación

| Feature | §3.1 API | §3.2 Componentes | §3.3 Routing | §3.4 Errores | §4.1 UI | §4.2 UX | §4.3 Vistas | §4.4 Paginación |
|---------|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| WEB-01 Landing | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | +1 vista | — |
| WEB-02 Chatbot | ✅ | ✅ | — | ✅ | ✅ | ✅ | +1 comp | — |
| WEB-03 CTA | — | — | — | — | ✅ | ✅ | — | — |
| WEB-04 Hamburguesa | — | ✅ | ✅ | — | ✅ | ✅ | — | — |
| WEB-05 DNI/RENIEC | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | +1 vista | — |
| WEB-06 Sonido | — | ✅ | — | ✅ | — | ✅ | — | — |
| WEB-07 IA Web | ✅ | ✅ | — | ✅ | — | ✅ | +1 vista | — |
| WEB-08 Tienda | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | +1 vista | ✅ |

**Vistas diferenciadas que suman con estas features:** Landing · Verificación DNI · IA Feature · Tienda Vendedor → **≥ 4 vistas nuevas** sobre las ya existentes ✅

---

## 🔗 Referencias del Proyecto

- [ ] Repositorio GitHub: `_link pendiente_`
- [ ] Figma / Diseño: `_link pendiente_`
- [ ] Backend base URL (producción): `_link pendiente_`
- [ ] Backend base URL (staging): `_link pendiente_`
- [ ] Notion/Linear del proyecto: `_link pendiente_`
