---
sidebar_position: 1
title: Introducción
---

# Modelos - Introducción

En Laravel, un **modelo** es una clase que representa una tabla en la base de datos. Los modelos son parte del patrón de diseño **MVC** (Modelo-Vista-Controlador) y se utilizan para interactuar con los datos de la base de datos, proporcionando una forma fácil de realizar operaciones como crear, leer, actualizar y eliminar registros.

### Conceptos Clave

1. **Modelo**: Es una clase que se asocia a una tabla en la base de datos y proporciona una interfaz para interactuar con esa tabla.

2. **Eloquent ORM**: Laravel utiliza Eloquent, un **Object-Relational Mapper (ORM)**, para manejar la relación entre los modelos y las tablas de la base de datos. Eloquent simplifica las consultas y operaciones de base de datos al usar un enfoque orientado a objetos.

3. **Atributos del Modelo**: Representan las columnas de la tabla en la base de datos y se pueden acceder como propiedades del modelo.

4. **Métodos del Modelo**: Permiten realizar operaciones como guardar, actualizar, eliminar y consultar registros en la base de datos.

### Crear y Usar un Modelo

#### 1. Crear un Modelo

Puedes crear un modelo usando el comando Artisan de Laravel. Por ejemplo, para crear un modelo llamado `Product`:

```bash
php artisan make:model Product
```

Este comando generará un archivo `Product.php` en el directorio `app/Models`.

#### 2. Definir el Modelo

Abre el archivo `Product.php` y define el modelo. Aquí hay un ejemplo básico de un modelo `Product`:

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

    // Los atributos que se deben ocultar para los arreglos JSON
    protected $hidden = [
        'created_at', 'updated_at'
    ];
}
```

En este ejemplo:

- **`protected $table`**: Define la tabla en la base de datos con la que está asociado el modelo. Si omites esta propiedad, Eloquent asumirá que el nombre de la tabla es el plural del nombre del modelo (`products` para el modelo `Product`).

- **`protected $fillable`**: Define qué atributos se pueden asignar masivamente usando métodos como `create()` o `update()`. Esto ayuda a proteger tu aplicación de ataques de **mass assignment** (asignación masiva).

- **`protected $hidden`**: Define qué atributos deben ser ocultos en las respuestas JSON. Esto es útil para ocultar campos sensibles o innecesarios.

#### 3. Usar el Modelo

Puedes usar el modelo en tus controladores para interactuar con la base de datos. Aquí hay un ejemplo de cómo usar el modelo `Product` en un controlador para crear un nuevo producto y obtener una lista de productos:

```php
<?php

namespace App\Http\Controllers;

use App\Models\Product;
use Illuminate\Http\Request;

class ProductController extends Controller
{
    // Método para mostrar una lista de productos
    public function index()
    {
        // Obtiene todos los productos
        $products = Product::all();

        return response()->json($products);
    }

    // Método para crear un nuevo producto
    public function store(Request $request)
    {
        // Crea un nuevo producto con los datos del request
        $product = Product::create([
            'name' => $request->input('name'),
            'price' => $request->input('price'),
            'description' => $request->input('description')
        ]);

        return response()->json($product, 201);
    }
}
```

En este ejemplo:

- **`Product::all()`**: Obtiene todos los registros de la tabla `products`.
- **`Product::create()`**: Crea un nuevo registro en la tabla `products` con los datos proporcionados.

### Beneficios de Usar Modelos

1. **Simplicidad**: Los modelos proporcionan una manera simple y elegante de interactuar con la base de datos sin escribir consultas SQL directamente.

2. **Eloquent ORM**: Permite usar una sintaxis orientada a objetos para realizar operaciones de base de datos, simplificando las consultas y las relaciones entre tablas.

3. **Protección**: Los modelos pueden proteger contra ataques de asignación masiva y ocultar información sensible en respuestas JSON.

### Resumen

- **Modelo**: Clase que representa una tabla en la base de datos y proporciona métodos para interactuar con esa tabla.
- **Eloquent ORM**: Herramienta de Laravel para manejar modelos y bases de datos de forma orientada a objetos.
- **Crear y Usar Modelos**: Usa Artisan para crear modelos, define los atributos y métodos necesarios, y utiliza el modelo en tus controladores para manejar datos de la base de datos.

Los modelos en Laravel hacen que trabajar con bases de datos sea más fácil y organizado, permitiéndote concentrarte en la lógica de tu aplicación sin preocuparte por detalles de SQL.
