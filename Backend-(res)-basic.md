// Importar Express
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
app.get('/api/text', (req, res) => {
    res.send("¡Hola, esta es una respuesta de texto!");
});

// 2. res.json() - Enviar respuesta en formato JSON
app.get('/api/json', (req, res) => {
    res.json({ message: "Respuesta en JSON", status: "success" });
});

// 3. res.status() - Usar status con send() para indicar estado
app.get('/api/notfound', (req, res) => {
    res.status(404).send("Recurso no encontrado");
});

// 4. res.sendFile() - Enviar un archivo al cliente
app.get('/api/file', (req, res) => {
    const filePath = path.join(__dirname, 'public', 'example.pdf'); // Ruta del archivo
    res.sendFile(filePath);
});

// 5. res.redirect() - Redirigir a otra ruta
app.get('/api/old-route', (req, res) => {
    res.redirect('/api/new-route');
});

app.get('/api/new-route', (req, res) => {
    res.send("Has sido redirigido a la nueva ruta");
});

// 6. res.end() - Terminar la respuesta sin contenido
app.get('/api/end', (req, res) => {
    res.status(204).end(); // Código 204 indica "Sin Contenido"
});

// 7. res.set() o res.header() - Establecer encabezado personalizado
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
