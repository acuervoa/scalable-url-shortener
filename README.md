# Scalable URL Shortener

Este proyecto es una aplicación de acortador de URL escalable implementada con Node.js, Express y Redis. El objetivo es distribuir las claves de forma equilibrada entre múltiples instancias de Redis para garantizar la alta disponibilidad y escalabilidad del sistema.

Esta desarrollado a partir del artículo https://www.freecodecamp.org/news/build-a-scalable-url-shortener-with-distributed-caching-using-redis/ habiendo modificado la gestión de servidores Redis para utilizar promesas

## Características
- **Escalabilidad**: Se utiliza múltiples instancias de Redis para distribuir las cargas de trabajo de almacenamiento de URLs.
- **Redis Sharding**: Distribuye las claves entre varios clientes de Redis, lo cual permite un balance de carga efectivo.
- **APIs Simples**: Ofrece endpoints RESTful para acortar URLs y redirigir a las URLs originales.
- **Configurabilidad**: Utiliza variables de entorno para configurar fácilmente los detalles del servidor y de Redis.

## Tecnologías Utilizadas
- **Node.js**: Entorno de ejecución para JavaScript en el backend.
- **Express.js**: Framework web minimalista y flexible para construir las APIs.
- **Redis**: Almacenamiento de datos en memoria para manejar el mapeo entre URLs cortas y originales.

## Requisitos Previos
- **Node.js** >= v14.x.x
- **Redis** >= v4.x.x
- **npm** o **yarn** para instalar las dependencias

## Instalación

1. Clonar el repositorio:
   ```bash
   git clone https://github.com/acuervoa/scalable-url-shortener.git
   cd scalable-url-shortener
   ```

2. Instalar las dependencias:
   ```bash
   npm install
   # o
   yarn install
   ```

3. Crear un archivo `.env` en el directorio principal y configurar los valores necesarios:
   ```env
   PORT=3000
   REDIS_HOST_1=localhost
   REDIS_PORT_1=6379
   REDIS_HOST_2=localhost
   REDIS_PORT_2=6380
   REDIS_HOST_3=localhost
   REDIS_PORT_3=6381
   ```

4. Asegurarse de que todas las instancias de Redis estén funcionando en los puertos especificados.
   Podemos utilizar docker para los servidores Redis:

	```bash
	docker run -p 6379:6379 --name redis1 -d redis
	docker run -p 6380:6379 --name redis2 -d redis
	docker run -p 6381:6379 --name redis3 -d redis
	```   

## Uso

1. Iniciar el servidor:
   ```bash
   npm start
   # o
   yarn start
   ```

2. El servidor debería estar funcionando en `http://localhost:3000`.

3. Endpoints disponibles:
   - **POST /shorten**: Acortar una URL.
     ```json
     {
       "url": "https://example.com",
       "ttl": 3600  // opcional, tiempo de vida en segundos
     }
     ```
     Respuesta:
     ```json
     {
       "shortUrl": "http://localhost:3000/<shortId>"
     }
     ```
   - **GET /:shortId**: Redirigir a la URL original usando un ID corto.

## Ejemplo de Petición

Para acortar una URL, se puede hacer una petición POST:

```bash
curl -X POST -H "Content-Type: application/json" -d '{"url": "https://example.com"}' http://localhost:3000/shorten
```

## Arquitectura

El proyecto utiliza un mecanismo de sharding simple para distribuir las claves entre múltiples instancias de Redis. Cada clave se asigna a una instancia de Redis en base a un valor hash de la clave, lo cual asegura que la carga se balancee eficientemente entre los distintos nodos de Redis.

## Mejoras Futuras
- **Autenticación y Autorización**: Agregar seguridad para controlar el acceso a las APIs.
- **Métricas y Monitoreo**: Integrar herramientas como Prometheus o Grafana para el monitoreo del sistema.
- **Interfaz Gráfica**: Crear una interfaz web para facilitar la interacción con el acortador de URLs.

## Contribuciones
Las contribuciones son bienvenidas. Por favor, abre un **issue** para cualquier sugerencia o envía un **pull request** con tus mejoras.

## Licencia
Este proyecto está licenciado bajo la [MIT License](./LICENSE).

