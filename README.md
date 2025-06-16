# Generador de Códigos Secuenciales Natura

Una aplicación Node.js con React frontend para generar códigos secuenciales para productos Natura, con dos bases de datos MySQL separadas.

## Características

- **Frontend React**: Interfaz moderna con el diseño de Natura
- **Backend Node.js**: API REST con Express
- **Dos bases de datos MySQL**:
  - `natura_products`: Para consultar códigos, nombres y descripción de productos
  - `natura_records`: Para almacenar registros de actividad
- **Funcionalidades**:
  - Generación de códigos secuenciales basados en año/mes/producto
  - Timer para trackear tiempo activo
  - Registro de actividades
  - Exportar datos a CSV
  - Opciones: Cambio de Orden, Generar Secuencial, Cambio de Batch

## Configuración

### 1. Instalar dependencias
```bash
npm install
```

### 2. Configurar bases de datos MySQL

**Opción A: Usar el script SQL**
```bash
mysql -u root -p < database/setup.sql
```

**Opción B: Configurar manualmente**
1. Crear las dos bases de datos:
   - `natura_products`
   - `natura_records`
2. Ejecutar las consultas SQL del archivo `database/setup.sql`

### 3. Configurar variables de entorno

Copiar `.env.example` a `.env` y configurar las credenciales de MySQL:

```bash
cp .env.example .env
```

Editar `.env`:
```env
# Base de datos para productos (consulta)
PRODUCTS_DB_HOST=localhost
PRODUCTS_DB_USER=tu_usuario
PRODUCTS_DB_PASSWORD=tu_password
PRODUCTS_DB_NAME=natura_products

# Base de datos para registros (escritura)
RECORDS_DB_HOST=localhost
RECORDS_DB_USER=tu_usuario
RECORDS_DB_PASSWORD=tu_password
RECORDS_DB_NAME=natura_records

# Puerto del servidor
PORT=3001
```

### 4. Ejecutar la aplicación

**Desarrollo (frontend y backend simultáneamente):**
```bash
npm run dev
```

**Solo backend:**
```bash
npm run server
```

**Solo frontend:**
```bash
npm run client
```

## Estructura del proyecto

```
├── server/
│   └── index.js          # Servidor Express con API
├── src/
│   ├── App.tsx           # Componente principal React
│   ├── main.tsx          # Punto de entrada React
│   └── index.css         # Estilos con Tailwind
├── database/
│   └── setup.sql         # Script de configuración de BD
├── .env.example          # Plantilla de variables de entorno
└── README.md
```

## API Endpoints

- `GET /api/products/:code` - Buscar producto por código
- `GET /api/sequential/:month/:productCode` - Obtener siguiente número secuencial
- `POST /api/activities` - Guardar nueva actividad
- `GET /api/activities` - Obtener todas las actividades
- `DELETE /api/activities/:id` - Eliminar actividad específica
- `DELETE /api/activities` - Limpiar todas las actividades

## Productos de ejemplo incluidos

La aplicación incluye 10 productos de ejemplo (P001-P010) con sus respectivos números de serie, nombres y descripciones de la línea Natura.

## Notas técnicas

- El frontend se ejecuta en puerto 3000 (Vite)
- El backend se ejecuta en puerto 3001 (Express)
- Las bases de datos se inicializan automáticamente al arrancar el servidor
- Los códigos secuenciales siguen el formato: `L.[AñoLetra][Mes]XM[Secuencial]`
- El año se mapea a letras (2025=A, 2026=B, etc.)
- Los meses se mapean a números/letras (1-9, A=Oct, B=Nov, C=Dic)