# mdb-tools-for-windows

Este repositorio contiene una versión compilada de **mdbtools** para Windows, permitiendo la manipulación de bases de datos de Microsoft Access (`.mdb`) directamente desde la línea de comandos en sistemas Windows.

## ¿Qué es mdbtools?

**mdbtools** es un conjunto de herramientas de línea de comandos diseñado para interactuar con bases de datos de Microsoft Access. La herramienta principal en este conjunto, **mdb-tables**, permite listar las tablas contenidas en un archivo `.mdb`.

### Herramientas incluidas

- `mdb-tables`: Lista las tablas disponibles en una base de datos `.mdb`.
- `mdb-export`: Exporta el contenido de una tabla a formato CSV.
- `mdb-schema`: Muestra el esquema de la base de datos.
- `mdb-sql`: Ejecuta consultas SQL en la base de datos.

## Configuración en Windows

### Paso 1: Mover la Carpeta a `C:\`

Descarga este repositorio y mueve la carpeta `mdbtools-app` a la raíz de tu disco **C:**, de modo que la ruta completa sea `C:\mdbtools-app`.

### Paso 2: Agregar la Carpeta al PATH de Windows

Agregar `C:\mdbtools-app` al **PATH** del sistema permitirá ejecutar los comandos de **mdbtools** desde cualquier ubicación en la terminal.

1. Abre **Configuración avanzada del sistema** (puedes buscarlo desde el menú de inicio).
2. Haz clic en **Variables de entorno**.
3. En las **Variables de sistema**, selecciona `Path` y haz clic en **Editar**.
4. Agrega `C:\mdbtools-app` como una nueva entrada.
5. Guarda los cambios y cierra la ventana.

### Paso 3: Verificar la Configuración

1. Abre una nueva ventana de **Símbolo del sistema** (cmd).
2. Ejecuta el siguiente comando para verificar la instalación:

   ```cmd
   mdb-tables --help
    ```

Si el comando muestra la ayuda de `mdb-tables`, la instalación fue exitosa.

## Ejemplos de Uso

Una vez configurado, puedes utilizar los comandos de **mdbtools** directamente en el cmd. Aquí algunos ejemplos:

- **Listar tablas en un archivo `.mdb`**:

   ```cmd
   mdb-tables "C:\ruta\al\archivo.mdb"
    ```
- **Exportar una tabla a CSV**:
   ```cmd
   mdb-export "C:\ruta\al\archivo.mdb" nombre_tabla > salida.csv
    ```
- **Ver el esquema de la base de datos:**:
   ```cmd
   mdb-schema "C:\ruta\al\archivo.mdb"
    ```
Para más información sobre cada comando, puedes ejecutar `[comando] --help`, como en el ejemplo:

```cmd
mdb-export --help
```
Esto mostrará las opciones y detalles de uso específicos de cada herramienta en **mdbtools**.

## Créditos

Este repositorio es una compilación de **mdbtools** adaptada para sistemas Windows, permitiendo el uso de herramientas de manipulación de bases de datos Access en entornos de línea de comandos sin necesidad de software adicional. 

Para más detalles y documentación completa sobre **mdbtools**, puedes visitar el repositorio original en [mdbtools](https://github.com/mdbtools/mdbtools).
