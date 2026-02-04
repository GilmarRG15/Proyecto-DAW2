# TIENDA DEPORTIVA - Sistema MVC PHP Puro

Proyecto universitario de tienda web de artículos deportivos desarrollado con arquitectura MVC en PHP puro, sin frameworks.

## Características

- Arquitectura MVC estricta
- PHP Puro (sin frameworks)
- PDO con Prepared Statements
- Sistema de autenticación con `password_hash()` y `password_verify()`
- Gestión de sesiones con `$_SESSION`
- Sistema de roles (ADMIN, GESTOR_PRODUCTOS, GESTOR_INVENTARIO, DESPACHADOR, CLIENTE)
- CRUDs completos con validaciones
- Carrito de compras
- Gestión de pedidos y despachos
- CSS propio responsive (sin Bootstrap)
- Middleware de protección por roles
- Sistema de routing propio

## Instalación en XAMPP

### 1. Requisitos Previos

- XAMPP instalado (PHP 7.4+ y MySQL/MariaDB)
- Navegador web moderno

### 2. Configuración de la Base de Datos

1. Inicia XAMPP y arranca **Apache** y **MySQL**
2. Abre phpMyAdmin en tu navegador: `http://localhost/phpmyadmin`
3. Importa el archivo `database.sql`:
   - Click en "Importar" en el menú superior
   - Click en "Seleccionar archivo" y elige `database.sql`
   - Click en "Continuar" en la parte inferior
4. La base de datos `bd_tienda` se creará automáticamente con todos los datos de prueba

### 3. Configuración del Proyecto

1. Copia la carpeta `tienda` a la carpeta `htdocs` de XAMPP:
   ```
   C:\xampp\htdocs\tienda
   ```

2. Verifica la configuración de base de datos en:
   ```
   tienda/app/config/database.php
   ```
   
   Por defecto está configurado para XAMPP:
   ```php
   private $host = 'localhost';
   private $dbname = 'bd_tienda';
   private $username = 'root';
   private $password = ''; // Vacío en XAMPP por defecto
   ```

### 4. Acceso al Sistema

Abre tu navegador y ve a:
```
http://localhost/tienda/public
```

##  Usuarios de Prueba

Todos los usuarios tienen la contraseña: **password123**

| Email | Rol | Descripción |
|-------|-----|-------------|
| admin@tienda.com | ADMIN | Acceso completo al sistema |
| gestor.productos@tienda.com | GESTOR_PRODUCTOS | Gestión de productos y categorías |
| gestor.inventario@tienda.com | GESTOR_INVENTARIO | Gestión de variantes e inventario |
| despachador@tienda.com | DESPACHADOR | Gestión de despachos y entregas |
| cliente1@email.com | CLIENTE | Usuario cliente normal |
| cliente2@email.com | CLIENTE | Usuario cliente normal |

## Estructura del Proyecto

```
tienda/
 app/
    config/
       config.php          # Configuración general
       database.php        # Configuración PDO
    core/
       Router.php          # Sistema de rutas
       Controller.php      # Controlador base
       Model.php           # Modelo base
       Auth.php            # Sistema de autenticación
       Middleware.php      # Middleware de roles
    controllers/
       HomeController.php
       AuthController.php
       CatalogoController.php
       CarritoController.php
       PedidosController.php
       AdminController.php
    models/
       Usuario.php
       Rol.php
       Categoria.php
       Producto.php
       VarianteProducto.php
       ImagenProducto.php
       Pedido.php
       DetallePedido.php
       DespachoPedido.php
    views/
        layouts/
           header.php
           footer.php
           navbar.php
        home/
        auth/
        catalogo/
        carrito/
        pedidos/
        admin/
 public/
    index.php               # Front Controller
    assets/
        css/
           styles.css
        img/
 database.sql                # Script SQL
 README.md                   # Este archivo
```

##  Seguridad Implementada

- Passwords hasheados con `password_hash(PASSWORD_DEFAULT)`
- Prepared Statements en todas las consultas
- Protección CSRF con tokens
- Validación de inputs en servidor
- Control de sesiones seguro
- Middleware de autenticación y roles
- Escape de salida HTML con `htmlspecialchars()`

##  Funcionalidades Principales

### Usuario Cliente
- Registro y login
- Navegación del catálogo por categorías
- Búsqueda de productos
- Visualización de detalles de productos
- Agregar productos al carrito
- Gestión del carrito (actualizar cantidades, eliminar)
- Proceso de checkout
- Visualización de pedidos realizados

### Gestor de Productos
- CRUD completo de categorías
- CRUD completo de productos
- Gestión de imágenes de productos
- Activar/desactivar productos

### Gestor de Inventario
- CRUD de variantes (talla, color, precio, stock)
- Control de stock
- Gestión de SKUs

### Despachador
- Visualización de pedidos pendientes
- Asignación de pedidos
- Actualización de estados de envío
- Registro de despachos y entregas

### Administrador
- Acceso a todas las funcionalidades
- CRUD de usuarios
- Asignación de roles
- Bloqueo/desbloqueo de usuarios
- Visualización de estadísticas

## Base de Datos

El sistema utiliza 9 tablas principales:

1. **roles** - Roles del sistema
2. **usuarios** - Información de usuarios
3. **usuario_roles** - Relación usuarios-roles (muchos a muchos)
4. **categorias** - Categorías de productos
5. **productos** - Información general de productos
6. **variantes_producto** - Variantes con stock y precio
7. **imagenes_producto** - URLs de imágenes
8. **pedidos** - Pedidos realizados
9. **detalle_pedido** - Items del pedido (snapshot)
10. **despacho_pedido** - Información de envío

##  Diseño

El sistema utiliza CSS propio sin frameworks, con:
- Variables CSS para colores y estilos
- Layout responsive con Flexbox y Grid
- Diseño limpio y profesional
- Navegación intuitiva
- Formularios bien estilizados

##  Personalización

### Cambiar configuración de Base de Datos

Edita `app/config/database.php`:
```php
private $host = 'localhost';      // Tu host
private $dbname = 'bd_tienda';    // Nombre de BD
private $username = 'root';        // Usuario
private $password = '';            // Contraseña
```

### Cambiar URL base

Edita `app/config/config.php`:
```php
define('APP_URL', 'http://localhost/tienda/public');
```

##  Solución de Problemas

### Error de conexión a base de datos
- Verifica que MySQL esté corriendo en XAMPP
- Comprueba las credenciales en `database.php`
- Asegúrate de haber importado el archivo SQL

### Página en blanco o error 500
- Activa la visualización de errores en `config.php`:
  ```php
  define('APP_ENV', 'development');
  ```
- Revisa los logs de Apache en `xampp/apache/logs/error.log`

### Las rutas no funcionan
- Asegúrate de acceder a través de `http://localhost/tienda/public`
- Si usas .htaccess, verifica que `mod_rewrite` esté habilitado

## Notas Técnicas

- **Autoload**: El sistema usa `spl_autoload_register()` para cargar clases automáticamente
- **Router**: Sistema de routing propio con soporte para parámetros dinámicos
- **MVC**: Separación estricta: Models (BD), Views (HTML), Controllers (lógica)
- **Helpers**: Funciones auxiliares en `config.php` (url, asset, flash, csrf, etc.)

## ‍ Desarrollo

Este es un proyecto universitario que demuestra:
- Arquitectura MVC desde cero
- Buenas prácticas de seguridad
- Manejo de sesiones y autenticación
- Sistema de roles y permisos
- Gestión de base de datos con PDO
- Desarrollo sin dependencias externas

##  Licencia

Proyecto educativo - Libre uso para fines académicos

---

Desarrollado con  para aprendizaje de PHP MVC
