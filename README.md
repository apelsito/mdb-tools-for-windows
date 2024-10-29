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

## Proceso de Realización de la Compilación

Este proyecto ha sido compilado y adaptado para sistemas Windows de 64 bits utilizando herramientas y librerías de código abierto. A continuación se describen los pasos, herramientas y librerías necesarias para reproducir esta compilación.

### Herramientas Utilizadas

1. **MSYS2**: Una plataforma de desarrollo de código abierto que proporciona una interfaz de terminal y un entorno de desarrollo compatible con herramientas de Unix para Windows.
2. **MinGW-w64**: Una versión de GCC (GNU Compiler Collection) adaptada para compilar aplicaciones en Windows. Específicamente, utilizamos el compilador de 64 bits.
3. **Automake y Autoconf**: Herramientas de GNU que ayudan en el proceso de configuración y construcción del software.
4. **Libtool**: Herramienta utilizada para crear bibliotecas compartidas de una manera portátil.
5. **Librerías de MSYS2**: Incluyendo `libgcc`, `libiconv`, `libreadline`, `libtermcap`, y `libwinpthread` para asegurar la compatibilidad en Windows.

### Librerías y Dependencias

La compilación incluyó varias librerías de MSYS2 que son necesarias para que **mdbtools** funcione en un sistema Windows de 64 bits:

- **libgcc**: Biblioteca de GCC para operaciones matemáticas avanzadas.
- **libiconv**: Biblioteca para la conversión de encodings de texto.
- **libreadline**: Biblioteca que permite la edición de líneas de texto.
- **libtermcap**: Para el control de terminal.
- **libwinpthread**: Biblioteca de hilos compatible con Windows.

### Proceso de Compilación

1. **Clonar el Repositorio de mdbtools**: Se comenzó clonando el repositorio oficial de **mdbtools** desde GitHub.
   
   ```bash
   git clone https://github.com/mdbtools/mdbtools.git
    ```

2. **Configuración del Entorno:** Usando MSYS2, se instalaron las dependencias necesarias:
   ```bash
    pacman -S base-devel mingw-w64-x86_64-toolchain autoconf automake libtool
    ```
3. **Configurar y Compilar:** Desde la terminal de MSYS2 MinGW64 (debes abrir la terminal MinGW64), se ejecutaron los siguientes comandos para configurar y compilar:
   ```bash
    autoreconf -i -f
    ```
    ```bash
    ./configure --host=x86_64-w64-mingw32
    ```
    ```bash
    make install
    ```

4. **Soluciones a Errores de Compatibilidad**: Durante la compilación, surgieron errores relacionados con el uso de tipos de datos y declaraciones de funciones específicos de Windows, como `locale_t` y `spawnv`. Estos problemas se resolvieron realizando los siguientes ajustes:

   - **Cambio de `locale_t` a `_locale_t`**:
     En el archivo de cabecera `mdbtools.h`, había una declaración que usaba `locale_t`, un tipo no reconocido en entornos Windows bajo MSYS2/MinGW. La línea original era:
     ```c
     typedef locale_t mdb_locale_t;
     ```
     Para que funcionara en Windows, cambiamos esta línea a:
     ```c
     typedef _locale_t mdb_locale_t;
     ```

   - **Modificación de la Declaración de `spawnv`**:
     Otro error ocurrió en el archivo `process.h`, relacionado con la declaración de `spawnv`, que causaba un conflicto de tipos. La línea original en `process.h` era:
     ```c
     _CRTIMP intptr_t __cdecl spawnv(int, const char *_Filename, char *const _ArgList[]) __MINGW_ATTRIB_DEPRECATED_MSVC2005;
     ```
     Para resolver el conflicto, ajustamos la declaración cambiando `char *const _ArgList[]` a `const char *const _ArgList[]` para que coincida con el tipo esperado. La línea modificada quedó así:
     ```c
     _CRTIMP intptr_t __cdecl spawnv(int, const char *_Filename, const char *const _ArgList[]) __MINGW_ATTRIB_DEPRECATED_MSVC2005;
     ```

Estos ajustes en el código permitieron que la compilación continuara sin conflictos de tipos, asegurando la compatibilidad con el entorno de Windows.


5. **Organización de los Archivos Compilados**: Una vez compilado, se recopilaron los archivos ejecutables (`.exe`) y las bibliotecas necesarias (`.dll`) en una carpeta independiente (`C:\mdb-tools`) para facilitar su uso sin dependencias externas.

Este proceso documentado permite reproducir la compilación de **mdbtools** para cualquier entorno de Windows en 64 bits.


## Créditos

Este repositorio es una compilación de **mdbtools** adaptada para sistemas Windows, permitiendo el uso de herramientas de manipulación de bases de datos Access en entornos de línea de comandos sin necesidad de software adicional. 

Para más detalles y documentación completa sobre **mdbtools**, puedes visitar el repositorio original en [mdbtools](https://github.com/mdbtools/mdbtools).
