---
sidebar_position: 1
title: Introducción
---

# Controladores - Introducción

En Laravel, es común usar **controladores** para gestionar la lógica de cada ruta. Un controlador organiza el código y facilita el mantenimiento. Por ejemplo, el `UserController` sería una clase que contiene métodos como `index`, `store`, `show`, `update`, y `destroy`, cada uno manejando una acción específica para los usuarios.

Crear un controlador para una API en Laravel es un proceso que te permite gestionar cómo se manejan las solicitudes HTTP y cómo se responden. Un controlador actúa como el "gestor" de las solicitudes que llegan a una ruta específica, realizando operaciones como acceder a datos, procesar información, y devolver respuestas.

### Conceptos Clave

1. **Controlador**: Es una clase en Laravel que contiene métodos para manejar las solicitudes HTTP. Cada método en el controlador suele corresponder a una acción específica, como mostrar datos o guardar información en la base de datos.

2. **Métodos del Controlador**: Son funciones dentro del controlador que procesan diferentes tipos de solicitudes. Por ejemplo, `index` para obtener una lista de recursos, `store` para crear un nuevo recurso, `show` para mostrar un recurso específico, `update` para actualizar un recurso, y `destroy` para eliminar un recurso.

3. **Rutas**: Definen las URLs que los usuarios pueden visitar y que están asociadas con métodos específicos del controlador.

### Crear un Controlador

#### 1. Crear el Controlador

Puedes crear un controlador usando el comando Artisan de Laravel. Por ejemplo, para crear un controlador llamado `ProductController`:

```bash
php artisan make:controller Api/ProductController
```

Esto generará un archivo `ProductController.php` en el directorio `app/Http/Controllers/Api`.

#### 2. Definir Métodos en el Controlador

Abre el archivo `ProductController.php` y define los métodos necesarios. Aquí hay un ejemplo básico:

```php
namespace App\Http\Controllers\Api;

use Illuminate\Http\Request;

class ProductController extends Controller
{
    // Método para obtener una lista de productos
    public function index()
    {
        // Aquí se obtendrían los productos de la base de datos
        return response()->json(['message' => 'List of products']);
    }

    // Método para crear un nuevo producto
    public function store(Request $request)
    {
        // Aquí se procesaría la creación del producto
        return response()->json(['message' => 'Product created']);
    }

    // Método para mostrar un producto específico
    public function show($id)
    {
        // Aquí se obtendría el producto con el ID dado
        return response()->json(['message' => 'Product details for ID ' . $id]);
    }

    // Método para actualizar un producto específico
    public function update(Request $request, $id)
    {
        // Aquí se procesaría la actualización del producto
        return response()->json(['message' => 'Product updated for ID ' . $id]);
    }

    // Método para eliminar un producto específico
    public function destroy($id)
    {
        // Aquí se procesaría la eliminación del producto
        return response()->json(['message' => 'Product deleted for ID ' . $id]);
    }
}
```

#### 3. Definir las Rutas o Endpoints

Ahora, necesitas definir las rutas en el archivo `routes/api.php` para que las solicitudes HTTP sean dirigidas a los métodos del controlador. Puedes hacer esto de la siguiente manera:

```php
use App\Http\Controllers\ProductController;
use Illuminate\Support\Facades\Route;

// Grupo de rutas para productos
Route::prefix('products')->group(function () {
    Route::get('/', [ProductController::class, 'index']);        // GET /api/products
    Route::post('/', [ProductController::class, 'store']);       // POST /api/products
    Route::get('{id}', [ProductController::class, 'show']);       // GET /api/products/{id}
    Route::put('{id}', [ProductController::class, 'update']);     // PUT /api/products/{id}
    Route::delete('{id}', [ProductController::class, 'destroy']); // DELETE /api/products/{id}
});
```

### Resumen

- **Controlador**: Clase que maneja la lógica de la aplicación para diferentes tipos de solicitudes.
- **Métodos del Controlador**: Definen cómo manejar cada tipo de solicitud (GET, POST, PUT, DELETE).
- **Rutas**: Definen cómo las solicitudes HTTP se dirigen a los métodos del controlador.
- **Ejemplo**: Creación de un controlador `ProductController` para manejar productos, con métodos para listar, crear, mostrar, actualizar y eliminar productos, y configuración de rutas en `api.php`.

Este enfoque organiza la lógica de tu API de manera clara y modular, facilitando su mantenimiento y extensión.
