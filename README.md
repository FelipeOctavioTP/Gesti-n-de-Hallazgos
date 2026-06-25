# Gestión de Hallazgos · DCSM

Dashboard ejecutivo de hallazgos para Data Center San Martín. Es un único
archivo HTML, sin backend propio: lee el Excel en el navegador y guarda la
"versión vigente" en Supabase para que tu cliente siempre vea la última
que subiste, sin que tú tengas que reenviar nada.

---

## 1. Crear el repositorio en GitHub (nuevo, separado del de Mantenimiento)

1. Entra a [github.com/new](https://github.com/new).
2. Nombre del repositorio, por ejemplo: `dashboard-hallazgos-dcsm`.
3. Público (necesario para que GitHub Pages lo sirva gratis) o privado si tienes plan que lo permita.
4. Click **Create repository** (sin agregar README desde ahí, ya tienes este).

## 2. Subir los archivos

Necesitas que el archivo principal se llame **`index.html`** en la raíz del
repo, para que GitHub Pages lo muestre directo en la URL del repo.

En tu computador:
1. Descarga `Gestión_de_Hallazgos_DCSM.html` (el dashboard) y renómbralo a `index.html`.
2. En GitHub, abre tu repo nuevo → **Add file → Upload files**.
3. Arrastra `index.html` (y este `README.md` si quieres).
4. Click **Commit changes**.

## 3. Activar GitHub Pages

1. En el repo: **Settings → Pages**.
2. En "Build and deployment" → Source: **Deploy from a branch**.
3. Branch: **main**, carpeta: **/ (root)**. Guardar.
4. Espera 1-2 minutos. Tu dashboard quedará en:
   `https://TU-USUARIO.github.io/dashboard-hallazgos-dcsm/`
5. Ese es el link que le compartes a tu cliente.

---

## 4. Conectar Supabase (para que "Actualizar Excel" se vea reflejado para todos)

Usas el **mismo proyecto** de Supabase que ya tienes para tu dashboard de
Mantenimiento, solo con una tabla nueva.

### 4.1 Crear la tabla
1. Entra a [supabase.com](https://supabase.com) → tu proyecto.
2. Ve a **SQL Editor → New query**.
3. Pega el contenido completo del archivo `schema.sql` (incluido junto a este README) y ejecuta (▶ Run).
4. Deberías ver la tabla `hallazgos_dcsm` en **Table Editor**.

### 4.2 Obtener tus credenciales
1. En Supabase: **Settings → API**.
2. Copia:
   - **Project URL** (algo como `https://xxxxx.supabase.co`)
   - **anon public key** (una clave larga que empieza con `eyJ...`)

### 4.3 Pegarlas en el dashboard
1. Abre `index.html` con un editor de texto.
2. Busca esta sección cerca del inicio del segundo `<script>`:
   ```js
   const SUPABASE_URL = "PEGA_AQUI_TU_PROJECT_URL";
   const SUPABASE_ANON_KEY = "PEGA_AQUI_TU_ANON_KEY";
   ```
3. Reemplaza por tus valores reales, por ejemplo:
   ```js
   const SUPABASE_URL = "https://xxxxx.supabase.co";
   const SUPABASE_ANON_KEY = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...";
   ```
4. Guarda el archivo y vuelve a subirlo a GitHub (Upload files → reemplazar `index.html` → Commit).

> La "anon key" está pensada para vivir en el frontend — no es secreta en
> el mismo sentido que una contraseña, pero junto con las políticas del
> `schema.sql` queda **completamente abierta**: cualquiera con el link de
> tu dashboard puede subir un Excel y reemplazar la versión vigente. Fue
> tu decisión no pedir PIN; si cambias de opinión, dímelo y lo protegemos.

---

## 5. Cómo se usa día a día

1. Abres tu link de GitHub Pages.
2. Click **Actualizar Excel** → seleccionas el `.xlsx` más reciente.
3. Te pide confirmación ("¿reemplazar la versión vigente?") → confirmas.
4. El dashboard guarda esos datos en Supabase.
5. **Cualquiera que abra el mismo link después** (tú o tu cliente) verá
   automáticamente esa versión — no necesitan recargar archivo, ni tú
   reenviar nada.

Si nadie ha subido nunca un Excel a Supabase (primera vez), el dashboard
muestra el snapshot de datos que quedó embebido al momento de crearlo,
para que nunca se vea vacío.

---

## Archivos de este paquete

- `index.html` — el dashboard (este es el que subes a GitHub).
- `schema.sql` — créalo una sola vez en Supabase.
- `README.md` — esta guía.
