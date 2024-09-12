---
sidebar_position: 3
title: Grupos
---

# Endpoints - Grupos

Un **grupo de endpoints** en Laravel es una forma de organizar y aplicar configuraciones comunes a varias endpoints al mismo tiempo. En lugar de repetir la misma configuración (como middleware, prefijos de URL, o namespaces) en cada ruta, puedes agruparlas para que compartan esos ajustes, lo que hace el código más limpio y fácil de mantener.

### ¿Por qué agrupar endpoints?

Imagina que tienes varias endpoints que requieren las mismas reglas, como autenticación o que todas comiencen con el mismo prefijo en la URL. Usar un **grupo de endpoints** te permite definir una vez esas reglas y aplicarlas a todas las endpoints dentro del grupo. Esto es útil especialmente en APIs, donde muchas endpoints pueden necesitar configuraciones similares.

### Cómo crear un grupo de endpoints

En el archivo `routes/api.php`, puedes definir un grupo de endpoints utilizando el método `Route::group()`. Este método acepta un array de configuraciones (como middleware o prefijos) y una función que contiene las endpoints que formarán parte del grupo.

### Ejemplo básico de grupo de endpoints

Supongamos que tienes varias endpoints para gestionar usuarios y deseas que todas esas endpoints compartan ciertas características, como protección por un middleware de autenticación y que todas las URLs empiecen con `/admin`.

```php
Route::middleware('auth:sanctum')->prefix('admin')->group(function () {
    Route::get('/users', 'UserController@index');   // URL: /api/admin/users
    Route::post('/users', 'UserController@store');  // URL: /api/admin/users
    Route::get('/users/{id}', 'UserController@show'); // URL: /api/admin/users/{id}
});
```

### Desglose del Ejemplo

- **Middleware `auth:sanctum`**: Esto asegura que todas las endpoints del grupo estén protegidas por el middleware de autenticación usando **Sanctum**. Si una solicitud no está autenticada, no podrá acceder a estas endpoints.
- **Prefijo `admin`**: Todas las endpoints dentro del grupo tendrán el prefijo `/admin` añadido a la URL. Por ejemplo, la ruta `/users` se convertirá en `/api/admin/users`.

- **endpoints dentro del grupo**:
  - `Route::get('/users', ...)`: Esta ruta responderá a `GET /api/admin/users`.
  - `Route::post('/users', ...)`: Esta ruta responderá a `POST /api/admin/users`.

### Aplicaciones Comunes de Grupos de endpoints

1. **Autenticación**: Proteger endpoints con middleware que solo permita el acceso a usuarios autenticados.

   ```php
   Route::middleware('auth:sanctum')->group(function () {
       Route::get('/profile', 'UserController@profile');
       Route::get('/settings', 'SettingsController@index');
   });
   ```

2. **Prefijos de URL**: Añadir un prefijo común a las endpoints. Esto es útil para organizar endpoints relacionadas bajo un mismo "nombre" en la URL.

   ```php
   Route::prefix('admin')->group(function () {
       Route::get('/dashboard', 'AdminController@dashboard');
       Route::get('/reports', 'AdminController@reports');
   });
   ```

3. **Configuraciones combinadas**: Usar tanto middleware como prefijos y otros ajustes en el mismo grupo.

   ```php
   Route::middleware('auth:sanctum')->prefix('admin')->group(function () {
       Route::get('/users', 'UserController@index');
       Route::post('/users', 'UserController@store');
   });
   ```

### Ventajas de Usar Grupos de endpoints

- **Reutilización de Configuraciones**: Si tienes varias endpoints que necesitan las mismas configuraciones (como un middleware o prefijo), los grupos te evitan tener que repetir código.
- **Mantenimiento Más Fácil**: Si necesitas cambiar una configuración (por ejemplo, añadir un middleware o cambiar un prefijo), solo tienes que hacerlo en el grupo, en lugar de actualizar cada ruta por separado.
- **Organización del Código**: Te permite agrupar endpoints lógicamente, como agrupar todas las endpoints que pertenecen a la administración del sistema bajo un mismo prefijo.

### Ejemplo Completo

Aquí un ejemplo más amplio de cómo se pueden combinar diferentes configuraciones en un grupo de endpoints:

```php
Route::middleware(['auth:sanctum', 'role:admin'])->prefix('admin')->group(function () {
    Route::get('/dashboard', 'AdminController@dashboard');  // URL: /api/admin/dashboard
    Route::get('/users', 'UserController@index');           // URL: /api/admin/users
    Route::post('/users', 'UserController@store');          // URL: /api/admin/users
    Route::get('/reports', 'ReportController@index');       // URL: /api/admin/reports
});
```

Aquí estamos:

- Protegiendo las endpoints con dos middlewares: `auth:sanctum` para autenticación y `role:admin` para verificar que el usuario es un administrador.
- Añadiendo el prefijo `admin` a todas las URLs dentro del grupo.

### Resumen

Un **grupo de endpoints** en Laravel es una forma eficiente de aplicar configuraciones comunes (como middleware, prefijos o nombres de endpoints) a varias endpoints en tu API. Esto hace el código más limpio, fácil de mantener y más organizado. Con los grupos de endpoints puedes aplicar autenticación, añadir prefijos a URLs o gestionar permisos de manera eficiente para un conjunto de endpoints relacionadas.
