# 🎸 Ecos del Ayer Store

Tienda online retro 80s. Archivo único, sin dependencias ni instalación.

---

## ▶ Cómo abrir en VS Code (localhost)

### Opción 1 — Live Server (recomendada, 1 clic)

1. Abrí VS Code
2. Instalá la extensión **Live Server** (buscarla en el panel de extensiones, es de Ritwick Dey)
3. Abrí esta carpeta en VS Code: `Archivo → Abrir Carpeta → ecos-del-ayer`
4. Hacé clic derecho sobre `index.html` → **"Open with Live Server"**
5. Se abre automáticamente en `http://127.0.0.1:5500`

### Opción 2 — Sin extensiones (Python)

Si tenés Python instalado, abrí una terminal en esta carpeta y ejecutá:

```bash
# Python 3
python -m http.server 3000
```

Luego abrí `http://localhost:3000` en el navegador.

### Opción 3 — Sin extensiones (Node.js)

```bash
npx serve .
```

Luego abrí la URL que aparece en la terminal.

---

## 🔑 Acceso al Panel Admin

- Botón **⚙ Admin** en la barra de navegación
- Contraseña por defecto: **`admin1234`**
- Cambiarla desde Admin → ⚙ Configuración

---

## 📁 Estructura del proyecto

```
ecos-del-ayer/
├── index.html      ← toda la app (HTML + CSS + JS en un solo archivo)
└── README.md       ← este archivo
```

---

## 💾 Dónde se guardan los datos

Todo se guarda en el **LocalStorage** del navegador automáticamente.
No se pierde al cerrar — persiste mientras no borres los datos del navegador.

---

## 📧 Activar envío de emails (EmailJS)

1. Registrate gratis en [emailjs.com](https://www.emailjs.com)
2. Creá un servicio de email y un template
3. Copiá las claves en Admin → ⚙ Configuración

---

*Ecos del Ayer Store © 2025*
