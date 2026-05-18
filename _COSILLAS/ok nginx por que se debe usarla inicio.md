NGINX sirve principalmente como servidor web y como **proxy inverso**: recibe las peticiones y las reparte al servicio que corresponda, además de poder servir archivos estáticos muy rápido. En proyectos frontend no siempre es obligatorio, pero suele ser muy útil cuando querés desplegar una SPA, servir HTML/CSS/JS, manejar rutas del frontend y ponerle delante compresión, caché o HTTPS.kinsta+2

## Para qué sirve en frontend

- Servir archivos estáticos como `index.html`, CSS, JS e imágenes de forma eficiente.vadavo+1
    
- Hacer que una app React, Vue o Angular funcione bien al recargar rutas internas, redirigiendo todo a `index.html` en una SPA.
    
- Actuar como capa intermedia entre el navegador y tu backend, por ejemplo para enviar `/api` a un servidor Node, Python o Java.kinsta+1
    
- Mejorar rendimiento con compresión, caché y manejo eficiente de muchas conexiones.tecnologia.euroinnova+1
    

## Por qué usarlo

No lo usás porque el frontend “no pueda vivir sin NGINX”, sino porque en producción te da una forma más sólida de servir tu app que abrir un servidor de desarrollo. También te simplifica el despliegue: un servidor puede servir tu frontend y reenviar las llamadas al backend sin que el cliente sepa dónde está cada servicio. Además, NGINX consume pocos recursos y escala bien cuando hay mucho tráfico.servidoresadmin+3

## Cuándo no hace falta

Si estás desarrollando localmente con Vite, Next dev server, React Scripts o similar, normalmente no necesitás NGINX. Tampoco es imprescindible si tu frontend se despliega en plataformas que ya resuelven esto por vos, como hosting estático o CDN con reglas de rewrites. En esos casos, la plataforma cumple el papel que NGINX tendría en un servidor propio.

## Ejemplo simple

Si tu frontend vive en `/` y tu API en `/api`, NGINX puede:

- servir tu build estático del frontend;
    
- enviar `/api` al backend;
    
- devolver `index.html` cuando el usuario entra a `/dashboard` o `/perfil`, para que la SPA maneje la ruta.
    

En resumen: usás NGINX cuando querés desplegar tu frontend de forma profesional, rápida y controlada; no es obligatorio en desarrollo, pero sí muy común en producción.takum+2


---

Sí: si tu sitio hoy corre en `localhost:3000`, NGINX se usa normalmente como **capa de entrada pública** delante de esa app, sobre todo cuando va a entrar gente externa. En producción no querés exponer directamente tu servidor de desarrollo; NGINX te ayuda a servir el frontend, manejar HTTPS, proteger el origen real y ordenar el tráfico para varios clientes.vadavo+3

## Comandos importantes

Estos son los más usados para administrar NGINX en Linux con `systemd`:extassisnetwork+2

```sh
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx
sudo systemctl status nginx
sudo systemctl enable nginx
sudo nginx -t
sudo nginx -v
```

## Para qué sirve cada uno

- `start`: inicia el servicio.ochobitshacenunbyte+1
    
- `stop`: lo detiene.extassisnetwork+1
    
- `restart`: lo reinicia por completo.ochobitshacenunbyte+1
    
- `reload`: recarga la configuración sin cortar tantas conexiones.extassisnetwork+1
    
- `status`: muestra si está activo o fallando.ochobitshacenunbyte+1
    
- `enable`: hace que arranque automáticamente al iniciar el sistema.donnierock+1
    
- `nginx -t`: prueba si la configuración está bien antes de aplicar cambios.donnierock+2
    

## Cómo encaja con localhost:3000

Lo habitual es dejar tu app corriendo internamente en `127.0.0.1:3000` y hacer que NGINX escuche en `80` o `443` hacia afuera. Luego NGINX reenvía las peticiones a tu app con `proxy_pass http://localhost:3000;`, así el usuario nunca ve el puerto 3000. Eso te deja cambiar la app sin exponer el servidor de desarrollo directamente.redhat+3

## Ejemplo útil

Una configuración típica para frontend sería esta:

```txt
server {
    listen 80;
    server_name midominio.com;

    location / {
        proxy_pass http://127.0.0.1:3000;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```
Con eso, el navegador entra a `http://midominio.com`, pero NGINX manda todo a tu app interna en `3000`.focusoft+1

## Por qué deberías usarlo

Si varios clientes externos van a usar tu sitio, NGINX te ayuda a poner una puerta única, estable y profesional delante de tu app. También te facilita sumar HTTPS, compresión, caché y reglas para rutas, cosas que en producción suelen ser necesarias. Además, puedes tener varias apps en el mismo servidor usando distintos `server_name` o subdominios, sin pelearte con puertos raros para cada cliente.stackoverflow+4

## Flujo recomendado

1. Tu frontend corre localmente en `localhost:3000`.
    
2. NGINX escucha en `80/443` y recibe el tráfico público.
    
3. NGINX hace proxy hacia `127.0.0.1:3000` o sirve el build estático.
    
4. Los clientes acceden por dominio, no por puerto.
    

Ese esquema es mucho más seguro y más ordenado que dejar el puerto 3000 abierto directamente al exterior.keepcoding+1

Si querés, te paso una configuración exacta para tu caso: **React/Vite**, **Next.js**, o **frontend estático**.