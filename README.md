# 🐕 WOL-AGENTE-Alexa-Completo

**La forma definitiva, gratuita y segura de encender, apagar o poner en modo de suspensión tus ordenadores mediante Alexa.** 🚀🖥️

¿Cansado de las funciones de pago de Alexa o de las configuraciones complejas? Este proyecto te permite crear tu propia **función privada para el hogar inteligente** para gestionar tus ordenadores con tu Amazon Echo. No se requiere ningún hardware físico: solo la nube y un agente ligero para Windows.

---

### 🔥 Características principales:
- **Control total de energía**: Encienda (Wake-on-LAN) y apague/suspenda/hiberne tu PC.
- **Compatibilidad con múltiples dispositivos**: Administre tantos ordenadores como desee (por ejemplo, "Alexa, enciende el PC para juegos", "Alexa, desactiva Office").
- **Agente de Windows (listo para usar)**: ejecutable precompilado que reside en la bandeja del sistema.
- **Puente SHA-256 seguro**: Comunicación cifrada entre Alexa y su PC mediante su hash privado.
- **Panel de control moderno**: Interfaz elegante de *Glassmorphism* para administrar tus dispositivos.
- **100% gratuito**: Funciona completamente dentro de los niveles gratuitos de Vercel, Upstash (Redis) y AWS.

---

### 🚀 Guía de configuración paso a paso:

#### 1. Configuración de la base de datos (Upstash Redis)
Regístrate en [Upstash](https://upstash.com).
- Crea una nueva base de datos **Redis**.
- Desplázate hacia abajo hasta la sección "API REST" y copia `UPSTASH_REDIS_REST_URL` y `UPSTASH_REDIS_REST_TOKEN`. Los necesitarás para Vercel.

#### 2. Implementación en la nube (Vercel)
- Clona este repositorio en tu cuenta de GitHub.
Crea un nuevo proyecto en [Vercel](https://vercel.com) y conecta tu repositorio GITHUB copiado.
- Luego Abajo veras > Environment Variables** y añade lo siguiente (clave:valor):
- `UPSTASH_REDIS_REST_URL`: (del Paso 1).
- `UPSTASH_REDIS_REST_TOKEN`: (del Paso 1).
- `ADMIN_PASSWORD`: Elige una contraseña secreta. La usarás para acceder al panel de control y como "Clave de Seguridad" para el Agente de Windows.
- Despliega el proyecto. Copia tu URL de despliegue (por ejemplo, `https://your-app.vercel.app`).

#### 3. Integración de Alexa y AWS Lambda
Primero AWS Lambda: (Inicia secion con tu correo de amazon).
- Alexa necesita un "puente" para comunicarse con Vercel.
- **AWS Lambda**: Crea una nueva función (Entorno de ejecución: Node.js 18+ ... En mi caso fue NODE.JS 24.x)
-Revisa que el Index termine en .JS y no .MJS
Copia el código de `/bridge/lambda_bridge.js` en este repositorio y pégalo en el editor de Lambda.
- **IMPORTANTE**: Cambia la variable `vercelUrl` en el código por tu URL de Vercel: `https://your-app.vercel.app/api/alexa`.
Amazon Developer SKILL: (Inicia secion con el mismo correo de amazon)
- Agrega un activador de **Alexa Smart Home** a tu Lambda y copia el **ARN** de Lambda (esquina superior derecha).
- **Consola para desarrolladores de Alexa**: Crea una nueva skill de **Hogar inteligente**.
- En **Punto final del servicio de Hogar inteligente**, pega el **ARN** de tu Lambda.
- En **Vinculación de cuentas**, proporciona estos puntos finales de Vercel:
- URI de autorización: `https://tu-aplicación.vercel.app/api/auth`
- URI del token de acceso: `https://tu-aplicación.vercel.app/api/token`
- ID de cliente: `cualquier cosa` (no está marcado en esta configuración privada).
- Secreto de cliente: `cualquiera cosa`.
- **Extras propios**:
- Scope*: `profile`
- Domain List: `tu-aplicación.vercel.app`
- Default Access Token Expiration Time: `3600`

#### 4. Agente de Windows (Apagado/Suspensión)
- Dirígete a la sección **Versiones** de este repositorio de GitHub.
- Descarga el archivo `agent.exe`.
- Ejecútalo en el PC que deseas controlar.
- **Configuración**:
- Introduce la **Dirección MAC** del PC (debe coincidir con la que introduces en el panel web).
- Introduce la **Clave de seguridad** (la `ADMIN_PASSWORD` que configuraste en Vercel).
- Haz clic en **Conectar y guardar**. (Deberia desaparecer el offline rojo y ponerse en verde `Secure Listening Active`)
- El agente se minimizará a la **Bandeja del sistema**. Haz clic con el botón derecho en el icono X de salir.
---

#### 5. Activacion de SKILL en APP Alexa
- Abres la aplicacion de Alexa > te diriges abajo a las 3 rayas
- Preciona Skills y juegos > Luego baja hasta `Mis Skills`
- Te saldran las Activas, Blueprint y Desarrolador
- Ve a desarrolador y encontraras tu Skill
- Abrela y activala (deberia Vincularce exitosamente y luego buscar el dispositivo)

### 🗣️ Uso
1. Abre tu panel de control de Vercel, inicia sesión con tu contraseña y añade el nombre y la dirección MAC de tu PC.
2. Dile a Alexa: «Alexa, descubre mis dispositivos».
3. Una vez encontrados:
- «Alexa, enciende [Nombre del dispositivo]»
- «Alexa, apaga [Nombre del dispositivo]» (Acción: Suspender/Apagar, según la opción seleccionada en el Agente).
---


### 🛡️ Seguridad y privacidad
La comunicación entre Vercel y tu PC se realiza a través de [ntfy.sh](https://ntfy.sh) mediante un identificador de tema único e imposible de adivinar. Este identificador se genera utilizando un **hash SHA-256** de tu dirección MAC y tu clave privada. Nadie puede acceder a tu PC sin conocer tu contraseña secreta.

---

### 📜 Licencia
Licencia MIT. Desarrollado con ❤️ por **FlowersPowerz**.

*Si te gusta este proyecto, ¡dale una ⭐!*
