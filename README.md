# ADBD-Practica3-EduardoMarichal

## 1. Descripción de las entidades definidas

*   **Vivero**: Representa cada uno de los establecimientos físicos de la empresa Tajinaste S.A. donde se venden los productos.
*   **Zona (Entidad Débil)**: Corresponde a las diferentes áreas o secciones físicas dentro de un vivero específico. Es una entidad débil dependiente en existencia de `Vivero`.
*   **Producto**: Modela los artículos que la empresa comercializa.
*   **Empleado**: Registra a los trabajadores de Tajinaste S.A. que son destinados a trabajar en los viveros.
*   **Pedido**: Representa una transacción de compra específica realizada dentro del programa de fidelización.
*   **Cliente**: Entidad que almacena los datos de los compradores, necesaria para llevar el control del programa de fidelización "Tajinaste Plus" y sus bonificaciones.

---

## 2. Descripción y dominios de los atributos

### Entidad: Vivero
*   **id_vivero (PK)**: Identificador único del vivero. *Dominio: Entero positivo (ej. `102`)*.
*   **latitud**: Coordenada de latitud para la georreferenciación. *Dominio: Decimal (ej. `28.4822`)*.
*   **longitud**: Coordenada de longitud para la georreferenciación. *Dominio: Decimal (ej. `-16.3211`)*.

### Entidad: Zona (Débil)
*   **id_zona (Weak PK)**: Identificador parcial discriminador de la zona dentro de un vivero. *Dominio: Cadena de texto o Entero (ej. `Z-EXT` o `1`)*.
*   **nombre**: Denominación descriptiva de la zona. *Dominio: Cadena de texto (ej. `"Almacén principal"`)*.
*   **latitud**: Coordenada específica de la zona. *Dominio: Decimal (ej. `28.4824`)*.
*   **longitud**: Coordenada específica de la zona. *Dominio: Decimal (ej. `-16.3215`)*.

### Entidad: Producto
*   **id_producto (PK)**: Identificador único del artículo. *Dominio: Cadena alfanumérica (ej. `"PRD-8891"`)*.
*   **nombre**: Nombre comercial del producto. *Dominio: Cadena de texto (ej. `"Rosal enano"`)*.
*   **categoria**: Clasificación del producto. *Dominio: Cadena de texto (ej. `"Plantas de interior"`, `"Decoración"`)*.
*   **precio**: Precio de venta unitario. *Dominio: Decimal monetario (ej. `15.99`)*.

### Entidad: Empleado
*   **id_empleado (PK)**: Código identificador del trabajador. *Dominio: Entero (ej. `405`)*.
*   **dni**: Documento de identidad. *Dominio: Cadena de texto (ej. `"12345678A"`)*.
*   **nombre**: Nombre de pila. *Dominio: Cadena de texto (ej. `"Laura"`)*.
*   **apellido_1 / apellido_2**: Apellidos del empleado. *Dominio: Cadena de texto (ej. `"Pérez"`, `"Gómez"`)*.
*   **telefono**: Número de contacto. *Dominio: Cadena de texto o Entero (ej. `"+34600123456"`)*.
*   **email**: Correo corporativo. *Dominio: Cadena de texto formato email (ej. `"laura.p@tajinaste.es"`)*.

### Entidad: Pedido
*   **id_pedido (PK)**: Identificador de la compra. *Dominio: Entero (ej. `99214`)*.
*   **fecha**: Fecha y hora de la formalización del pedido. *Dominio: Fecha/Hora (ej. `"2026-10-07 10:30:00"`)*.
*   **estado**: Situación actual del envío/entrega. *Dominio: Cadena de texto (ej. `"En proceso"`, `"Entregado"`)*.

### Entidad: Cliente
*   **id_cliente (PK)**: Código único de cliente. *Dominio: Entero (ej. `7001`)*.
*   **es_tajinaste_plus**: Indicador de pertenencia al programa VIP. *Dominio: Booleano (ej. `True` o `False`)*.
*   **pedidos_realizados (Derivado)**: Cantidad total de compras hechas. Se calcula a partir de las relaciones con la entidad Pedido. *Dominio: Entero positivo (ej. `12`)*.
*   **bonificaciones**: Saldo de puntos o descuentos acumulados. *Dominio: Decimal (ej. `25.50`)*.
*   **nombre**: Nombre del cliente. *Dominio: Cadena de texto (ej. `"Carlos"`)*.
*   **apellido_1 / apellido_2**: Apellidos del cliente. *Dominio: Cadena de texto (ej. `"Pérez"`, `"Gómez"`)*.
*   **telefono**: Número de contacto. *Dominio: Cadena de texto o Entero (ej. `"+34600112233"`)*.
*   **email**: Correo del cliente. *Dominio: Cadena de texto formato email (ej. `"carlos@mail.es"`)*.

---

## 3. Descripción y cardinalidad de las relaciones

*   **Relación Identificadora: `tiene` (Vivero - Zona)**
    *   *Descripción*: Asigna zonas físicas a un vivero concreto.
    *   *Cardinalidad*: **1:N**. 
        *   Un `Vivero` tiene un mínimo de 1 y un máximo de N (muchas) zonas `(1,N)`.
        *   Una `Zona` pertenece estrictamente a un mínimo de 1 y un máximo de 1 vivero `(1,1)`.

*   **Relación: `almacenado` (Zona - Producto)**
    *   *Descripción*: Define la ubicación y disponibilidad de los productos dentro de las zonas de los viveros.
    *   *Atributo en la relación*: `stock` (Cantidad disponible del producto en esa zona). *Dominio: Entero (ej. `50`)*.
    *   *Cardinalidad*: **N:M**. 
        *   Una `Zona` puede almacenar desde 0 hasta N productos `(0,N)`.
        *   Un `Producto` puede estar almacenado desde en 0 hasta N zonas `(0,N)`.

*   **Relación: `asignado` (Zona - Empleado)**
    *   *Descripción*: Registra el histórico de los destinos laborales de los empleados en diferentes zonas a lo largo del tiempo.
    *   *Atributos en la relación*: 
        *   `fecha_inicio`: Inicio del destino. *Dominio: Fecha (ej. `"2026-01-01"`)*.
        *   `fecha_fin`: Fin del destino. *Dominio: Fecha (ej. `"2026-06-30"`)*.
        *   `productividad`: Métrica de desempeño en ese periodo. *Dominio: Decimal o Porcentaje (ej. `85.5`)*.
    *   *Cardinalidad*: **N:M**. 
        *   Una `Zona` tiene un histórico desde 1 hasta N empleados asignados `(1,N)`.
        *   Un `Empleado` ha estado asignado históricamente desde 0 hasta N zonas `(0,N)`.

*   **Relación: `gestiona` (Empleado - Pedido)**
    *   *Descripción*: Vincula a los empleados responsables de las ventas para calcular su cumplimiento de objetivos.
    *   *Cardinalidad*: **1:N**.
        *   Un `Empleado` puede gestionar desde 0 hasta N pedidos `(0,N)`.
        *   Un `Pedido` es gestionado obligatoriamente por 1 y solo 1 empleado `(1,1)`.

*   **Relación: `realiza` (Cliente - Pedido)**
    *   *Descripción*: Relaciona las compras formales con el cliente del programa de fidelización.
    *   *Cardinalidad*: **1:N**.
        *   Un `Cliente` puede realizar desde 0 hasta N pedidos `(0,N)`.
        *   Un `Pedido` es realizado de forma exclusiva por 1 y solo 1 cliente `(1,1)`.

*   **Relación: `tiene` (Pedido - Producto)**
    *   *Descripción*: Detalla los artículos específicos que se han incluido en una transacción de compra.
    *   *Atributo en la relación*: `cantidad` (Unidades del producto concreto en el pedido). *Dominio: Entero positivo (ej. `3`)*.
    *   *Cardinalidad*: **N:M**.
        *   Un `Pedido` debe contener obligatoriamente desde 1 hasta N productos `(1,N)`.
        *   Un `Producto` puede figurar desde en 0 hasta N pedidos a lo largo del tiempo `(0,N)`.

---

## 4. Restricciones semánticas

Se establecen las siguientes restricciones de negocio que no pueden representarse estructuralmente en el diagrama E/R:

1.  **Exclusividad temporal de destino**: Un empleado nunca puede tener dos destinos activos simultáneamente. Las fechas en los atributos `fecha_inicio` y `fecha_fin` de la relación `asignado` para un mismo `id_empleado` no pueden solaparse en el tiempo bajo ninguna circunstancia.
2.  **Integridad de pedidos en el programa de fidelización**: Todo registro instanciado en la entidad `Pedido` debe estar obligatoriamente vinculado (a través de la relación `realiza`) con un `Cliente` cuyo atributo `es_tajinaste_plus` tenga el valor Verdadero (`True`).
