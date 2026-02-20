

# 🔍 Laboratorio: Insecure direct object references (IDOR)

**Plataforma:** PortSwigger Web Security Academy  
**Herramienta de Interceptación:** Caido  
**Objetivo:** Encontrar la contraseña filtrada del usuario `carlos` explotando una vulnerabilidad de referencia directa a objetos insegura (IDOR) en la función de descarga del historial de chat.

---

## 📝 Descripción de la Vulnerabilidad
La vulnerabilidad de Referencia Directa a Objetos Insegura (IDOR) ocurre cuando una aplicación proporciona acceso directo a objetos basándose en la entrada del usuario (como el nombre o ID de un archivo en la URL) sin verificar si el usuario tiene los permisos adecuados para acceder a ese recurso. 

En este laboratorio, los historiales del chat se guardan en archivos de texto con nombres predecibles y secuenciales (`1.txt`, `2.txt`, etc.). Al no existir un control de acceso, un atacante puede alterar este número para descargar las conversaciones privadas de otros usuarios, resultando en una **Exposición de Información** crítica.

## 🚀 Pasos para la Explotación

### 1. Exploración de la Funcionalidad
Comenzamos explorando la aplicación web y detectamos una funcionalidad de **Live chat** en la página principal.

> <img width="1914" height="1000" alt="Pagina_Principal" src="https://github.com/user-attachments/assets/36c29828-460f-4a57-a556-0362f12662f1" />


Interactuamos con el chat enviando un mensaje de prueba. Notamos que hay un botón llamado "View transcript" que nos permite descargar un registro de la conversación.

> <img width="1902" height="629" alt="Live_Chat" src="https://github.com/user-attachments/assets/9a2bd65b-baa7-4c2c-b273-f5f122272b88" />


Al hacer clic, el navegador descarga un archivo de texto llamado `2.txt`.

> <img width="541" height="113" alt="Descarga" src="https://github.com/user-attachments/assets/cbf91ce9-b4e6-4e52-bc4f-78ce40aafa30" />


### 2. Análisis del Tráfico en Caido
Nos dirigimos a la pestaña **HTTP History** en Caido para analizar qué sucedió "por debajo" cuando hicimos clic en el botón. Observamos que el sistema realizó una petición `GET` a la ruta `/download-transcript/2.txt`.

> <img width="1903" height="1037" alt="HTTP_HISTORY" src="https://github.com/user-attachments/assets/94e2c331-0d5f-4a13-b0fd-add8bf48f65c" />


### 3. Preparación del Ataque (Replay)
Para probar si podemos acceder a los historiales de otros usuarios, hacemos clic derecho en esa petición y la enviamos a la herramienta **Replay** de Caido. Aquí podemos ver la petición original pidiendo nuestro propio archivo (`2.txt`).

> <img width="1912" height="1043" alt="Replay_Num_Original" src="https://github.com/user-attachments/assets/c9a48c20-84f2-423d-8a74-7ca4fd6d190f" />


### 4. Explotación del IDOR (Extracción de credenciales)
Modificamos la ruta en la petición, cambiando el número del archivo de `2.txt` a `1.txt` para intentar acceder a la primera conversación registrada en el servidor. Al enviar la solicitud, el servidor responde con un código `200 OK` y nos devuelve el contenido de ese chat.

Analizando la respuesta, encontramos que el usuario reveló su contraseña durante esa conversación: `g5umuyktf4oo7s80y998`.

> <img width="1916" height="1041" alt="Replay_Num_Modificado" src="https://github.com/user-attachments/assets/cd95c232-ac77-4295-ad75-26a6e3a02105" />


### 5. Acceso al Sistema
Con la contraseña comprometida en nuestro poder, nos dirigimos a la página de **Login**. Ingresamos el usuario `carlos` y la contraseña obtenida.

> <img width="770" height="373" alt="Login" src="https://github.com/user-attachments/assets/23a242b7-259b-4518-9028-57eb336d41b2" />


### 6. Resolución del Laboratorio
El inicio de sesión es exitoso, logramos acceder a la cuenta de `carlos` y el laboratorio se marca como resuelto.

> <img width="1908" height="742" alt="Solve" src="https://github.com/user-attachments/assets/fca64590-b560-4639-a156-e702254bd7c6" />


---

## 🛡️ Medidas de Mitigación
Para prevenir vulnerabilidades IDOR, se deben implementar las siguientes prácticas:
1. **Controles de Acceso Estrictos:** El servidor debe validar siempre si el usuario autenticado tiene los permisos necesarios para acceder al objeto solicitado (en este caso, comprobar si el ID del chat le pertenece al usuario actual).
2. **Identificadores Indirectos o Impredecibles:** En lugar de usar números secuenciales predecibles (`1.txt`, `2.txt`), se recomienda usar UUIDs (Identificadores Únicos Universales) como `550e8400-e29b-41d4-a716-446655440000`, lo que hace casi imposible adivinar el nombre de otros archivos.

---
*Generado por Santiago Avendaño López*
