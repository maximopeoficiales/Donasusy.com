# Mapa de Google y calculo de envio automatico en Woocommerce

Se agregaron las siguiente funciones

- ✅ Mapa intereactivo para que el usuario eliga su direccion de recojo
- ✅ Calculo de envio automatico por codigo postal
- ✅ Link de Google Maps para obtener la ubicacion precisa del cleinte
- ✅ Impresion de detalle con el mapa

##### Importante:

Hay archivos que no es optimo pegar todo codigo de los archivos del repositorio, ya que si se actualiza woocomerce. quizas modifique algunas propiedades. Asi que es importante solo copiar los bloques de codigo especificados en la documentacion

#### Agregamos el plugin Chekout Fields de Woocomerce y creamos los siguiente campos

- ✅ Activamos el billing_postcode
- ✅ Creamos ce_distanciakm
- ✅ Creamos ce_latitud
- ✅ Creamos ce_longitud

**Nota:** Deben ser los mismo nombres y crearlos con ese orden si es posible para que funcione perfectamente el codigo

![Custom Fields](https://raw.githubusercontent.com/maximopeoficiales/Donasusy.com/master/imgs/CustomFields.PNG)

#### 1. Instalar el plugin head and footer scripts a Wordpress

Este plugin es necesario para poder agregar scripts en la pagina de **Finalizar Compra** , scripts como los iconos de FontAwesome,Google Maps y algunas variables globales

#### 2. Entrar al menu de todas las paginas

Aqui entramos a su pagina de **Checkout**

**Nota:** Una vez dentro recuerda la url asignada a checkout sin "/"

Ejemplo: "finalizar-compra" esta sera usada para ponerla en una variable constante

#### 3. Ponemos los scripts dentro de esta pagina

Especificamos las coordenadas de la tienda necesario para obtener lo km de distancia de la tienda hasta el lugar de envio.

En la variable **rutaFinalizarCompra** se especifica la url de la pagina checkout. Si no se especifica la **rutaFinalizarCompra** no se mostrara el mapa ni el calculo automatico por codigo postal.
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

**wp-content\plugins\woocommerce\includes\admin\class-wc-admin-assets.php**

Aqui estamos agregando los propiedades **ajax_url** para que sea accedido desde todo wp-admin(Wordpress).

Buscamos este script:

```php
wp_localize_script(
'accounting',
'accounting_params',
     array(
          'mon_decimal_point' => wc_get_price_decimal_separator(),
     )
);
```

Y Lo reemplazamos por este:

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

Buscamos este bloque de codigo:

```php

wp_localize_script('woodmart-theme', 'woodmart_settings', $translations);
```

Debajo desde este bloque pegamos:

```php
/* agrego mis scripts perzonalizados */
wp_enqueue_script('myscripts', WOODMART_SCRIPTS . '/myscripts.js', array(), '1.0.0', true);
/* fin de scripts */
```

#### 6. En la siguiente ruta creamos el archivo myscripts.js en la ruta:

**wp-content\themes\woodmart\js**

Este archivo lo encontramos en este repositorio en la carpeta **/cambios/myscripts.js**

En este archivo se explican las funciones que usamos para googlemaps como tambien para el calculo de envio automatico.

![mapa Google Maps](https://raw.githubusercontent.com/maximopeoficiales/Donasusy.com/master/imgs/mapaGoogleMaps.PNG)

#### 7. Modificando el Template del Modal de vision

Modificamos el template para agregar el mapa con la ubicacion exacta de lugar de entrega de nuestro cliente.Como tambien agregamos la funcion Imprimir Simple con **Javascript**
Esto estara en :

/Cambios/class-wc-admin-list-table-orders.php

Ruta: **wp-content\plugins\woocommerce\includes\admin\list-tables\class-wc-admin-list-table-orders.php**

Aqui modificamos toda esta funcion hasta el comentario **"fin de modficacion"**

```php
public function order_preview_template()

```

![template](https://raw.githubusercontent.com/maximopeoficiales/Donasusy.com/master/imgs/modalPedidoOjito.PNG)

#### 8.Modificacion en el template de edicion de pedido

Aqui se agrego el codigo para la obtencion de link de gmaps, la funcion imprimir, se uso una peticion ajax para obtener los datos y llenar el modal que se agrego.

Esto se encuentra en el repositorio:
/Cambios/class-wc-meta-box-order-data.php

Ruta: **wp-content\plugins\woocommerce\includes\admin\meta-boxes\class-wc-meta-box-order-data.php**

Buscamos en el archivo del repositorio

```php
<div class="order_data_column_container">
```

Aqui pegamos todo el codigo que se indica como comentario **bloque de codigo**

Luego buscamos:

```php
if (apply_filters('woocommerce_enable_order_notes_field', 'yes' == get_option('woocommerce_enable_order_comments', 'yes')) && $post->post_excerpt) {
  echo '<p class="order_note"><strong>' . __('Customer provided note:', 'woocommerce') . '</strong> ' . nl2br(esc_html($post->post_excerpt)) . '</p>';
}
?>
```

Debajo de este bloque de codigo pegamos:

```php
<!-- CAMPO url Gmaps -->
  <p id="url_mapa"></p>
<!-- campo url gmaps -->
```

![template](https://raw.githubusercontent.com/maximopeoficiales/Donasusy.com/master/imgs/modalImprimirResumenn.PNG)
