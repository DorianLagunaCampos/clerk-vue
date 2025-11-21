# 🔑 Clerk Token Generator (Local Helper)

Esta es una pequeña utilidad construida con **Vue.js** y **Clerk**. Su único propósito es autenticarte y generar un **JWT (Token)** válido que incluya la sesión y la organización activa, para usarlo en pruebas de Backend (Postman, Insomnia, cURL).

## ¿Por qué usar esto?
Probar endpoints protegidos en local es complicado porque los tokens de Clerk rotan y requieren una firma específica. Esta app:
1. Te permite iniciar sesión.
2. **Auto-activa** tu primera organización (necesario para que el token tenga permisos de org).
3. Copia el token al portapapeles con un clic.

## 📋 Requisitos

- Tener el `Publishable Key` de tu proyecto de Clerk.
- Pertenecer a al menos una Organización en Clerk.

## 🚀 Instalación Rápida

1. **Instalar dependencias:**
   ```bash
   npm install
   ```

2. Configurar entorno: Crea un archivo .env en la raíz y agrega tu clave pública de Clerk:
    ```bash
    VITE_CLERK_PUBLISHABLE_KEY=pk_test_tu_clave_publica_aqui
    ```

3. Ejecutar:
    ```bash
    npm run dev
    ```
## Uso
1. Abre la URL que te indica la terminal (usualmente http://localhost:5173).

2. Haz clic en Sign In e inicia sesión con tus credenciales de prueba.

3. Espera un segundo: El sistema detectará automáticamente tu organización y la activará (verás un log en consola: ⚠️ No hay Org activa. Auto-activando...).

4. Haz clic en el botón "Copiar token para Postman".

5. Pega el token en el Header de tu petición: Authorization: Bearer <TU_TOKEN_COPIADO>