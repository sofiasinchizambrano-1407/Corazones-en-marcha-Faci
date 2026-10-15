# Inicio de sesión Google — Corazones en Marcha

## Qué incluye
- Botón **Acceso miembros** en las cuatro páginas públicas.
- Acceso con cuenta Google real (Firebase Authentication); sin contraseñas propias.
- Validación de correo verificado y lista autorizada en Firestore.
- Panel de miembros y panel del administrador para añadir y revocar los seis correos.
- Cuenta administradora independiente de los seis integrantes.

## Activarlo (obligatorio)
1. Crea un proyecto en https://console.firebase.google.com/.
2. En Authentication > Sign-in method habilita **Google** (elige correo de asistencia).
3. En Authentication > Settings > Authorized domains añade el dominio donde publicarás el sitio (y localhost para desarrollo, si aplica).
4. Registra una app web en Configuración del proyecto > Tus apps y copia sus datos a `firebase-config.js`.
5. Crea una base Cloud Firestore en modo producción. En Firestore > Rules reemplaza las reglas por el contenido de `firestore.rules` y publica.
6. En Firestore > Data crea colección `authorizedEmails`. Crea un documento cuya **ID es el correo exacto del administrador en minúsculas**, por ejemplo `administrador@gmail.com` y dos campos: `role` (string) = `admin`, `active` (boolean) = `true`. Hazlo desde la consola Firebase, NO desde el navegador del sitio.
7. Sirve el proyecto con un servidor web, no abriendo el HTML como `file://`. Por ejemplo, en la carpeta: `python -m http.server 8000` y abre http://localhost:8000/acceso.html. Alternativamente publícalo con Firebase Hosting.
8. Ingresa como administrador y agrega hasta seis direcciones de los miembros oficiales. Cada miembro elige su propia cuenta al usar Google.

**Importante:** La autenticación NO estará operativa hasta que completes los pasos anteriores. No se incluyen claves reales porque aún no se han proporcionado datos del proyecto Firebase ni los correos de los integrantes.

## Control de acceso y limitaciones
- La autorización se hace por **correo verificado**, comprobado en las reglas de Firestore, no solo por JavaScript del navegador.
- El frontend limita la lista a seis cuentas activas. Si necesitas hacer cumplir el máximo también ante escrituras simultáneas maliciosas, implementa una función backend transaccional o reglas adicionales: el frontend por sí solo no es una garantía de cupo.
- El administrador puede añadir y retirar miembros pero no puede crear nuevos administradores desde la página.
- El acceso se revoca al quitar el correo del directorio; los tokens de Firebase pueden permanecer activos, pero los datos protegidos por las reglas dejarán de estar accesibles. Este sitio no contiene aún contenido privado en Firestore aparte del directorio.
- Un miembro no puede ver el directorio completo. Todas las otras colecciones Firestore están bloqueadas por defecto.
- Mantén tu correo administrador seguro y utiliza verificación en dos pasos de Google.
