# data-modeling-and-business-intelligence
Proyecto integral de análisis de datos empresariales que comprende exploración, limpieza, transformación, modelado de datos y desarrollo de dashboards interactivos en Power BI para la generación de indicadores y conocimientos de negocio.

![Modelado de Datos](https://img.shields.io/badge/Modelado_de_Datos-4472C4?style=for-the-badge) ![Power BI](https://img.shields.io/badge/Power_BI-F2C811?style=for-the-badge&logo=powerbi&logoColor=black) ![DAX](https://img.shields.io/badge/DAX-217346?style=for-the-badge) ![ETL](https://img.shields.io/badge/ETL-6A5ACD?style=for-the-badge) ![Análisis de Datos](https://img.shields.io/badge/Análisis_de_Datos-008080?style=for-the-badge)

___

## Dataset

<img width="1301" height="815" alt="image" src="https://github.com/user-attachments/assets/fe650f20-a318-49d6-9c08-813ee6dbb733" />

___

## 1. Preparación y exploración de los datos

### 1.1. Revisar el modelo desordenado

1.  Revisar el modelo de datos original.
2.  Preparar el área de trabajo.
3.  Comprender el negocio.
4.  Comprender los datos.
5.  Identificar las tablas de dimensiones y de hechos.

### 1.2. Creación de las dimensiones

1.  Agrupar las tablas por entidad.
2.  Transformarlas en dimensiones limpias.
3.  Aplicar los estándares de limpieza.
4.  Repetir el proceso para cada dimensión.

### 1.3. Creación de las tablas de hechos

1.  Identificar un evento y definir su granularidad (*grain*).
2.  Construir la tabla de hechos a partir de sus detalles.
3.  Conectar cada dimensión.
4.  Realizar pruebas para validar que los valores numéricos se mantengan
    correctos.

### 1.4. Ajustes finales

1.  Volver a validar los estándares.
2.  Añadir la dimensión de fechas.
3.  Construir los cálculos.
4.  Añadir la seguridad a nivel de fila (RLS).
5.  Realizar la validación final.

## 2. Reglas del modelado

1.  Procurar que el modelo siga un esquema en estrella.
2.  Comprender los datos antes de realizar cambios.
3.  Proteger la integridad de los valores numéricos.
4.  Asignar a cada columna una función clara dentro del modelo.

## 3. Estándares de nomenclatura

1.  Utilizar el inglés como idioma para los nombres.
2.  Utilizar `snake_case` para la nomenclatura.
3.  Identificar las tablas con los prefijos correspondientes:
    -   `fact_` para las tablas de hechos.
    -   `dim_` para las tablas de dimensiones.
4.  Nombrar las columnas de claves primarias con el sufijo `_key` o
    `_id`:
    -   `_key` cuando la clave se crea durante el modelado.
    -   `_id` cuando la clave proviene de la fuente.
5.  Procurar que los nombres sean claros y fáciles de interpretar.

## 4. Preparación y exploración de los datos

### 4.1. Revisión de las tablas de origen

-   **`Address`**: contiene el identificador, las calles y la ciudad.

-   **`CAMPAIGN_LOG`**: contiene el tipo de campaña, el medio, las
    fechas de inicio y fin, el presupuesto, la fecha, las impresiones,
    los clics y los gastos. Se debe corregir el nombre de la tabla y
    evaluar si corresponde a una tabla de hechos.

-   **`campaign_skus`**: contiene el nombre de la campaña y el SKU. Se
    deben corregir los encabezados.

-   **`cities`**: contiene las ciudades y las regiones. Se deben
    corregir los encabezados. Esta tabla se relaciona con `Address`.

-   **`CUST_MASTER`**: contiene el identificador del cliente, el nombre
    del cliente (las entidades son empresas), el segmento, el
    identificador de la dirección, el gerente de cuenta, los términos de
    pago, `hash_key` y `source_id`. Se debe corregir el nombre de la
    tabla, eliminar `hash_key` y evaluar si `source_id` también puede
    eliminarse. Esta tabla se relaciona con `Address`.

-   **`customer_contacts`**: contiene el identificador del cliente, el
    nombre del contacto, el correo electrónico y el indicador de
    contacto principal. Se relaciona con `CUST_MASTER` mediante una
    relación de uno a muchos.

-   **`dim_order`**: contiene `OrderID`. Se debe corregir el encabezado
    y evaluar si la tabla puede eliminarse.

-   **`exchange_rate`**: contiene la moneda, la tasa de cambio y la
    fecha.

-   **`inventory`**: contiene el nombre del producto y las columnas
    correspondientes a los meses de 2025. Se deben corregir los
    encabezados y transformar los nombres de las columnas de fechas para
    representar los meses. También se debe evaluar si la tabla puede
    renombrarse como `inventory_2025`.

-   **`invoices_line`**: contiene el identificador de la línea de
    factura, el identificador de la factura, el nombre del producto y el
    importe.

-   **`INVOICES`**: contiene el identificador de la factura, el
    identificador de la orden, el nombre del cliente, la fecha de la
    factura y el importe. Se debe corregir el nombre de la tabla y
    evaluar si corresponde a una tabla de hechos. Se relaciona con
    `invoices_line` y, posiblemente, con una tabla de órdenes y con
    `CUST_MASTER`.

-   **`order_line_items`**: contiene el identificador de la línea, el
    identificador de la orden, el nombre del producto, la cantidad, el
    precio unitario, el costo unitario, el porcentaje de descuento y el
    total de la línea. Se relaciona con la tabla de productos.

-   **`ORDERS_2025`**: contiene el identificador de la orden, la
    referencia del sistema legado, el nombre del cliente, la ciudad y la
    región del cliente, la ciudad de destino, la ciudad de facturación,
    la fecha de la orden, el canal de la orden, el estado, la prioridad,
    el total de la orden, las notas de la orden, el mensaje de regalo y
    la fuente del archivo. Se podría eliminar la referencia del sistema
    legado, la fuente del archivo, las notas de la orden y el mensaje de
    regalo.

-   **`ORDERS_2026`**: contiene las mismas columnas que `ORDERS_2025`.
    Se pueden combinar ambas tablas mediante una operación de anexado
    (*append*), y se debe evaluar si la tabla resultante corresponde a
    una tabla de hechos.

-   **`payments`**: contiene el identificador del método de pago, el
    identificador de la factura, el nombre del cliente, la fecha de pago
    y el importe. Se relaciona con `INVOICES` y con la tabla de
    clientes. Se debe evaluar si corresponde a una tabla de hechos.

-   **`products`**: contiene el código del producto, el nombre del
    producto, la marca, la subcategoría, el precio unitario, el
    proveedor principal, la descripción, `hash_key` y `source_id`. Se
    pueden eliminar las columnas `hash_key` y `source_id`.

-   **`regions`**: contiene el nombre de la región. Se debe corregir el
    encabezado.

-   **`sales_target`**: contiene el periodo y el objetivo de ganancia.

-   **`security`**: contiene el correo electrónico del usuario y la
    región. Se debe corregir el nombre de la tabla y los encabezados. Se
    utilizará para implementar RLS.

-   **`Sheet1`**: contiene el identificador del envío, el identificador
    de la orden, el método de envío, la fecha de envío y la fecha de
    entrega. Se debe asignar un nombre apropiado a la tabla. Se
    relaciona con `ORDERS`.

-   **`shipments`**: contiene la misma información que `Sheet1`; por
    tanto, parece ser una tabla duplicada.

-   **`subcategories`**: contiene la columna `CategoriaSubcategoria`. Se
    debe dividir su contenido en dos columnas y corregir los
    encabezados.

-   **`user_detail`**: contiene el identificador del usuario, el límite
    de crédito y el teléfono. Se relaciona con `CUST_MASTER`.

## 5. Creación de las dimensiones

### 5.1. Dimensiones creadas

#### `dim_customer`

1.  Se utilizó `CUST_MASTER` como tabla principal.
2.  Se filtró `customer_contacts` para conservar únicamente los
    contactos principales.
3.  Se realizó una combinación (*merge*) entre `CUST_MASTER` y
    `customer_contacts`.
4.  Se realizó una combinación entre `CUST_MASTER` y `Address`.
5.  Se corrigieron los encabezados de `cities`.
6.  Se eliminaron de `CUST_MASTER` las columnas `AddressID`,
    `isPrimary`, `hash_key` y `source_id`.
7.  Se estandarizaron los nombres de las columnas.
8.  Se deshabilitó la carga de las tablas `cities`, `regions`,
    `customer_contacts`, `Address` y `user_details`.

#### `dim_product`

1.  Se utilizó `products` como tabla principal.
2.  Se corrigieron los encabezados de `subcategories`.
3.  Se dividieron los valores de `subcategories` en dos columnas.
4.  Se realizó una combinación entre `products` y `subcategories`.
5.  Se estandarizaron los nombres de las columnas.
6.  Se eliminaron las columnas `description`, `hash_key` y `source_id`.
7.  Se deshabilitó la carga de las tablas `products` y `subcategories`.

#### `dim_order_flag`

1.  Se conservaron las columnas relacionadas con la orden: `channel`,
    `channelCode`, `status` y `priority`.
2.  Se añadió una columna de clave (`key`).

#### `dim_campaign`

1.  Se conservaron las columnas `campaignName`, `channel`, `startDate`,
    `endDate` y `budget`.
2.  Se añadió una columna de clave (`key`).

#### `dim_geo`

1.  Se modificaron los encabezados de la tabla `cities`.
2.  Se añadió una columna de clave (`key`).

#### `dim_date`

1.  Se utilizó la función `CALENDARAUTO()` para generar la dimensión de
    fechas.

## 6. Creación de las tablas de hechos

### 6.1. Tablas de hechos creadas

#### `fact_sales`

1.  Se combinaron las tablas `ORDERS_2025` y `ORDERS_2026`.
2.  Se eliminaron las columnas `legacy_id`, `comments`, `giftcoments` y
    `source_file`.
3.  Se creó la dimensión `dim_order_flag` para retirar información
    considerada no esencial.
4.  Se añadió una tabla `channel` para dar contexto a la columna
    `channel_code` de `dim_order_flag`, y se realizó una combinación con
    dicha tabla.
5.  Se realizó una combinación con `dim_product`.
6.  Se realizó una combinación con `dim_order_flag`.
7.  Se eliminaron de `fact_sales` las columnas duplicadas que ya se
    encuentran en las tablas de dimensiones.
8.  Se eliminó `OrderTotal` de la tabla de órdenes porque es una columna
    preagregada.
9.  Se renombraron las columnas.

#### `fact_inventory`

1.  Se aplicó una transformación de anulación de dinamización
    (*unpivot*) a las columnas correspondientes a los meses de la tabla
    `inventory`.
2.  Se realizó una combinación con `dim_product`.
3.  Se eliminó la columna `product`.
4.  Se renombraron las columnas.

#### `fact_campaign_spend`

1.  Se retiraron las columnas `campaignName`, `channel`, `startDate`,
    `endDate` y `budget`.
2.  Se realizó una combinación con `dim_campaign`.

#### `fact_promotion_coverage` --- Tabla de hechos sin medidas (*factless fact table*)

1.  Se dividió el contenido de la columna `skus` utilizando el
    delimitador de texto correspondiente.
2.  Se aplicó la operación `Trim` para eliminar los espacios en blanco.
3.  Se realizaron combinaciones con `dim_product` y `dim_campaign`.
4.  Se eliminaron las columnas `skus` y `campaignName`.

#### `fact_order_process` --- Tabla de hechos de tipo *accumulating snapshot*

1.  Se realizó una combinación con la tabla `INVOICES`.
2.  Se realizó una combinación con la tabla `payments`.
3.  Se realizó una combinación con la tabla `shipments`.
4.  Se realizó una combinación con `dim_customer`.
5.  Se conservaron únicamente las columnas de fechas y la columna de
    clave de `dim_customer`.

#### `fact_sales_targets`

1.  Únicamente se estandarizaron los nombres de las columnas.
2.  La tabla no se conectó con ninguna otra tabla del modelo.

## 7. Ajustes finales del modelo

1.  Se estandarizaron las fechas con el formato `dd/mm/yyyy`.
2.  Se validó que todos los valores numéricos pudieran utilizarse en las
    operaciones de resumen correspondientes.
3.  Se generó una dimensión de fechas (`dim_date`) que se actualiza
    automáticamente para incluir los años nuevos.
4.  Se establecieron relaciones entre la dimensión de fechas y las
    tablas de hechos correspondientes.
5.  Se añadió una tabla oculta, `_measure`, para organizar los cálculos
    básicos y las medidas predeterminadas.

<img width="1091" height="722" alt="Pasted image 20261024180758" src="https://github.com/user-attachments/assets/57926739-5984-4c52-bba3-2e349d0dbf40" />

## 8. Seguridad a nivel de fila (RLS)

La seguridad se configuró de acuerdo con la guía de seguridad y el tipo
de filtrado que debe aplicarse a cada empleado.

En este caso, se relacionó la tabla `security` con `dim_customer`, ya
que se busca filtrar la información de `fact_sales` y
`fact_order_process` según la región asignada al usuario.

Se utilizó la siguiente expresión DAX:

``` dax
[region] =
    LOOKUPVALUE(
        security[region],
        security[user_email],
        USERPRINCIPALNAME()
    )
```

La expresión devuelve la región asociada al registro de `security` cuyo
`user_email` coincide con el nombre principal del usuario que ha
iniciado sesión, obtenido mediante `USERPRINCIPALNAME()`.

### 8.1. Pruebas de seguridad

Las pruebas se realizaron con los siguientes usuarios:

-   `nora.adel@arka.com`: Norteamérica.
-   `diego.ramos@arka.com`: Latinoamérica.

#### 8.1.1. Vista de administrador

<img width="667" height="548" alt="Pasted image 20261024200813" src="https://github.com/user-attachments/assets/bf10e3b4-fce0-4fc8-980a-cb46a0aa683a" />

Se verificó la vista del administrador.
#### 8.1.2. Vista de `nora.adel@arka.com`

<img width="759" height="535" alt="Pasted image 20261024205004" src="https://github.com/user-attachments/assets/66208533-6b38-4aae-a296-8af13e08109c" />

Se verificó que la usuaria visualizara la información correspondiente a
Norteamérica.

#### 8.1.3. Vista de `diego.ramos@arka.com`

<img width="762" height="534" alt="Pasted image 20261024204928" src="https://github.com/user-attachments/assets/00522926-ded7-4317-a139-4f3e4f0dde25" />

Se verificó que el usuario visualizara la información correspondiente a
Latinoamérica.

Se validó el funcionamiento de la seguridad a nivel de fila.


## Análisis de clientes

<img width="1464" height="816" alt="image" src="https://github.com/user-attachments/assets/c3d12ff5-2d4b-4a22-a298-554b7f98ca1a" />
