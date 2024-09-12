---
sidebar_position: 1
---

# Rutas - Introducción

Una ruta en Laravel es la definición de una URL específica que asocia una solicitud HTTP (GET, POST, PUT, PATCH, DELETE) con un [controlador](../controladores/introduccion.md) o función que maneja la lógica correspondiente.

En Laravel, las rutas son esenciales para definir cómo tu aplicación responde a las solicitudes HTTP. En el caso de las **APIs**, las rutas que las gestionan se encuentran en el archivo `routes/api.php`. Este archivo es el lugar donde defines las URL que tu API va a exponer y cómo deben ser procesadas. Laravel hace que este proceso sea simple y estructurado.

### Conceptos Clave

1. **Archivo `api.php`**: Todas las rutas definidas en este archivo automáticamente están **"preconfiguradas"** para usar el prefijo `/api` en la URL. Por ejemplo, si defines una ruta para `/users`, la URL final sería `https://tusitio.com/api/users`.

2. **Verbos HTTP**: Las rutas en una API comúnmente usan diferentes métodos HTTP como:
   - **GET**: Para obtener recursos.
   - **POST**: Para crear nuevos recursos.
   - **PUT/PATCH**: Para actualizar recursos.
   - **DELETE**: Para eliminar recursos.

### Ejemplo Básico

Supongamos que queremos crear rutas para gestionar usuarios en una API. Dentro del archivo `api.php`, podemos definir las siguientes rutas:

```php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;

Route::get('/users', 'UserController@index'); // Obtener todos los usuarios
Route::post('/users', 'UserController@store'); // Crear un nuevo usuario
Route::get('/users/{id}', 'UserController@show'); // Obtener un usuario específico
Route::put('/users/{id}', 'UserController@update'); // Actualizar un usuario específico
Route::delete('/users/{id}', 'UserController@destroy'); // Eliminar un usuario específico
```

### Desglose

- `Route::get('/users', 'UserController@index');`: Esta ruta responde a solicitudes GET y llama al método `index` del controlador `UserController`. Generalmente, el método `index` se usa para devolver una lista de usuarios.
- `Route::post('/users', 'UserController@store');`: Esta ruta responde a solicitudes POST para crear un nuevo usuario y llama al método `store`.

- `Route::get('/users/{id}', 'UserController@show');`: Esta ruta responde a solicitudes GET con un parámetro `{id}`, que permite obtener un usuario específico con ese `id`.

- `Route::put('/users/{id}', 'UserController@update');`: Esta ruta se usa para actualizar un usuario existente y llama al método `update` del controlador, basado en el `id` del usuario.

- `Route::delete('/users/{id}', 'UserController@destroy');`: Permite eliminar un usuario por su `id`.

```php title="routes\api.php"
<?php

use App\Http\Controllers\Api\FGESolicitudController;
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Route;
use App\Http\Controllers\ApiIntercommunication;

Route::middleware('auth:sanctum')->get('/user', function (Request $request) {
    return $request->user();
});


Route::middleware('basic.auth')->group(function () {

    Route::get('/catalogos/defensores', [ApiIntercommunication::class, 'getDefensores']);

    Route::post('/solicitudes/defensores', [FGESolicitudController::class, 'createRequest']);
    Route::get('/solicitudes/defensores', [FGESolicitudController::class, 'getRequests']);
});
```
