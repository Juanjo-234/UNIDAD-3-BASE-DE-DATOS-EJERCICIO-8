# UNIDAD-3-BASE-DE-DATOS-EJERCICIO-8
El diagrama muestra un **Modelo Relacional (MR)** orientado a la gestión de inventarios, depósitos, productos, proveedores y órdenes de abastecimiento o movimiento, compuesto por **7 tablas interconectadas**.

A continuación, se detalla la estructura de cada tabla y las relaciones representadas:

---

### 1. Tablas y Estructura

* **DEPOSITO**
* `ID_DEPOSITO` (PK)


* `NOMBRE`


* `DIRECCION`


* `RESPONSABLE_CARGO`


* `RESPONSABLE_NOMBRE`




* **STOCK_DEPOSTIO** *(Stock en Depósito)*
* `ID_DEPOSITO` (PK / FK)


* `CODIGO_BARRAS` (PK / FK)


* `STOCK_DISPONIBLE`




* **PRODUCTO**
* `CODIGO_BARRAS` (PK)


* `DESCRIPCION`


* `STOCK`


* `PESO`


* `DIMENSIONES`




* **STOCK_DEPOSTIO / DETALLS_ORDEN** *(Detalle de Orden)*
* `CODIGO_BARRAS` (PK / FK)


* `NUMERO_ORDEN` (PK / FK)


* `PRECIO`


* `CANTIDAD`




* **ORDEN**
* `NUM_ORDEN` (PK)


* `ID_DEPOSITO` (FK) — Relacionado con `DEPOSITO`


* `FEHCA_EMISION` *(Fecha de emisión)*


* `ESTADO`


* `DEPOSITO_DESTINO`


* `COSTO`




* **PRODUCTO_PROVEEDOR** *(Tabla intermedia / asociativa entre Producto y Proveedor)*
* `CODIGO_BARRAS` (PK / FK)


* `CUIT` (PK / FK)


* `PRECIO`




* **PROVEEDOR**
* `CUIT` (PK)


* `RAZON_SOCIAL`


* `PAIS_ORIGEN`


* `TELEFONO`


* `CORREO`





---

### 2. Relaciones Principales

* **Depósito y Producto (`STOCK_DEPOSTIO`):** La tabla asociativa `STOCK_DEPOSTIO` resuelve la relación de muchos a muchos entre los depósitos y los productos, permitiendo registrar el stock disponible específico de cada artículo en cada depósito en particular.


* **Depósito y Orden:** Un depósito puede emitir o gestionar múltiples órdenes de abastecimiento o traslado (`ORDEN`) a lo largo del tiempo, vinculadas mediante `ID_DEPOSITO`.


* **Orden y Productos (`DETALLS_ORDEN`):** La tabla intermedia `DETALLS_ORDEN` desglosa los ítems incluidos en cada orden de compra o movimiento, relacionando el `NUM_ORDEN` con el `CODIGO_BARRAS` del producto y registrando la cantidad y el precio acordado.


* **Producto y Proveedor (`PRODUCTO_PROVEEDOR`):** Permite vincular qué proveedores suministran qué productos (relación de muchos a muchos), especificando el precio de costo o provisión que ofrece cada proveedor para un producto determinado.
<img width="981" height="902" alt="EJERCICIO 8-MR drawio" src="https://github.com/user-attachments/assets/01b3b445-40d3-455f-8845-73b05a90929f" />

La imagen muestra el proceso completo de **normalización de bases de datos** aplicado de manera secuencial —desde una tabla inicial sin normalizar hasta alcanzar la Tercera Forma Normal (3NF)— para un sistema de gestión de inventarios, depósitos, productos, proveedores y órdenes de movimiento.

A continuación, se detalla lo que se realiza en cada fase representada en el esquema:

---

### 1. Fase Normal N°0 (Tabla Desnormalizada)

* **Estado inicial:** Se parte de una **única tabla grande y plana** donde cada fila representa un depósito y contiene listas anidadas y multivaluadas en una misma celda (productos y stock disponible en formato empaquetado, órdenes con sus detalles de productos, y proveedores con sus respectivos CUIT).
* **Problema:** Presenta grupos repetitivos, falta de atomicidad y una severa redundancia de datos, lo que genera anomalías operativas de inserción, actualización y borrado.

### 2. Primera Forma Normal (1NF)

* **Condición:** Se exige que todos los atributos contengan **valores atómicos** (indivisibles, un solo dato por celda) y que cada tabla disponga de una clave primaria identificativa.
* **Proceso aplicado:** Se desglosan los grupos anidados en filas tabulares individuales:
* **TABLA PLANILLA:** Aplana los datos operativos principales relacionando depósitos, códigos de barras, órdenes, cantidades, precios y fechas de emisión.
* **TABLA PRODUCTO_PROVEEDOR:** Extrae como una tabla independiente la relación entre los productos y los proveedores que los suministran, especificando el precio unitario.



### 3. Segunda Forma Normal (2NF)

* **Condición:** La base de datos debe estar en 1NF y **todos los atributos que no forman parte de la clave deben depender por completo de toda la clave primaria** (eliminando las dependencias parciales generadas por claves compuestas).
* **Proceso aplicado:** Se separan las entidades base según la clave exacta de la que dependen:
* **TABLA DEPOSITO:** Aísla la información propia de cada depósito (`ID_DEPOSITO` como PK).
* **TABLA STOCK_DEPOSTIO:** Tabla intermedia que vincula los depósitos con los productos para registrar el stock disponible específico (`ID_DEPOSITO` y `CODIGO_BARRAS` como clave compuesta).
* **TABLA PRODUCTOS:** Contiene el catálogo general de artículos (`CODIGO_BARRAS` como PK).
* **TABLA ORDEN:** Gestiona la cabecera de las órdenes de movimiento o abastecimiento emitidas por los depósitos.



### 4. Tercera Forma Normal (3NF)

* **Condición:** Debe estar en 2NF y **carecer de dependencias transitivas** (ningún atributo no clave debe depender de otro atributo que tampoco sea clave; todo atributo no clave debe depender estrictamente de la clave primaria).
* **Proceso aplicado:** Se termina de modularizar y aislar por completo todas las entidades independientes del sistema logístico, conformando las **7 tablas definitivas**:
* **TABLA DEPOSITO:** Almacena la información de los depósitos y sus responsables.
* **TABLA STOCK_DEPOSTIO:** Mantiene el control del stock disponible por depósito.
* **TABLA PRODUCTOS:** Contiene el detalle técnico y descriptivo del inventario.
* **TABLA PRODUCTO_PROVEEDOR y TABLA PROVEEDOR:** Desglosan la red de proveedores, sus datos de contacto y los precios de provisión.
* **TABLA ORDEN y TABLA DETALLES_ORDEN:** Estructuran de forma limpia las cabeceras de las órdenes y el desglose atómico de los productos, cantidades y precios incluidos en cada una.
<img width="4602" height="2372" alt="EJERCICIO 8-FASES DE NORMALIZACION drawio" src="https://github.com/user-attachments/assets/0e90a761-1783-435e-96bd-6b58d8192493" />


