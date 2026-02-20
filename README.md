# 🛡️ Portafolio de Ciberseguridad: Web Security Write-ups

Bienvenido a mi repositorio de prácticas de Hacking Web. Como estudiante de Ingeniería Electrónica enfocado en el área de la ciberseguridad, he creado este espacio para documentar mis resoluciones paso a paso (*write-ups*) de los laboratorios de la **Web Security Academy de PortSwigger**.

El objetivo principal de este repositorio es demostrar de forma práctica el proceso de análisis, explotación de diversas vulnerabilidades web y la explicación de sus respectivas medidas de mitigación.

## 🛠️ Herramientas Utilizadas
* **Plataforma de entrenamiento:** PortSwigger Web Security Academy
* **Proxy HTTP / Interceptador:** Caido
* **Entorno de Trabajo:** Distribuciones de seguridad (ej. Kali Linux)

---

## 📚 Índice de Laboratorios Resueltos

A continuación, se presenta la lista de vulnerabilidades explotadas hasta el momento. Haz clic en cada enlace para acceder a la documentación detallada, capturas de pantalla del tráfico interceptado y el proceso de explotación:

### 🛒 Vulnerabilidades de Lógica de Negocio (Business Logic)
* [**Excessive trust in client-side controls**](./Excessive-Trust-Lab)
    * *Descripción:* Explotación de una validación deficiente en el lado del servidor, manipulando los parámetros de una petición POST para alterar el precio final de un producto en el carrito de compras.

### 🔑 Control de Acceso (Access Control)
* [**Insecure direct object references (IDOR)**](./Insecure-Direct-Object-References)
    * *Descripción:* Descubrimiento y explotación de un IDOR en la función de descarga de historiales de chat,
