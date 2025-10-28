### Examen Práctico: Gestión de Insumos Médicos

#### Ejercicio 1: Configuración Inicial del Proyecto

1. **Crea la estructura de archivos:**
   - Crea una carpeta para tu proyecto llamada `gestion-insumos`.
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

     app.use(bodyParser.urlencoded({ extended: true }));
     app.use(express.static('public'));

     app.listen(PORT, () => {
       console.log(`Servidor corriendo en http://localhost:${PORT}`);
     });
     ```

4. **Muestra que el servidor esté corriendo correctamente mostrando la terminal con el mensaje de confirmación: `Servidor corriendo en http://localhost:3000`.**

#### Ejercicio 2: Creación del Formulario en HTML para Registrar Insumos

1. **Crea el formulario:**
   - En el archivo `public/index.html`, escribe el siguiente código HTML para crear un formulario con los campos necesarios para registrar un insumo médico:
     ```html
     <!DOCTYPE html>
     <html lang="es">
     <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Gestión de Insumos Médicos</title>
       <link rel="stylesheet" href="styles.css">
     </head>
     <body>
       <h1>Registrar Insumo Médico</h1>
       <form action="/add-insumo" method="POST">
         <label for="nombre">Nombre del insumo:</label>
         <input type="text" id="nombre" name="nombre" required><br><br>

         <label for="cantidad">Cantidad:</label>
         <input type="number" id="cantidad" name="cantidad" required><br><br>

         <label for="proveedor">Proveedor:</label>
         <input type="text" id="proveedor" name="proveedor" required><br><br>

         <label for="fecha">Fecha de adquisición:</label>
         <input type="date" id="fecha" name="fecha" required><br><br>

         <button type="submit" class="btn-submit">Registrar Insumo</button>
       </form>
     </body>
     </html>
     ```

2. **Estiliza la página**: Crea un archivo `public/styles.css`, agrega los siguientes estilos:
   ```css
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
    
    form {
      max-width: 400px;
      margin: 0 auto;
      padding: 20px;
      background-color: #fff;
      border: 1px solid #ddd;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    }
    
    label {
      font-weight: bold;
      display: block;
      margin-bottom: 10px;
    }
    
    input {
      width: calc(100% - 20px);
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

3. **Muestra la página en ejecución**

#### Ejercicio 3: Inserción de Datos en la Base de Datos

1. **Conexión a la base de datos**: Crea una base de datos y asegúrate de tener una tabla llamada `insumos` con las columnas `nombre`, `cantidad`, `proveedor`, y `fecha_adquisicion`.

2. **Configura la conexión a MySQL en `server.js`:**
   - Agrega el siguiente código en el archivo `server.js`:
     ```javascript
     const db = mysql.createConnection({
       host: 'localhost',
       user: 'tu_usuario',
       password: 'tu_contraseña',
       database: 'gestion_insumos'
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
   ```javascript
     app.post('/add-insumo', (req, res) => {
       const { nombre, cantidad, proveedor, fecha } = req.body;
       const query = 'INSERT INTO insumos (nombre, cantidad, proveedor, fecha_adquisicion) VALUES (?, ?, ?, ?)';

       db.query(query, [nombre, cantidad, proveedor, fecha], (err, result) => {
         if (err) {
           console.error('Error insertando datos:', err);
           return res.status(500).send('Error al registrar el insumo');
         }
         res.send('Insumo registrado con éxito');
       });
     });
   ```

4. **Inserta los datos en la base de datos desde la página (al menos cinco insumos) y muestra todos los registros hechos desde la terminal con MySQL.**

#### Ejercicio 4: Consulta y Muestra de Insumos Registrados

1. **Consulta los insumos registrados**: Añade una ruta GET en el archivo `server.js` para consultar todos los insumos:
   ```javascript
      app.get('/insumos', (req, res) => {
        const query = 'SELECT * FROM insumos';
        db.query(query, (err, results) => {
          if (err) {
            console.error('Error consultando datos:', err);
            return res.status(500).send('Error al consultar insumos');
          }
      
          let html = `
          <!DOCTYPE html>
          <html lang="es">
          <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <title>Insumos Registrados</title>
            <link rel="stylesheet" href="styles.css">
          </head>
          <body>
            <h1>Insumos Médicos Registrados</h1>
            <table border="1">
              <thead>
                <tr>
                  <th>Nombre</th>
                  <th>Cantidad</th>
                  <th>Proveedor</th>
                  <th>Fecha de Adquisición</th>
                </tr>
              </thead>
              <tbody>`;
      
          results.forEach(insumo => {
            html += `
              <tr>
                <td>${insumo.nombre}</td>
                <td>${insumo.cantidad}</td>
                <td>${insumo.proveedor}</td>
                <td>${insumo.fecha_adquisicion}</td>
              </tr>`;
          });
      
          html += `</tbody></table><br><a href="/">Volver</a></body></html>`;
          res.send(html);
        });
    });
   ```

2. **Agregar estilos para la tabla y realizar una consulta desde la página para mostrar todos los insumos.**

#### Ejercicio 5: Buscar Insumos Médicos

1. **Agregar los bloques necesarios para que se puedan buscar insumos médicos en la Base de Datos y mostrar los resultados en una tabla.**

#### Ejercicio 6: Ordenar Insumos Médicos por Fecha

1. **Agregar los bloques necesarios para que la interfaz muestre los insumos ordenados por fecha de adquisición.**

#### Ejercicio 7: Mostrar Mensajes de Confirmación

1. **Agregar los bloques necesarios para que la interfaz muestre un mensaje estilizado cuando un insumo ha sido registrado con éxito o haya algún error en el registro.**

#### Ejercicio 8: Agrega al sistema la característica que desees utilizando llaves foráneas (relación entre tablas).
