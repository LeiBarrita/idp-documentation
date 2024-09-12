---
sidebar_position: 2
---

# Rutas - Middleware

Un **middleware** en Laravel es una capa intermedia que inspecciona, filtra o modifica solicitudes HTTP **antes** de que lleguen a las rutas o controladores, y **después** de que las respuestas se envíen al cliente. En el caso de las APIs, los middlewares suelen usarse para tareas como la **autenticación**, la **verificación de permisos**, o la **modificación de datos** antes de que lleguen a su destino.

Piensa en un middleware como un guardia de seguridad que decide si una solicitud puede pasar o si debe ser bloqueada. Puedes aplicar middleware a rutas específicas para protegerlas o procesar las solicitudes antes de entregarlas al controlador correspondiente.

### Usos Comunes

1. **Autenticación**: Verificar si el usuario está autenticado antes de acceder a una ruta.
2. **Autorización**: Verificar si el usuario tiene permisos suficientes para realizar una acción.
3. **Control de Cors**: Permitir o bloquear solicitudes desde diferentes dominios (CORS).
4. **Transformación de datos**: Modificar la solicitud o la respuesta.

### Cómo Usar un Middleware

1. **Crear un Middleware**:

   Puedes generar un middleware con el siguiente comando:

   ```bash
   php artisan make:middleware CheckToken
   ```

   Esto creará un archivo en `app/Http/Middleware/CheckToken.php`. Dentro de este archivo, puedes definir la lógica que desees aplicar a las solicitudes. Un middleware básico que verifique la existencia de un token en la solicitud podría verse así:

   ```php
   namespace App\Http\Middleware;

   use Closure;

   class CheckToken
   {
       /**
        * Handle an incoming request.
        *
        * @param  \Illuminate\Http\Request  $request
        * @param  \Closure  $next
        * @return mixed
        */
       public function handle($request, Closure $next)
       {
           if (!$request->has('api_token')) {
               return response()->json(['message' => 'API token missing'], 401);
           }

           return $next($request); // Permite que la solicitud continúe
       }
   }
   ```

   En este ejemplo, el middleware verifica si la solicitud incluye un parámetro `api_token`. Si no está presente, devuelve una respuesta con un error 401.

2. **Registrar el Middleware**:

   Después de crear el middleware, debes **registrarlo** en el archivo `app/Http/Kernel.php`, dentro del arreglo `$routeMiddleware` para poder usarlo en rutas específicas:

   ```php
   protected $routeMiddleware = [
       // Otros middlewares
       'check.token' => \App\Http\Middleware\CheckToken::class,
   ];
   ```

3. **Aplicar el Middleware a Rutas**:

   Una vez registrado, puedes aplicar el middleware a rutas individuales o a grupos de rutas. Para una API, normalmente definirías las rutas en el archivo `routes/api.php`.

   - **En una ruta específica**:

     ```php
     Route::get('/protected-route', 'SomeController@index')->middleware('check.token');
     ```

   - **Para un grupo de rutas**:

     ```php
     Route::middleware('check.token')->group(function () {
         Route::get('/protected-route', 'SomeController@index');
         Route::post('/protected-data', 'SomeController@store');
     });
     ```

   En ambos casos, el middleware `check.token` será ejecutado antes de que la solicitud llegue al controlador. Si el token no está presente, la solicitud será bloqueada y se enviará una respuesta de error.

### Middleware Global vs. Middleware por Ruta

- **Middleware Global**: Se aplica a todas las rutas de tu aplicación. Puedes registrar un middleware como global en la sección `$middleware` de `Kernel.php`.
- **Middleware por Ruta**: Se aplica a rutas o grupos de rutas específicas, como vimos anteriormente.

### Ejemplo Completo

Supongamos que quieres proteger varias rutas de tu API con el middleware de verificación de token:

1. Creas el middleware `CheckToken` para verificar la existencia de un token en la solicitud.
2. Registras el middleware en `Kernel.php`.
3. Aplicas el middleware a las rutas en `api.php`:

```php
Route::middleware('check.token')->group(function () {
    Route::get('/users', 'UserController@index');
    Route::post('/users', 'UserController@store');
    Route::get('/users/{id}', 'UserController@show');
});
```

Con esto, todas las rutas dentro del grupo estarán protegidas. Si una solicitud llega sin el parámetro `api_token`, Laravel devolverá una respuesta de error y no permitirá el acceso al controlador.

### Resumen

Los middlewares son una forma poderosa de mantener tu aplicación más segura y organizada, ya que separan la lógica de seguridad y validación de la lógica principal del negocio.
