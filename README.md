# Modificaciones en doñamarce.com

#### Agregos plugin Chekout Fields de Woocomerce y creamos los siguiente campos

- [x] Activamos el billing_postcode
- [x] Creamos ce_distanciakm
- [x] Creamos ce_latitud
- [x] Creamos ce_longitud

**Nota:** Deben ser los mismo nombres para que funcione el codigo

![Custom Fields](https://raw.githubusercontent.com/maximopeoficiales/Donasusy.com/master/imgs/CustomFields.PNG)

#### 1. Instalar el plugin head and footer scripts a Wordpress

Este plugin es necesario para poder agregar scripts en la pagina de **Finalizar Compra** , scripts como los iconos de FontAwesome,Google Maps y algunas variables globales

#### 2. Entrar al menu de todas las paginas

Aqui entramos a su pagina de **Checkout**

**Nota:** Una vez dentro recuerda la url asignada a checkout sin "/"

Ejemplo: "finalizar-compra" esta sera usada para ponerla en una variable constante

#### 3. Ponemos los scripts dentro de esta pagina

Especificamos las coordenadas de la tienda necesario para obtener lo km de distancia de la tienda hasta el lugar de envio.

En la variable **rutaFinalizarCompra** se especifica la url de la pagina checkout.Si no se especifica la **rutaFinalizarCompra** no se mostrara el mapa ni el calculo automatico por codigo postal.
Si no especifica las coordenadas de la Tienda se pondra las coordenadas de Lima,Peru

```html
<script src="https://maps.googleapis.com/maps/api/js?key="></script>
<script
  src="https://kit.fontawesome.com/asdf.js"
  crossorigin="anonymous"
></script>
<script>
  /* Coordenadas de la Tienda */
  const TiendaLatitud = -12.055347200000002;
  const TiendaLongitud = -77.03101439999999;
  /* Aqui se pone la url de la pagina del checkout*/
  const rutaFinalizarCompra = "finalizar-compra";
</script>
```

![Custom Fields](https://raw.githubusercontent.com/maximopeoficiales/Donasusy.com/master/imgs/scriptHeader.PNG)

#### 4. Ahora dentro la siguiente ruta:

Aqui estamos agregando los propiedades **ajax_url** para que sea accedido desde todo wp-admin(Wordpress).

**wp-content\plugins\woocommerce\includes\admin\class-wc-admin-assets.php**

```php
wp_localize_script(
'accounting',
'accounting_params',
     array(
          'mon_decimal_point' => wc_get_price_decimal_separator(),
          /* le paso esta variable para que pueda ser accedida por todo el wp-admin */
          'ajax_url'      => admin_url('admin-ajax.php'),
          'preview_nonce' => wp_create_nonce('woocommerce-preview-order'),
     )
);
```

#### 5. Registramos el script personalizado

En este caso estamos usando el tema Woodmart entonces lo agregamos a:
**wp-content\themes\woodmart\functions.php**

Esto es para registrar un archivo de javascript personalizado en el tema.

```php
wp_localize_script('woodmart-theme', 'woodmart_settings', $translations);
/* agrego mis scripts perzonalizados */
wp_enqueue_script('myscripts', WOODMART_SCRIPTS . '/myscripts.js', array(), '1.0.0', true);
/* fin de scripts */
```

#### 6. En la siguiente ruta creamos el archivo myscripts.js en la ruta:

**wp-content\themes\woodmart\js**

Este archivo lo encontramos en este repositorio en la carpeta **/cambios/myscripts.js**

En este archivo se explican las funciones que usamos para googlemaps como tambien para el calculo de envio

Esto nos habilita el mapa de google maps, como tambien el calculo de envio automatico

![mapa Google Maps](https://raw.githubusercontent.com/maximopeoficiales/Donasusy.com/master/imgs/mapaGoogleMaps.PNG)

#### 7. Modificando el Template del Modal de vision

Modificamos el template para agregar el mapa con la ubicacion exacta de lugar de entraga de nuestro cliente.Como tambien agregamos la funcion Imprimir Simple con **Javascript**
Esto estara en :

/Cambios/class-wc-admin-list-table-orders.php

Ruta: **wp-content\plugins\woocommerce\includes\admin\list-tables\class-wc-admin-list-table-orders.php**

![template](https://raw.githubusercontent.com/maximopeoficiales/Donasusy.com/master/imgs/modalPedidoOjito.PNG)

#### 8.Modifacion en el template edit.php

Aqui se agrego el codigo para la obtencion de link de gmaps, la funcion imprimir, se uso una peticion ajax para obtener los datos y llenar el modal que se agrego.

Esto se encuentra en el repositorio:
/Cambios/class-wc-meta-box-order-data.php

Ruta: **wp-content\plugins\woocommerce\includes\admin\meta-boxes\class-wc-meta-box-order-data.php**

![template](https://raw.githubusercontent.com/maximopeoficiales/Donasusy.com/master/imgs/modalImprimirResumenn.PNG)
