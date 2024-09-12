---
sidebar_position: 3
title: Validators
---

# Controladores - Validators

Usar el **validator** en un controlador de Laravel es una forma de asegurarte de que los datos enviados a tu aplicación cumplen con ciertos requisitos antes de procesarlos. El **validator** permite definir reglas de validación para los datos y manejar errores si los datos no cumplen con esas reglas.

### Pasos para Usar el Validator en un Controlador

1. **Importar el Validator**:

   Primero, necesitas importar la clase `Validator` en tu controlador. Asegúrate de agregar esta línea al inicio de tu archivo de controlador:

   ```php
   use Illuminate\Support\Facades\Validator;
   ```

2. **Definir las Reglas de Validación**:

   En tu método del controlador, puedes usar el `Validator::make()` para definir las reglas de validación para los datos de la solicitud. Las reglas indican qué campos son requeridos y qué tipo de datos deben contener.

3. **Ejecutar la Validación y Manejar Errores**:

   Después de definir las reglas, ejecuta la validación y verifica si falla. Si la validación falla, puedes retornar una respuesta con los errores.

### Ejemplo Detallado

Supongamos que tienes una API que recibe datos para crear una solicitud y deseas validar estos datos. Aquí está cómo podrías hacerlo:

**1. Crear el Controlador**

Para este ejemplo, supongamos que ya tienes un controlador llamado `SolicitudController`. Aquí está cómo agregar la validación:

```php
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Illuminate\Support\Facades\Validator;

class SolicitudController extends Controller
{
    // Método para manejar la creación de una solicitud
    public function store(Request $request)
    {
        // Definir las reglas de validación
        $validator = Validator::make($request->all(), [
            'idSolicitud' => 'required|integer',
            'mp' => 'required|string',
            'unidad' => 'required|string',
            'municipio' => 'required|string',
            'documento' => 'required|mimes:pdf,doc,docx|max:2048',
        ]);

        // Verificar si la validación falló
        if ($validator->fails()) {
            // Crear una respuesta con los errores de validación
            $response = [
                "message" => "Error al validar la información",
                "errors" => $validator->errors(),
                "code" => 400
            ];
            return response()->json($response, 400);
        }

        // Aquí puedes continuar con la lógica si la validación fue exitosa
        // Por ejemplo, guardar los datos en la base de datos

        return response()->json([
            "message" => "Solicitud creada exitosamente",
            "code" => 201
        ], 201);
    }
}
```

**2. Definir las Reglas de Validación**

En el ejemplo anterior, las reglas de validación son:

- `'idSolicitud' => 'required|integer'`: El campo `idSolicitud` es obligatorio y debe ser un número entero.
- `'mp' => 'required|string'`: El campo `mp` es obligatorio y debe ser una cadena de texto.
- `'unidad' => 'required|string'`: El campo `unidad` es obligatorio y debe ser una cadena de texto.
- `'municipio' => 'required|string'`: El campo `municipio` es obligatorio y debe ser una cadena de texto.
- `'documento' => 'required|mimes:pdf,doc,docx|max:2048'`: El campo `documento` es obligatorio, debe ser un archivo con una de las extensiones especificadas (`pdf`, `doc`, `docx`) y no debe superar los 2048 kilobytes (2 MB).

**3. Manejar Errores de Validación**

Si la validación falla, `Validator::make()` devuelve un objeto `Validator` que contiene los errores. Puedes usar el método `fails()` para verificar si hubo errores y el método `errors()` para obtener una lista de errores. Luego, puedes retornar estos errores en una respuesta JSON, junto con un mensaje y un código de estado HTTP (en este caso, 400 para indicar una solicitud incorrecta).

**4. Continuar con la Lógica**

Si la validación es exitosa, puedes proceder a manejar los datos (por ejemplo, guardarlos en la base de datos) y retornar una respuesta indicando que la solicitud se ha creado correctamente.

### Resumen

- **Importar `Validator`**: Asegúrate de importar la clase en tu controlador.
- **Definir Reglas**: Usa `Validator::make()` para definir qué datos son necesarios y qué reglas deben cumplir.
- **Ejecutar y Verificar**: Usa el método `fails()` para comprobar si hubo errores y el método `errors()` para obtener los detalles.
- **Manejar Errores**: Retorna una respuesta con los errores y un código de estado apropiado si la validación falla.
- **Continuar con la Lógica**: Procede a manejar los datos si la validación es exitosa.

Este enfoque asegura que los datos recibidos en tu API son válidos y te permite manejar cualquier problema antes de procesar los datos.
