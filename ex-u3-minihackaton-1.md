### Mini Hackatón: Desarrollo de un Sistema Web con Autenticación y Roles

**Duración**: 1 hora con 40 minutos  
**Equipos**: 4 personas por equipo  
**Recursos Permitidos**:  
- Tus prácticas previas.  
- Consultas en internet.  
- Apoyo entre los integrantes del equipo.  

---

### **Objetivo**
Desarrollar un sitio web básico con autenticación de usuarios y gestión de roles. El sitio debe cumplir con los requisitos mínimos establecidos en esta guía y utilizar tecnologías como Node.js, Express, MySQL y hashing de contraseñas.

---

### **Requerimientos del Proyecto**

1. **Estructura de Archivos**  
   Organiza tu proyecto con la siguiente estructura básica:  
   ```
   /mi-hackaton
       ├── server.js
       ├── package.json
       ├── node_modules
       ├── /public
       │       ├── index.html
       │       ├── login.html
       │       ├── registro.html
       │       ├── navbar.html
       │       ├── styles.css
   ```

2. **Base de Datos**  
   - Crea una base de datos llamada `hackaton_db` con las tablas necesarias:  
     - **usuarios**: (id, nombre, correo, contraseña_hash, tipo_usuario)  
     - **datos_biomedicos**: (id, nombre_dispositivo, descripcion, estado, usuario_id).  

   - Inserta al menos un usuario administrador para probar el sistema.  

   **Pista**: Usa las estructuras de las tablas vistas en las prácticas.

---

### **Puntos Clave del Proyecto**

#### 1. **Autenticación de Usuarios**  
- Implementa las rutas para **registro**, **login** y **logout**.  
- Las contraseñas deben almacenarse en la base de datos con hashing.  

**Pista**: Consulta cómo usamos `bcrypt` en las prácticas anteriores.  

---

#### 2. **Roles de Usuarios**  
- Implementa al menos dos roles:  
  - **Admin**: Puede ver, editar y eliminar todos los datos en la base de datos.  
  - **Médico**: Puede ver y editar únicamente los datos que le pertenecen.  

**Pista**: Usa la función `requireRole` que implementamos en las prácticas para proteger las rutas.

---

#### 3. **Gestión de Datos Biomédicos**  
- Administra los dispositivos biomédicos desde el sistema:  
  - **Admin**:  
    - Puede agregar nuevos dispositivos, editar cualquier registro y eliminar dispositivos.  
  - **Médico**:  
    - Solo puede ver y editar los dispositivos asignados a su usuario.  

**Pista**: Recuerda cómo protegimos las rutas para diferentes roles y cómo manejamos formularios para insertar y actualizar datos.

---

#### 4. **Interfaz de Usuario**  
- **Barra de Navegación**:  
  Incluye un menú dinámico que muestre opciones según el rol del usuario:  
  - **Admin**:  
    - Ver Usuarios  
    - Gestionar Dispositivos  
    - Cerrar Sesión  
  - **Médico**:  
    - Ver Dispositivos  
    - Editar Dispositivos  
    - Cerrar Sesión  

**Pista**: Si no estás seguro cómo implementar esto, consulta el uso de `fetch` desde `navbar.html`.

- **Estilos**:  
  Aplica estilos básicos utilizando `styles.css`.  

---

#### 5. **Rutas Dinámicas**  
- La ruta `/datos-biomédicos` debe responder dinámicamente según el tipo de usuario:  
  - **Admin**: Puede ver todos los dispositivos biomédicos.  
  - **Médico**: Solo puede ver sus dispositivos.  

**Pista**: Revisa cómo hicimos consultas condicionales en las rutas protegidas.  

---

### **Criterios de Evaluación**
| Criterio                             | Puntos |
|--------------------------------------|--------|
| Estructura de archivos               | 10     |
| Implementación de autenticación      | 20     |
| Uso de roles y protección de rutas   | 20     |
| Gestión de datos biomédicos          | 20     |
| Barra de navegación dinámica         | 15     |
| Estilo y organización del código     | 10     |
| Funcionamiento general               | 5      |
| **Total**                            | **100**|

---

### **Entrega**  
1. Crea un documento con portada, que incluya los integrantes del equipo y el título de la actividad (Examen U3 - Mini hackatón: ...).  
2. En el documento, explicar brevemente el funcionamiento y capuras del funcionamiento de cada etapa descrita en la tabla de los criterios de evaluación. Y los roles y las actividadres que realizaron los diferentes miembros del equipo.  
3. Subir el documento en formato PDF en el enlace del examen.  
