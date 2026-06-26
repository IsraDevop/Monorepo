## 0. Repositorios, estructura y entornos

El monorepo de trabajo contiene tres carpetas, cada una enlazada a su propio repositorio Git:

| Carpeta        | Rol            | Repositorio |Project|
|----------------|----------------|-------------|
| `frontend_yala` (o `frontend-web`) | Frontend web (React + Vite + TS) | https://github.com/Sebas-Leo/frontend_yala.git |https://github.com/users/Sebas-Leo/projects/1/views/1|
| `backend`      | Backend (Spring Boot) | https://github.com/katealva/backend-yala |https://github.com/users/katealva/projects/5/views/1|
| `frontend-mobile` | Frontend mobile (React Native + Expo) |https://github.com/IsraDevop/Frontend-Mobile2.git |https://github.com/users/IsraDevop/projects/4/views/1|

**Entornos y despliegue:**
- **Backend desplegado en:** `https://yala.dpdns.org/api/v1/` — esta es la `baseURL` de producción
  para web y mobile. Nunca hardcodear; va en variables de entorno (ver §2 y §11).
- **Frontend web desplegado en AWS Amplify.**
- **Frontend mobile** publicado vía Expo (EAS / Expo Go para demo).
- **Workflow de CI/CD:** debe existir un workflow de GitHub Actions que **se dispare cuando haya
  cambios en `main`** y **actualice el frontend web en AWS Amplify** automáticamente.

> Cada repositorio (web, backend y mobile) tiene su **propio GitHub Project** donde se debe
> visualizar el flujo de trabajo (issues → en progreso → en revisión → hecho).

Lee el archivo @Buenas Practicas.md
