# Racoon Shop

Catálogo digital público de Racoon para `https://shop.racoonjw.com`.

## Funcionalidad

- Productos activos y categorías activas.
- Búsqueda y filtro por categoría.
- Carga progresiva de productos.
- Cards responsivas.
- Detalle con imágenes, videos y PDF.
- Compra por WhatsApp.

El número usado por defecto es el de SIGA (`5215634861468`). Cámbialo en `js/catalogo.js` o define `window.RACOON_WHATSAPP` antes de cargar el módulo.

## Desarrollo local

El sitio usa `http://localhost:8030/api/v1` cuando se abre desde localhost. Para servirlo localmente se puede usar cualquier servidor estático, por ejemplo:

```bash
python3 -m http.server 5502
```

## Producción

El frontend no requiere Node.js. Nginx debe servir esta carpeta y enviar `/api/v1/` al backend Racoon en el puerto `8030`. Los archivos `/uploads/` deben apuntar a `backend/uploads/multimedia`.

Las plantillas Nginx para el servidor de aplicaciones y el load balancer están en `deploy/nginx`. Después de configurar el DNS `shop.racoonjw.com` hacia el load balancer:

```bash
sudo mkdir -p /var/www/racoon/ecommerce
sudo git clone https://github.com/bcssystems/racoonEcommerce.git /var/www/racoon/ecommerce
sudo ln -s /var/www/racoon/ecommerce/deploy/nginx/racoon-shop-origin.conf /etc/nginx/sites-enabled/racoon-shop-origin
sudo nginx -t && sudo systemctl reload nginx
sudo certbot --nginx -d shop.racoonjw.com
```

En el load balancer se debe instalar `racoon-shop-load-balancer.conf` en `sites-available`, crear su enlace en `sites-enabled` y ejecutar `nginx -t`. El backend debe permitir tráfico únicamente desde el Nginx local y el servidor de origen debe aceptar tráfico del load balancer.
