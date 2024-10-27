# Código de Backend en Express.js con Explicación de Métodos de Respuesta

Este código muestra cómo usar diferentes métodos de `res` en Express.js para enviar respuestas desde un backend a los clientes. Cada método tiene un propósito específico y se utiliza en una ruta particular para ilustrar su uso.

```javascript
// Importar Express y otros módulos necesarios
const express = require('express');
const path = require('path');
const app = express();
const PORT = 3000;

// Middleware para parsear JSON en el cuerpo de las solicitudes
app.use(express.json());

/* ========================
   Rutas con diferentes métodos de res
   ======================== */

// 1. res.send() - Enviar respuesta en texto
// Esta ruta envía una respuesta de texto simple, útil para devolver mensajes de texto o HTML.
app.get('/api/text', (req, res) => {
    res.send("¡Hola, esta es una respuesta de texto!");
});

// 2. res.json() - Enviar respuesta en formato JSON
// Aquí usamos res.json() para enviar un objeto en formato JSON.
// Express establece automáticamente el Content-Type en 'application/json'.
app.get('/api/json', (req, res) => {
    res.json({ message: "Respuesta en JSON", status: "success" });
});

// 3. res.status() - Usar status con send() para indicar estado
// Usamos res.status() para establecer un código de estado específico y luego send() para enviar un mensaje.
// En este caso, devolvemos un error 404 (no encontrado).
app.get('/api/notfound', (req, res) => {
    res.status(404).send("Recurso no encontrado");
});

// 4. res.sendFile() - Enviar un archivo al cliente
// res.sendFile() permite enviar un archivo al cliente, en este caso un PDF almacenado en la carpeta 'public'.
app.get('/api/file', (req, res) => {
    const filePath = path.join(__dirname, 'public', 'example.pdf'); // Ruta del archivo
    res.sendFile(filePath);
});

// 5. res.redirect() - Redirigir a otra ruta
// Esta ruta redirige al cliente a otra URL. Cuando el cliente accede a '/api/old-route',
// se redirige automáticamente a '/api/new-route'.
app.get('/api/old-route', (req, res) => {
    res.redirect('/api/new-route');
});

// Ruta de destino de la redirección
app.get('/api/new-route', (req, res) => {
    res.send("Has sido redirigido a la nueva ruta");
});

// 6. res.end() - Terminar la respuesta sin contenido
// res.end() finaliza la respuesta sin enviar contenido. Aquí usamos el código de estado 204, que indica "Sin Contenido".
app.get('/api/end', (req, res) => {
    res.status(204).end(); // Código 204 indica "Sin Contenido"
});

// 7. res.set() o res.header() - Establecer encabezado personalizado
// Con res.set() (o res.header()), podemos añadir encabezados personalizados a la respuesta.
// En este ejemplo, añadimos un encabezado llamado 'X-Custom-Header' antes de enviar la respuesta.
app.get('/api/custom-header', (req, res) => {
    res.set('X-Custom-Header', 'ValorEjemplo'); // Agrega un encabezado personalizado
    res.send("Encabezado personalizado agregado");
});

/* ========================
   Iniciar el servidor
   ======================== */
app.listen(PORT, () => {
    console.log(`Servidor ejecutándose en http://localhost:${PORT}`);
});
