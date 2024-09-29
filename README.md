
# Vue.js Publicador

Este proyecto es una aplicación frontend construida con Vue.js que permite a los usuarios enviar mensajes a través de un servicio HTTP a una API .NET. Los mensajes luego son publicados en un broker de mensajería RabbitMQ.

## Requisitos

- Node.js 12.x o superior
- npm o yarn
- RabbitMQ corriendo en la infraestructura

## Comandos

### Ejecutar la aplicación con Docker Compose

Para ejecutar la aplicación, utiliza el siguiente comando:

```bash
docker-compose up
```

Este comando levantará los servicios necesarios definidos en el archivo `docker-compose.yml` para ejecutar la aplicación.

### Compilar y recargar automáticamente para desarrollo

```bash
npm run serve
```

Este comando levanta un servidor local en `http://localhost:8080` con hot-reloading para facilitar el desarrollo.

### Compilar y minificar para producción

```bash
npm run build
```

Este comando genera los archivos optimizados para producción, listos para ser desplegados en un servidor.

### Corregir y arreglar archivos de linting

```bash
npm run lint
```

Ejecuta ESLint para corregir problemas de código y mantener una buena calidad de código.

## Despliegue

1. Luego de ejecutar `npm run build`, toma los archivos generados en la carpeta `dist/` y despliega en un servidor web (Nginx, Apache, etc.).
2. Asegúrate de configurar el proxy en el servidor web para apuntar a la API de .NET si es necesario.

## Interacción con otros componentes

- La aplicación Vue.js envía peticiones HTTP a un endpoint expuesto por el servicio **.NET Publicador** que luego envía los mensajes a RabbitMQ.
- RabbitMQ se encarga de distribuir los mensajes a otros servicios suscriptores, como el servicio que guarda en SQL Server y el servicio que distribuye a través de WebSockets.

## Personalizar configuración

Para más detalles sobre cómo personalizar la configuración del proyecto Vue.js, consulta la [documentación de Vue CLI](https://cli.vuejs.org/config/).
