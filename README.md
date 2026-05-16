# Control de Gastos 💰

PWA de control de gastos en pareja, sincronizada en tiempo real con Firebase. Funciona offline, es instalable en móvil y escritorio, e incluye escáner de tickets con IA (Claude).

---

## Características

- Registro de gastos e ingresos por persona y categoría
- Sincronización en tiempo real vía Firebase Firestore
- Escáner de tickets con IA (Anthropic Claude)
- Presupuestos mensuales con alertas
- Exportación / importación CSV
- Modo oscuro / claro y temas de color
- Funciona offline (Service Worker + precaché)

---

## Estructura de archivos

```
ControlGastos/
├── index.html        # Estructura HTML de la app
├── styles.css        # Todos los estilos CSS
├── app.js            # Lógica JavaScript de la aplicación
├── sw.js             # Service Worker (caché offline)
├── manifest.json     # Manifiesto PWA
├── icon-192.png      # Icono 192×192
├── icon-512.png      # Icono 512×512
├── .env.example      # Variables de entorno de ejemplo
├── .gitignore
└── SECURITY.md       # Notas de seguridad
```

---

## Configurar Firebase

1. Ve a [Firebase Console](https://console.firebase.google.com) y crea un proyecto.
2. Activa **Firestore Database** (modo producción o prueba).
3. Activa **Authentication** → habilita acceso anónimo y/o Email/contraseña.
4. En Configuración del proyecto → Tus apps → Web, copia el objeto `firebaseConfig`.
5. Pégalo en `app.js` (busca `const firebaseConfig`).

> **Producción**: crea `config.local.js` con el config y cárgalo antes de `app.js`.
> Nunca subas claves reales a un repo público.

---

## Configurar la API key de Anthropic (escáner IA)

1. Obtén una key en [console.anthropic.com](https://console.anthropic.com).
2. En la app, ve a **Config → API Key de Claude** e introdúcela.
3. La key se guarda **solo en `localStorage` de ese dispositivo**.

⚠️ Lee `SECURITY.md` — la key está expuesta en el cliente. Para producción usa un proxy server-side (Firebase Cloud Function o Cloudflare Worker).

---

## Despliegue en GitHub Pages

```bash
# Asegúrate de que todo está en la rama main
git push origin main
```

Ve a **Settings → Pages → Source: Deploy from branch → main / (root)**.

La app quedará en `https://<tu-usuario>.github.io/ControlGastos/`.

> El `manifest.json` usa `"start_url": "./"` para funcionar correctamente en subdirectorios de GitHub Pages.

---

## Despliegue en Netlify

1. Conecta el repositorio en [app.netlify.com](https://app.netlify.com).
2. Build command: *(vacío — no hay build)*
3. Publish directory: `.` (raíz del repo)
4. Deploy.

---

## Despliegue en Firebase Hosting

```bash
npm install -g firebase-tools
firebase login
firebase init hosting   # public dir: .  /  single-page app: no
firebase deploy
```

---

## Desarrollo local

Sirve los archivos con cualquier servidor HTTP estático (necesario para el Service Worker):

```bash
npx serve .
# o
python3 -m http.server 8080
```

Abre `http://localhost:8080/ControlGastos/` (o la ruta que uses).

Activa los logs de debug en la consola del navegador:

```js
localStorage.setItem('debug', '1');
location.reload();
```
