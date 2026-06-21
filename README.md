# 🎸 Ecos del Ayer Store

Tienda online retro 80s. Un solo archivo (`index.html`), sin instalación de programas.

Esta guía te lleva paso a paso desde "probarla en mi compu" hasta "publicada en mi dominio, con stock y música sincronizados para todos".

---

## 🗺️ Orden recomendado

1. [Probarla en tu compu (localhost)](#1--probarla-en-tu-compu-localhost)
2. [Conectar Firebase (base de datos gratis)](#2--conectar-firebase-base-de-datos-gratis)
3. [Publicar en internet con tu dominio (Netlify)](#3--publicar-en-internet-con-tu-dominio-netlify)
4. [Crear el usuario admin (quién puede entrar al panel)](#4--crear-el-usuario-admin)
5. [Configurar WhatsApp para recibir pedidos](#5--configurar-whatsapp)

---

## 1. ▶ Probarla en tu compu (localhost)

1. Abrí VS Code
2. Instalá la extensión **Live Server** (la de Ritwick Dey, está en el panel de extensiones)
3. `Archivo → Abrir Carpeta` → seleccioná la carpeta `ecos-del-ayer`
4. Clic derecho sobre `index.html` → **"Open with Live Server"**
5. Se abre en `http://127.0.0.1:5500`

Sin Firebase conectado todavía, funciona en **modo demo local** (los datos se guardan solo en tu navegador, contraseña demo: `admin1234`). Para que la otra persona vea lo mismo que vos, necesitás el paso 2.

---

## 2. 🔥 Conectar Firebase (base de datos gratis)

Esto hace que el stock, los productos y la música se sincronicen **en tiempo real** entre vos, tu socio/empleado y todos los visitantes de la tienda, sin importar desde qué dispositivo entren.

### 2.1 Crear el proyecto

1. Andá a [console.firebase.google.com](https://console.firebase.google.com)
2. Iniciá sesión con una cuenta de Google
3. **"Crear un proyecto"** → ponele un nombre (ej: `ecos-del-ayer`) → seguí los pasos (podés desactivar Google Analytics, no hace falta)

### 2.2 Crear la base de datos (Firestore)

1. En el menú izquierdo: **Compilación → Firestore Database**
2. **"Crear base de datos"**
3. Elegí **"Modo de producción"**
4. Elegí la ubicación más cercana (ej: `southamerica-east1` para Argentina/Brasil)

### 2.3 Configurar las reglas de seguridad

Dentro de Firestore Database → pestaña **"Reglas"**, reemplazá todo el contenido por esto y hacé clic en **"Publicar"**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /products/{productId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /tracks/{trackId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    match /settings/{docId} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

Esto dice: "cualquiera puede ver la tienda, pero solo alguien con sesión iniciada puede modificarla".

### 2.4 Activar el login (Authentication)

1. Menú izquierdo: **Compilación → Authentication**
2. **"Comenzar"**
3. En la lista de proveedores, elegí **"Correo electrónico/contraseña"** → activalo → Guardar

(Más abajo, en el paso 4, creás el usuario admin real.)

### 2.5 Obtener las claves de configuración

1. Hacé clic en el ⚙️ (arriba a la izquierda, junto a "Vista general del proyecto") → **"Configuración del proyecto"**
2. Bajá hasta **"Tus apps"** → clic en el ícono `</>`  (Web)
3. Ponele un apodo (ej: `tienda-web`) → **"Registrar app"**
4. Te va a mostrar un bloque de código `firebaseConfig` con varias claves. Copialo.

### 2.6 Pegar las claves en el código

1. Abrí `index.html` en VS Code
2. Buscá esta sección cerca del final del archivo (`Ctrl+F` → buscá `PEGA_TU_API_KEY_ACA`):

```js
const firebaseConfig = {
  apiKey: "PEGA_TU_API_KEY_ACA",
  authDomain: "PEGA_TU_AUTH_DOMAIN_ACA",
  projectId: "PEGA_TU_PROJECT_ID_ACA",
  storageBucket: "PEGA_TU_STORAGE_BUCKET_ACA",
  messagingSenderId: "PEGA_TU_SENDER_ID_ACA",
  appId: "PEGA_TU_APP_ID_ACA"
};
```

3. Reemplazá cada valor `"PEGA_TU_..._ACA"` por el dato real que copiaste de Firebase
4. Guardá el archivo

Listo — a partir de acá, la tienda ya usa Firebase. Si volvés a abrirla con Live Server, vas a ver que **ya no dice "modo demo"** en la consola del navegador.

---

## 3. 🌐 Publicar en internet con tu dominio (Netlify)

Como solo tenés el dominio (sin hosting), la forma más simple y gratuita es **Netlify**.

### 3.1 Subir el sitio a Netlify

1. Andá a [netlify.com](https://netlify.com) → creá una cuenta gratis
2. En el panel, buscá la zona que dice **"Add new site" → "Deploy manually"**
3. Arrastrá tu carpeta `ecos-del-ayer` (o el archivo `index.html`) directo a esa zona
4. En segundos te da una URL tipo `https://nombre-random.netlify.app` — ya está online

### 3.2 Conectar tu dominio propio

1. Dentro del sitio en Netlify → **"Domain settings"** → **"Add a domain"**
2. Escribí tu dominio (ej: `ecosdelayer.com`) → seguí los pasos
3. Netlify te va a dar unos registros DNS (tipo `A` o `CNAME`) para configurar
4. Entrás al panel donde compraste tu dominio (GoDaddy, Namecheap, etc.) → sección DNS → cargás esos registros que te dio Netlify
5. Esperá unas horas (a veces hasta 24-48hs) a que se propague el DNS

Cuando termine, tu tienda va a estar en `https://tudominio.com` ✅

---

## 4. 🔑 Crear el usuario admin

Una vez conectado Firebase (paso 2):

1. En Firebase Console → **Authentication** → pestaña **"Users"**
2. **"Add user"**
3. Cargá el email y contraseña de la persona que va a administrar la tienda (puede ser vos, tu socio, o ambos con emails distintos)
4. Guardá

### Cómo entra esa persona al panel

1. Va a `https://tudominio.com/#enki605`
2. Ingresa con el **email y contraseña** que cargaste en Firebase
3. Ya puede gestionar productos, stock y música — los cambios se ven al instante para todos

### Agregar más administradores

Repetís el paso "Add user" en Firebase Authentication con otro email. No hay límite.

### Si alguien olvida su contraseña

Dentro del panel admin → pestaña **Configuración** → botón **"Enviarme un link para cambiar mi contraseña"** (le llega un email).

---

## 5. 📲 Configurar WhatsApp

Para que los pedidos y consultas te lleguen:

1. Entrá al panel (`#enki605`) → pestaña **Configuración**
2. En **"WhatsApp — Recepción de Pedidos"**, cargá tu número con código de país, sin espacios ni el `+`
   Ejemplo Argentina: `5491122334455`
3. Guardá

Desde ese momento, cada compra y cada consulta del formulario de contacto abren WhatsApp automáticamente con el mensaje armado.

---

## 🎵 Subir música con Google Drive

Con Firebase conectado, **subir un archivo MP3 directo desde la compu ya no está disponible** (son demasiado pesados para la base de datos). En su lugar, se usa Google Drive — la tienda convierte el link automáticamente, no hace falta ninguna herramienta externa.

### Pasos

1. Subí el MP3 a tu Google Drive (arrastralo a [drive.google.com](https://drive.google.com))
2. Clic derecho sobre el archivo → **Compartir**
3. Cambiá el acceso de "Restringido" a **"Cualquier usuario con el enlace"** (importante — si no, nadie va a poder escucharlo)
4. Hacé clic en **"Copiar enlace"**
5. Pegá ese link tal cual (sin modificarlo) en **Admin → Música → "Pegar link de Google Drive"**
6. La tienda detecta que es un link de Drive y lo convierte sola al formato reproducible — vas a ver un aviso "🔄 Link de Google Drive detectado y convertido automáticamente"

### Otras opciones (si preferís no usar Drive)

| Servicio | Cómo conseguir el link directo |
|---|---|
| **Archive.org** | Subís el MP3 en [archive.org/upload](https://archive.org/upload) → copiás el link `.mp3` que te da |
| **Dropbox** | Copiás el link → cambiás `?dl=0` por `?dl=1` al final |

---

## 📧 Activar envío de emails (opcional, EmailJS)

Además de WhatsApp, podés activar el envío por correo:

1. Registrate gratis en [emailjs.com](https://www.emailjs.com)
2. Creá un servicio de email y un template
3. Cargá las claves en Admin → Configuración

---

## 📁 Estructura del proyecto

```
ecos-del-ayer/
├── index.html      ← toda la app (HTML + CSS + JS en un solo archivo)
└── README.md       ← este archivo
```

---

## ❓ Preguntas frecuentes

**¿Cuánto cuesta Firebase?**
Nada, mientras no superes el plan gratuito (Spark), que es muy generoso para una tienda chica/mediana: 50.000 lecturas y 20.000 escrituras por día, gratis.

**¿Qué pasa si no conecto Firebase?**
La tienda sigue funcionando, pero en "modo demo": cada navegador ve sus propios datos, no sirve para que varias personas administren o para que los clientes vean el stock real.

**¿Puedo seguir probando en localhost después de conectar Firebase?**
Sí, sin problema. Localhost y el sitio publicado van a compartir la misma base de datos de Firebase.

---

*Ecos del Ayer Store © 2025*
