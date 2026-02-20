

# 🛒 Laboratorio: Excessive trust in client-side controls

**Plataforma:** PortSwigger Web Security Academy  
**Herramienta de Interceptación:** Caido  (Link de instalacion: https://docs-caido-io.translate.goog/app/quickstart/linux?_x_tr_sl=en&_x_tr_tl=es&_x_tr_hl=es&_x_tr_pto=tc)

**Objetivo:** Explotar una vulnerabilidad de confianza excesiva en los controles del lado del cliente para comprar la chaqueta "Lightweight 'l33t' Leather Jacket" aprovechando un error en la lógica de negocio.

---

## 📝 Descripción de la Vulnerabilidad
La aplicación web procesa las compras enviando el precio del producto desde el lado del cliente (navegador) al servidor a través de una petición POST. Dado que el servidor no verifica el precio real del producto en su base de datos, confía ciegamente en el valor proporcionado en la solicitud, permitiendo la manipulación del costo final.

## 🚀 Pasos para la Explotación

### 1. Autenticación
Primero, iniciamos sesión en la aplicación utilizando las credenciales proporcionadas para el laboratorio (`wiener:peter`).

> <img width="1851" height="683" alt="Login" src="https://github.com/user-attachments/assets/23dc0e9f-46d2-48bb-86b6-1788353b9a72" />


### 2. Exploración del Catálogo
Navegamos por la página principal de la tienda en busca de nuestro producto objetivo: la **"Lightweight 'l33t' Leather Jacket"**.

> <img width="1917" height="958" alt="Pagina_Principal" src="https://github.com/user-attachments/assets/cd9926b4-db9d-46da-9559-6a9db9aa3027" />

Al ver los detalles, confirmamos que el precio original de la chaqueta es de **$1337.00**, lo cual supera nuestro crédito disponible en la tienda.

> <img width="1812" height="729" alt="Precio" src="https://github.com/user-attachments/assets/7589f9fc-6327-4b12-8274-069ef63d1eae" />


### 3. Interceptación de la Petición (El inicio del ataque)
Activamos la interceptación en **Caido** (modo *Queuing*) y procedemos a agregar la chaqueta al carrito. Al capturar la petición `POST` a la ruta `/cart`, observamos que el cuerpo de la solicitud incluye el parámetro `price=133700` (el precio está representado en centavos).

> <img width="1908" height="1034" alt="Precio_Inicial" src="https://github.com/user-attachments/assets/c20f219b-89f3-47ca-82e1-bc1005eaf8e5" />


### 4. Modificación del Parámetro (Payload)
Para explotar la vulnerabilidad, alteramos directamente el valor del parámetro `price` dentro de Caido. Cambiamos el valor de `133700` a `1`, lo que equivale a un costo de **$0.01**. Luego, reenviamos (Forward) la petición al servidor.

> 

El servidor responde con un código `302 Found`, indicando que la petición fue procesada y nos redirige de vuelta a la página.

> <img width="1915" height="1042" alt="Respuesta_Precio_Modificado" src="https://github.com/user-attachments/assets/4db9a7a9-24ea-4fa5-9a9f-2c121b6893c5" />


### 5. Verificación de la Explotación
Desactivamos el proxy y nos dirigimos a nuestro carrito de compras. Podemos observar que el sistema confió en el dato manipulado y ahora la chaqueta se encuentra en nuestro carrito con un precio total de **$0.01**.

*Nota: Así se vería un carrito normal si no se altera el precio:*

><img width="806" height="535" alt="Carrito_Antes_De_Modificacion_De_Precio" src="https://github.com/user-attachments/assets/3370f8e4-9185-4896-bfe5-e3bcb3582dc2" />


*Así se ve nuestro carrito vulnerado:*

> <img width="1917" height="803" alt="Carrito_Preio_Modificado" src="https://github.com/user-attachments/assets/b6433e2f-3bac-456c-9ba7-04f4682d3051" />


### 6. Completar la Compra
Hacemos clic en "Place order" para finalizar la compra con nuestro crédito disponible. La compra se procesa exitosamente por $0.01 y el laboratorio es marcado como resuelto.

> <img width="1917" height="948" alt="Compra_Carrito" src="https://github.com/user-attachments/assets/22acf412-d923-496c-8648-ea7783b939d7" />


---

## 🛡️ Medidas de Mitigación
Para prevenir esta vulnerabilidad, el diseño de la aplicación debe cambiar drásticamente:
1. **Nunca confiar en datos del lado del cliente:** El precio de un producto no debe ser un parámetro enviado por el usuario.
2. **Validación en el Backend:** Al enviar un producto al carrito, el cliente solo debe enviar el `productId` y la `quantity`. El servidor debe consultar el precio real directamente en la base de datos al momento de calcular el total a pagar.

---
*Generado por Santiago Avendaño López*
