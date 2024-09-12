---
sidebar_position: 2
title: Eloquent
---

# Modelos - Eloquent

**Eloquent** es el **Object-Relational Mapper (ORM)** que Laravel utiliza para interactuar con bases de datos. Eloquent facilita el manejo de datos al proporcionar una interfaz orientada a objetos para realizar operaciones comunes de base de datos como crear, leer, actualizar y eliminar registros.

### ¿Qué es Eloquent?

Eloquent es una herramienta que permite interactuar con bases de datos utilizando objetos en lugar de escribir consultas SQL directamente. Esto hace que trabajar con datos sea más intuitivo y fácil de mantener, ya que puedes usar la misma lógica orientada a objetos que utilizas en otras partes de tu aplicación.

### Cómo Usar Eloquent

#### 1. Crear un Modelo

Primero, necesitas un modelo en Eloquent. Un modelo representa una tabla en la base de datos y proporciona métodos para interactuar con ella.

**Crear un Modelo**

Para crear un modelo, usa el comando Artisan de Laravel. Por ejemplo, para crear un modelo llamado `Product`:

```bash
php artisan make:model Product
```

Esto generará un archivo `Product.php` en el directorio `app/Models`.

#### 2. Definir el Modelo

En el archivo `Product.php`, define el modelo. Aquí está un ejemplo básico:

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Product extends Model
{
    // La tabla asociada al modelo
    protected $table = 'products';

    // Los atributos que se pueden asignar de manera masiva
    protected $fillable = [
        'name', 'price', 'description'
    ];
}
```

#### 3. Realizar Operaciones Básicas

Con Eloquent, puedes realizar varias operaciones comunes de base de datos de manera sencilla:

**Crear un Registro**

Puedes crear un nuevo registro en la tabla usando el método `create`:

```php
use App\Models\Product;

$product = Product::create([
    'name' => 'New Product',
    'price' => 100,
    'description' => 'This is a new product.'
]);
```

**Leer Registros**

Para obtener registros, puedes usar métodos como `all`, `find`, y `where`:

```php
// Obtener todos los registros
$products = Product::all();

// Obtener un registro por su clave primaria
$product = Product::find(1);

// Obtener registros con una condición
$expensiveProducts = Product::where('price', '>', 100)->get();
```

**Actualizar un Registro**

Para actualizar un registro, primero obtén el registro y luego modifica sus atributos:

```php
$product = Product::find(1);
$product->price = 150;
$product->save();
```

**Eliminar un Registro**

Para eliminar un registro, puedes usar el método `delete`:

```php
$product = Product::find(1);
$product->delete();
```

#### 4. Relaciones entre Modelos

Eloquent facilita el manejo de relaciones entre modelos. Por ejemplo, si tienes dos modelos relacionados como `Post` y `Comment`, puedes definir estas relaciones en los modelos.

**Ejemplo: Relación Uno a Muchos**

En el modelo `Post`:

```php
public function comments()
{
    return $this->hasMany(Comment::class);
}
```

En el modelo `Comment`:

```php
public function post()
{
    return $this->belongsTo(Post::class);
}
```

**Usar Relaciones**

```php
// Obtener los comentarios de un post
$post = Post::find(1);
$comments = $post->comments;

// Obtener el post de un comentario
$comment = Comment::find(1);
$post = $comment->post;
```

### Resumen

- **Modelo**: Representa una tabla en la base de datos. Define cómo interactuar con la tabla usando Eloquent.
- **Operaciones Básicas**: Usa métodos como `create`, `all`, `find`, `where`, `update`, y `delete` para manejar datos.
- **Relaciones**: Define y usa relaciones entre modelos para manejar datos relacionados de manera sencilla.

Eloquent simplifica el manejo de datos al permitirte trabajar con objetos en lugar de escribir consultas SQL manualmente, haciendo que el código sea más limpio y fácil de mantener.
