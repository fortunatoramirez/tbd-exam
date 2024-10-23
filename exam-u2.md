Aquí tienes el examen actualizado con los cambios solicitados, incluyendo los colores para los botones y los nuevos requerimientos para que los alumnos te muestren los equipos registrados desde la terminal. Además, he agregado el paso final en cada ejercicio que indica lo que los estudiantes deben mostrar.

---

### Examen Práctico: Proyecto de Gestión de Equipos Biomédicos

#### Ejercicio 1: Configuración Inicial del Proyecto

1. **Crea la estructura de archivos:**
   - Crea una carpeta para tu proyecto llamada `gestion-equipos`.
   - Dentro de esta carpeta, crea un archivo llamado `server.js`.
   - Crea otra carpeta llamada `public` y dentro de ella, un archivo llamado `index.html`.

2. **Instala las dependencias necesarias:**
   - Abre una terminal en la carpeta del proyecto y ejecuta los siguientes comandos para instalar las dependencias:
     ```bash
     npm init -y
     npm install express mysql2 body-parser
     ```

3. **Crea el servidor básico:**
   - En el archivo `server.js`, escribe el siguiente código para configurar un servidor básico utilizando Express:
     ```javascript
     const express = require('express');
     const bodyParser = require('body-parser');
     const mysql = require('mysql2');

     const app = express();
     const PORT = 3000;

     // Configurar body-parser para manejar datos del formulario
     app.use(bodyParser.urlencoded({ extended: true }));
     app.use(express.static('public')); // Servir archivos estáticos desde la carpeta 'public'

     // Servidor escuchando en el puerto 3000
     app.listen(PORT, () => {
       console.log(`Servidor corriendo en http://localhost:${PORT}`);
     });
     ```

4. **Muestra que el servidor esté corriendo correctamente mostrando la terminal con el mensaje de confirmación: `Servidor corriendo en http://localhost:3000`.**

#### Ejercicio 2: Creación del Formulario en HTML para Registrar Equipos

1. **Crea el formulario:**
   - En el archivo `public/index.html`, escribe el siguiente código HTML para crear un formulario con los campos necesarios para registrar un equipo biomédico:
     ```html
     <!DOCTYPE html>
     <html lang="es">
     <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Gestión de Equipos Biomédicos</title>
       <link rel="stylesheet" href="styles.css">
     </head>
     <body>
       <h1>Registrar Equipo Biomédico</h1>
       <form action="/add-equipo" method="POST">
         <label for="nombre">Nombre del equipo:</label>
         <input type="text" id="nombre" name="nombre" required><br><br>

         <label for="modelo">Modelo:</label>
         <input type="text" id="modelo" name="modelo" required><br><br>

         <label for="serie">Número de serie:</label>
         <input type="text" id="serie" name="serie" required><br><br>

         <label for="fecha">Fecha de adquisición:</label>
         <input type="date" id="fecha" name="fecha" required><br><br>

         <button type="submit" class="btn-submit">Registrar Equipo</button>
       </form>
     </body>
     </html>
     ```

2. **Estiliza la página**: Crea un archivo `public/styles.css`, agrega los siguientes estilos para el botón:
   ```css
   /* Estilos para el cuerpo de la página */
    body {
      font-family: Arial, sans-serif;
      background-color: #f4f4f9;
      margin: 0;
      padding: 20px;
      text-align: center;
    }
    
    h1 {
      color: #333;
      margin-bottom: 20px;
    }
    
    /* Estilos del formulario */
    form {
      max-width: 400px;
      margin: 0 auto 20px auto;
      padding: 20px;
      background-color: #fff;
      border: 1px solid #ddd;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    
    label {
      font-weight: bold;
      display: block;
      margin-bottom: 10px; /* Añadimos margen inferior para separar las etiquetas del input */
    }
    
    input {
      width: calc(100% - 20px); /* Ajusta el tamaño del input para que tenga espacio en los márgenes */
      padding: 10px;
      margin-bottom: 15px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    
    button {
      width: 100%;
      padding: 10px;
      background-color: #007bff;
      color: white;
      font-weight: bold;
      border: none;
      cursor: pointer;
      border-radius: 4px;
    }
    
    button:hover {
      background-color: #0067d4;
    }
   ```

3. **Mouestra la página en ejecución**

#### Ejercicio 3: Inserción de Datos en la Base de Datos

1. **Conexión a la base de datos**: Crea una Base de Datos y Asegúrate de tener una tabla llamada `equipos` con las columnas `nombre`, `modelo`, `numero_serie`, y `fecha_adquisicion`.

2. **Configura la conexión a MySQL en `server.js`:**
   - A continuación, agrega el código para conectarte a la base de datos en el archivo `server.js`:
     ```javascript
     const db = mysql.createConnection({
       host: 'localhost',
       user: 'tu_usuario',
       password: 'tu_contraseña',
       database: 'gestion_equipos'
     });

     db.connect((err) => {
       if (err) {
         console.error('Error conectando a la base de datos:', err);
         return;
       }
       console.log('Conectado a la base de datos MySQL');
     });
     ```

3. **Configura la ruta POST para procesar el formulario:**
   - En el mismo archivo `server.js`, añade una ruta POST que procese el formulario y guarde los datos en la base de datos:
     ```javascript
     app.post('/add-equipo', (req, res) => {
       const { nombre, modelo, serie, fecha } = req.body;
       const query = 'INSERT INTO equipos (nombre, modelo, numero_serie, fecha_adquisicion) VALUES (?, ?, ?, ?)';

       db.query(query, [nombre, modelo, serie, fecha], (err, result) => {
         if (err) {
           console.error('Error insertando datos:', err);
           return res.status(500).send('Error al registrar el equipo');
         }
         res.send('Equipo registrado con éxito');
       });
     });
     ```

4. **Inserta los datos en la base de datos desde la página (al menos cinco) y muéstra todos los registros hechos directamente desde la terminal con MySQL.**

#### Ejercicio 4: Consulta y Muestra de Equipos Registrados desde la página

1. **Consulta los equipos**: En el archivo `server.js`, añade una ruta GET para consultar todos los equipos biomédicos registrados en la base de datos:
   ```javascript
      app.get('/equipos', (req, res) => {
        const query = 'SELECT * FROM equipos';
        db.query(query, (err, results) => {
          if (err) {
            console.error('Error consultando datos:', err);
            return res.status(500).send('Error al consultar equipos');
          }
      
          // Crear el HTML con la tabla de resultados
          let html = `
          <!DOCTYPE html>
          <html lang="es">
          <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Equipos Registrados</title>
            <link rel="stylesheet" href="styles.css">
          </head>
          <body>
            <h1>Equipos Biomédicos Registrados</h1>
            <table border="1">
              <thead>
                <tr>
                  <th>Nombre</th>
                  <th>Modelo</th>
                  <th>Número de Serie</th>
                  <th>Fecha de Adquisición</th>
                </tr>
              </thead>
              <tbody>`;
      
          // Agregar cada equipo a la tabla
          results.forEach(equipo => {
            html += `
              <tr>
                <td>${equipo.nombre}</td>
                <td>${equipo.modelo}</td>
                <td>${equipo.numero_serie}</td>
                <td>${equipo.fecha_adquisicion}</td>
              </tr>`;
          });
      
          // Cerrar el HTML
          html += `
              </tbody>
            </table>
            <br>
            <a href="/">Volver al registro de equipos</a>
          </body>
          </html>`;
      
          // Enviar el HTML completo como respuesta
          res.send(html);
        });
    });

   ```

2. **Realiza una consulta de todos los equipos registrados y muéstra los resultados desde la página web.**

#### Ejercicio 5: Notificación de Éxito o Error al Insertar Datos

