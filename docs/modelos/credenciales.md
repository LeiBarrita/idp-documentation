---
sidebar_position: 3
title: Credenciales
---

# Modelos - Credenciales

En Laravel, los **modelos** están vinculados a tablas en la base de datos y, a menudo, usan credenciales (por ejemplo, el nombre de la tabla, la conexión de la base de datos, etc.) que pueden necesitar ser configuradas o cambiadas. Aquí te explico cómo puedes cambiar las credenciales de un modelo de manera simple y detallada.

### Conceptos Clave

1. **Nombre de la Tabla**: El nombre de la tabla en la base de datos con la que está asociado el modelo.
2. **Conexión de Base de Datos**: La base de datos específica a la que el modelo se conecta.
3. **Primaria Clave**: El campo que actúa como identificador único en la tabla.

### Cambiar Credenciales del Modelo

#### 1. Cambiar el Nombre de la Tabla

Por defecto, Eloquent asume que el nombre de la tabla es el plural del nombre del modelo. Si tu tabla tiene un nombre diferente, puedes especificarlo en el modelo usando la propiedad `$table`.

**Ejemplo: Cambiar el Nombre de la Tabla**

Supongamos que tienes una tabla llamada `my_products`, pero tu modelo se llama `Product`. Puedes especificar el nombre de la tabla en el modelo así:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    // Especifica el nombre de la tabla
    protected $table = 'my_products';
}
```

#### 2. Cambiar la Conexión de Base de Datos

Si tu modelo necesita conectarse a una base de datos diferente a la configuración predeterminada, puedes especificar la conexión en el modelo usando la propiedad `$connection`.

**Ejemplo: Cambiar la Conexión de Base de Datos**

Supongamos que quieres que el modelo `Product` use una conexión llamada `mysql2` en lugar de la conexión predeterminada. Puedes configurar esto en el modelo de la siguiente manera:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    // Especifica la conexión de base de datos
    protected $connection = 'mysql2';
}
```

Asegúrate de definir esta conexión en el archivo `config/database.php`.

#### 3. Cambiar la Clave Primaria

Por defecto, Eloquent asume que el campo de la clave primaria es `id` y que es de tipo entero. Si tu tabla usa un nombre diferente para la clave primaria o un tipo diferente, puedes configurarlo en el modelo.

**Ejemplo: Cambiar la Clave Primaria**

Supongamos que la clave primaria de tu tabla es `product_id` y es una cadena de texto en lugar de un entero:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    // Especifica el nombre de la clave primaria
    protected $primaryKey = 'product_id';

    // Indica que la clave primaria no es un entero
    public $incrementing = false;

    // Indica que la clave primaria no es de tipo entero
    protected $keyType = 'string';
}
```

#### 4. Cambiar las Timestamps

Por defecto, Eloquent maneja los campos `created_at` y `updated_at` como timestamps. Si tu tabla no tiene estos campos o usa nombres diferentes, puedes desactivar o cambiar esta funcionalidad.

**Ejemplo: Desactivar Timestamps**

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    // Desactiva el manejo automático de timestamps
    public $timestamps = false;
}
```

**Ejemplo: Usar Nombres Diferentes para los Timestamps**

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    // Usa nombres diferentes para los campos de timestamps
    const CREATED_AT = 'creation_date';
    const UPDATED_AT = 'last_update';
}
```

### Resumen

- **Nombre de la Tabla**: Usa la propiedad `$table` para especificar un nombre de tabla diferente.
- **Conexión de Base de Datos**: Usa la propiedad `$connection` para especificar una conexión diferente.
- **Clave Primaria**: Usa `$primaryKey` para especificar un nombre diferente para la clave primaria y ajusta `$incrementing` y `$keyType` según sea necesario.
- **Timestamps**: Usa `$timestamps` para desactivar el manejo automático de timestamps o define constantes para nombres personalizados.

Estas configuraciones te permiten adaptar los modelos de Eloquent a diferentes estructuras de base de datos y requerimientos específicos.
