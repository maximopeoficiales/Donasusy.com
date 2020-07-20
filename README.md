# Modificaciones en doñamarce.com

#### 1. Instalar el plugin head and footer scripts a Wordpress

#### 2. Entrar al menu todas las paginas

Aqui entramos a su pagina de **chekout**

**Nota:** Una vez dentro apunta la url checkout sin "/"

Ejemplo: finalizar-compra esta sera usada para ponerla en una variable

#### 3. Ponemos los scripts dentro de esta pagina

```html

```

#### 4. Ahora dentro la siguiente ruta:
Aqui estamos agregando los propiedades **ajax_url** para que sea accedido desde todo wp-admin(Wordpress) 
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
#### 5. Agregando con un gancho
En este caso estamos usando el tema Woodmart entonces le agregamos a:
**wp-content\themes\woodmart\functions.php**

Esto es para registrar un archivo de javascript personalizado en el tema
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


#### 7. Modificando el template wp-admin
Modificamos el template para agregar el mapa con la ubicacion exacta de lugar de entraga de nuestro cliente.Como tambien agregamos la funcion Imprimir Simple con **Javascript**
Esto estara en : 

/Cambios/class-wc-admin-list-table-orders.php

Ruta: **wp-content\plugins\woocommerce\includes\admin\list-tables\class-wc-admin-list-table-orders.php**

#### 8.Modifacion en el template edit.php

Aqui se agrego el codigo para la obtencion de link de gmaps, la funcion imprimir, se uso una peticion ajax para obtener los datos y llenar el modal que se agrego.

Esto se encuentra en el repositorio:
/Cambios/class-wc-meta-box-order-data.php

Ruta: **wp-content\plugins\woocommerce\includes\admin\meta-boxes\class-wc-meta-box-order-data.php**




