### Mini Hackatón: **Desarrollo de un Sistema Web para Gestión de Equipos de Imagen Médica**

**Duración**: 1 hora con 40 minutos  
**Equipos**: 4 personas por equipo  
**Recursos Permitidos**:  
- Tus prácticas previas.  
- Consultas en internet.  
- Apoyo entre los integrantes del mismo equipo.  

---

### **Objetivo**
Desarrollar un sistema web básico para gestionar equipos de **imagen médica** (por ejemplo, ultrasonidos, resonancias magnéticas, tomógrafos) con autenticación y roles. El sistema debe cumplir los requisitos establecidos en esta guía y utilizar tecnologías como Node.js, Express, MySQL y hashing de contraseñas.

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
   - Crea una base de datos llamada `equipos_imagen` con las tablas necesarias:  
     - **usuarios**: (id, nombre, correo, contraseña_hash, tipo_usuario).  
     - **equipos**: (id, nombre_equipo, tipo_equipo, descripcion, estado, usuario_id).  

   - Inserta al menos un usuario administrador para probar el sistema.  

   **Pista**: Usa las estructuras de las tablas vistas en las prácticas.

---

### **Puntos Clave del Proyecto**

#### 1. **Autenticación de Usuarios**  
- Implementa las rutas para **registro**, **login** y **logout**.  
- Las contraseñas deben almacenarse en la base de datos con hashing.  

**Pista**: Usa `bcrypt` como hicimos en las prácticas previas.

---

#### 2. **Roles de Usuarios**  
- Implementa al menos tres roles:  
  - **Admin**: Puede ver, editar y eliminar todos los datos en la base de datos.  
  - **Técnico**: Puede ver y editar los datos asignados a su usuario.  
  - **Auditor**: Solo puede ver los datos.  

**Pista**: Adapta la función `requireRole` para manejar múltiples roles con acceso a las mismas rutas.

---

#### 3. **Gestión de Equipos de Imagen Médica**  
- Administra los equipos desde el sistema:  
  - **Admin**:  
    - Puede agregar nuevos equipos, editar cualquier registro y eliminar equipos.  
  - **Técnico**:  
    - Puede ver y editar solo los equipos asignados a su usuario.  
  - **Auditor**:  
    - Solo puede ver los equipos, sin editar ni eliminar.  

**Pista**: Usa consultas condicionales para diferenciar los datos visibles según el rol.

---

#### 4. **Interfaz de Usuario**  
- **Barra de Navegación**:  
  Incluye un menú dinámico que muestre opciones según el rol del usuario:  
  - **Admin**:  
    - Ver Usuarios  
    - Gestionar Equipos  
    - Cerrar Sesión  
  - **Técnico**:  
    - Ver Equipos  
    - Editar Equipos  
    - Cerrar Sesión  
  - **Auditor**:  
    - Ver Equipos  
    - Cerrar Sesión  

**Pista**: Consulta cómo usamos `fetch` desde `navbar.html` para mostrar opciones dinámicas.

- **Estilos**:  
  Aplica estilos básicos utilizando `styles.css`.

---

#### 5. **Rutas Dinámicas**  
- La ruta `/equipos` debe responder dinámicamente según el tipo de usuario:  
  - **Admin**: Puede ver todos los equipos.  
  - **Técnico**: Solo puede ver sus equipos.  
  - **Auditor**: Puede ver todos los equipos, pero no puede editarlos.  

**Pista**: Usa la lógica de roles implementada en las prácticas previas.

---

### **Entrega**
1. Crea un documento con portada, que incluya:  
   - Los integrantes del equipo.  
   - El título de la actividad: **Examen U3 - Mini Hackatón: Sistema para Gestión de Equipos de Imagen Médica**.  

2. En el documento, explica brevemente el funcionamiento de cada parte del sistema (autenticación, roles, gestión de datos) y agrega capturas del funcionamiento de cada etapa.  

3. Describe las actividades realizadas por cada integrante del equipo.  

4. Convierte el documento a formato PDF y súbelo al enlace del examen.  

---

### **Criterios de Evaluación**
| Criterio                             | Puntos |
|--------------------------------------|--------|
| Estructura de archivos               | 10     |
| Implementación de autenticación      | 20     |
| Uso de roles y protección de rutas   | 20     |
| Gestión de equipos de imagen médica  | 20     |
| Barra de navegación dinámica         | 15     |
| Estilo y organización del código     | 10     |
| Funcionamiento general               | 5      |
| **Total**                            | **100**|
