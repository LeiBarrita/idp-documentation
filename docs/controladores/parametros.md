---
sidebar_position: 2
title: Parametros
---

# Controladores - Parametros

Recibir parámetros en un controlador en Laravel es una forma de obtener datos enviados con una solicitud HTTP, ya sea desde una URL, un formulario, o en el cuerpo de la solicitud. Dependiendo del tipo de solicitud y cómo se envían los datos, puedes acceder a estos parámetros de diferentes maneras.

### Conceptos Clave

1. **Parámetros en la URL**: Datos enviados como parte de la URL, por ejemplo, `https://example.com/products/1` donde `1` es un parámetro.
2. **Datos en el Cuerpo de la Solicitud**: Datos enviados en el cuerpo de la solicitud, comúnmente en solicitudes POST o PUT, como en un formulario HTML.

3. **Objeto `Request`**: En Laravel, el objeto `Request` encapsula todos los datos de la solicitud, incluyendo parámetros de la URL, datos del cuerpo de la solicitud, y otros detalles.

### Recibir Parámetros

#### 1. Parámetros de la URL

Estos parámetros se especifican en la definición de la ruta y se capturan en el controlador como argumentos de método.

**Definir la Ruta con Parámetros**

En `routes/api.php`:

```php
use App\Http\Controllers\ProductController;
use Illuminate\Support\Facades\Route;

Route::get('/products/{id}', [ProductController::class, 'show']);
```

Aquí, `{id}` es un parámetro de la URL.

**Capturar el Parámetro en el Controlador**

En `ProductController.php`:

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ProductController extends Controller
{
    // Método para mostrar un producto específico
    public function show($id)
    {
        // Aquí, $id es el parámetro recibido de la URL
        return response()->json(['product_id' => $id]);
    }
}
```

En este ejemplo, cuando una solicitud se realiza a `/products/1`, el valor `1` se pasa al método `show` como el parámetro `$id`.

#### 2. Datos del Cuerpo de la Solicitud

Estos datos se envían en el cuerpo de la solicitud, típicamente en solicitudes POST o PUT. Puedes acceder a ellos usando el objeto `Request`.

**Definir la Ruta para Solicitudes POST**

En `routes/api.php`:

```php
Route::post('/products', [ProductController::class, 'store']);
```

**Capturar los Datos del Cuerpo en el Controlador**

En `ProductController.php`:

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;

class ProductController extends Controller
{
    // Método para crear un nuevo producto
    public function store(Request $request)
    {
        // Acceder a datos del cuerpo de la solicitud
        $name = $request->input('name');
        $price = $request->input('price');

        // Realizar alguna acción con los datos recibidos
        return response()->json([
            'name' => $name,
            'price' => $price,
            'message' => 'Product created successfully!'
        ]);
    }
}
```

En este ejemplo, si se envía una solicitud POST con un cuerpo que contiene `name` y `price`, el método `store` puede acceder a estos datos usando `$request->input('name')` y `$request->input('price')`.

### Resumen

- **Parámetros en la URL**: Se definen en la ruta y se capturan en el controlador como argumentos del método. Ejemplo: `/products/{id}`.

- **Datos del Cuerpo de la Solicitud**: Se envían en el cuerpo de la solicitud (para POST, PUT, etc.) y se accede a ellos mediante el objeto `Request`. Ejemplo: `$request->input('name')`.
