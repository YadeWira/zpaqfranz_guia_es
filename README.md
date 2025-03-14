# README: zpaqfranz

**zpaqfranz** es una herramienta avanzada de compresión, descompresión, backup y gestión de archivos basada en el formato ZPAQ. Es una versión mejorada y extendida de `zpaq`, con soporte para múltiples funcionalidades adicionales, como la deduplicación, verificación de integridad, gestión de multipartes, y más. Este documento proporciona una guía detallada sobre cómo utilizar **zpaqfranz** y sus diversas características.

---

## Tabla de Contenidos

1. [Introducción](#introducción)
2. [Instalación](#instalación)
3. [Uso Básico](#uso-básico)
4. [Comandos Principales](#comandos-principales)
   - [a (add)](#a-add)
   - [x (extract)](#x-extract)
   - [l (list)](#l-list)
   - [t (test)](#t-test)
   - [v (verify)](#v-verify)
   - [d (deduplicate)](#d-deduplicate)
   - [c (compare)](#c-compare)
   - [b (benchmark)](#b-benchmark)
   - [g (Windows C: backup)](#g-windows-c-backup)
   - [q (Windows C: backup avanzado)](#q-windows-c-backup-avanzado)
   - [m (merge)](#m-merge)
   - [testbackup](#testbackup)
5. [Opciones Avanzadas](#opciones-avanzadas)
6. [Ejemplos de Uso](#ejemplos-de-uso)
7. [Consideraciones de Seguridad](#consideraciones-de-seguridad)
8. [Soporte y Actualizaciones](#soporte-y-actualizaciones)
9. [Créditos](#créditos)
10. [Redes Sociales](#redes-sociales)

---

## Introducción

**zpaqfranz** es una herramienta de línea de comandos diseñada para la compresión, descompresión, backup y gestión de archivos. Es especialmente útil para:

- **Backups incrementales**: Permite realizar backups incrementales y deduplicados, lo que ahorra espacio y tiempo.
- **Verificación de integridad**: Incluye funciones avanzadas para verificar la integridad de los archivos comprimidos.
- **Multipartes**: Soporta la creación y gestión de archivos multipartes, útiles para backups grandes.
- **Deduplicación**: Elimina archivos duplicados para optimizar el almacenamiento.
- **Compatibilidad con múltiples sistemas de archivos**: Soporta NTFS, ZFS, y otros sistemas de archivos.

---

## Instalación

### Instalación desde el código fuente

1. Clona el repositorio de **zpaqfranz**:
   ```bash
   git clone https://github.com/fcorbelli/zpaqfranz.git
   cd zpaqfranz
   ```
2. Compila el proyecto:
   ```bash
   make
   ```
3. Haz que el ejecutable sea accesible globalmente:
   ```bash
   chmod 755 zpaqfranz
   sudo cp zpaqfranz /bin
   ```

### Instalación en distribuciones basadas en Debian/Ubuntu

#### Para sistemas x64:
```bash
wget https://ftp.debian.org/debian/pool/main/z/zpaqfranz/zpaqfranz_60.10-1_amd64.deb
sudo dpkg -i zpaqfranz_60.10-1_amd64.deb
```

#### Para sistemas x86:
```bash
wget https://ftp.debian.org/debian/pool/main/z/zpaqfranz/zpaqfranz_60.10-1_i386.deb
sudo dpkg -i zpaqfranz_60.10-1_i386.deb
```

---

## Uso Básico

El uso básico de **zpaqfranz** sigue la siguiente estructura:

```bash
zpaqfranz comando archivo.zpaq archivos_o_directorios -opciones
```

- **comando**: El comando a ejecutar (por ejemplo, `a` para añadir archivos, `x` para extraer, etc.).
- **archivo.zpaq**: El archivo ZPAQ que se va a crear o manipular.
- **archivos_o_directorios**: Los archivos o directorios que se van a comprimir, extraer, o manipular.
- **opciones**: Opciones adicionales para personalizar el comportamiento del comando.

---

## Comandos Principales

### a (add)

Añade archivos o directorios a un archivo ZPAQ.

**Sintaxis**:
```bash
zpaqfranz a archivo.zpaq archivos_o_directorios -opciones
```

**Opciones comunes**:
- `-key X`: Establece una contraseña para el archivo.
- `-mN`: Establece el nivel de compresión (0 más rápido, 5 mejor compresión).
- `-test`: Realiza una verificación después de añadir los archivos.
- `-comment "texto"`: Añade un comentario a la versión.
- `-chunk X`: Divide el archivo en partes de tamaño `X` (por ejemplo, `500M` para partes de 500 MB).

**Ejemplo**:
```bash
zpaqfranz a backup.zpaq /ruta/al/directorio -key mi_contraseña -m5 -test
```

### x (extract)

Extrae archivos de un archivo ZPAQ.

**Sintaxis**:
```bash
zpaqfranz x archivo.zpaq -to directorio_destino -opciones
```

**Opciones comunes**:
- `-all`: Extrae todas las versiones de los archivos.
- `-force`: Sobrescribe archivos existentes.
- `-utf`: Convierte nombres de archivos no UTF-8 a UTF-8.

**Ejemplo**:
```bash
zpaqfranz x backup.zpaq -to /ruta/de/destino -force
```

### l (list)

Lista el contenido de un archivo ZPAQ.

**Sintaxis**:
```bash
zpaqfranz l archivo.zpaq -opciones
```

**Opciones comunes**:
- `-checksum`: Muestra los checksums de los archivos.
- `-summary`: Muestra un resumen compacto.
- `-find "texto"`: Busca archivos que coincidan con el texto.

**Ejemplo**:
```bash
zpaqfranz l backup.zpaq -checksum -summary
```

### t (test)

Verifica la integridad de un archivo ZPAQ.

**Sintaxis**:
```bash
zpaqfranz t archivo.zpaq -opciones
```

**Opciones comunes**:
- `-verify`: Realiza una verificación completa contra el sistema de archivos.
- `-paranoid`: Realiza una verificación más exhaustiva.

**Ejemplo**:
```bash
zpaqfranz t backup.zpaq -verify
```

### v (verify)

Verifica los archivos en el sistema de archivos contra el archivo ZPAQ.

**Sintaxis**:
```bash
zpaqfranz v archivo.zpaq -opciones
```

**Opciones comunes**:
- `-ssd`: Usa múltiples hilos para la verificación (recomendado para SSDs).
- `-find "texto"`: Busca archivos que coincidan con el texto.

**Ejemplo**:
```bash
zpaqfranz v backup.zpaq -ssd
```

### d (deduplicate)

Elimina archivos duplicados en un directorio.

**Sintaxis**:
```bash
zpaqfranz d directorio -opciones
```

**Opciones comunes**:
- `-kill`: Realiza la eliminación (por defecto es una simulación).
- `-force`: Fuerza la eliminación sin preguntar.

**Ejemplo**:
```bash
zpaqfranz d /ruta/al/directorio -kill
```

### c (compare)

Compara dos directorios o archivos.

**Sintaxis**:
```bash
zpaqfranz c directorio1 directorio2 -opciones
```

**Opciones comunes**:
- `-checksum`: Compara los checksums de los archivos.
- `-ssd`: Usa múltiples hilos para la comparación.

**Ejemplo**:
```bash
zpaqfranz c /ruta/directorio1 /ruta/directorio2 -checksum
```

### b (benchmark)

Realiza pruebas de rendimiento de los algoritmos de hash.

**Sintaxis**:
```bash
zpaqfranz b -opciones
```

**Opciones comunes**:
- `-n X`: Establece el tiempo límite en segundos.
- `-minsize Y`: Establece el tamaño mínimo de los datos a probar.

**Ejemplo**:
```bash
zpaqfranz b -n 10 -minsize 1048576
```

### g (Windows C: backup)

Realiza un backup del disco C: en Windows.

**Sintaxis**:
```bash
zpaqfranz g archivo.zpaq -opciones
```

**Opciones comunes**:
- `-all`: Incluye todos los archivos, excepto el archivo de intercambio.
- `-frugal`: Excluye carpetas como `Windows` y `Program Files`.

**Ejemplo**:
```bash
zpaqfranz g backup.zpaq -all
```

### q (Windows C: backup avanzado)

Similar al comando `g`, pero con más opciones avanzadas.

**Sintaxis**:
```bash
zpaqfranz q archivo.zpaq -opciones
```

**Opciones comunes**:
- `-forcewindows`: Incluye la carpeta `Windows`.
- `-to directorio`: Especifica la carpeta de destino para el backup.

**Ejemplo**:
```bash
zpaqfranz q backup.zpaq -forcewindows -to C:\backup
```

### m (merge)

Consolida archivos multipartes en un solo archivo ZPAQ.

**Sintaxis**:
```bash
zpaqfranz m "archivo_???.zpaq" archivo_completo.zpaq -opciones
```

**Opciones comunes**:
- `-verify`: Verifica la integridad de las partes antes de consolidar.

**Ejemplo**:
```bash
zpaqfranz m "backup_???.zpaq" backup_completo.zpaq -verify
```

### testbackup

Verifica la integridad de un archivo multiparte.

**Sintaxis**:
```bash
zpaqfranz testbackup "archivo_???.zpaq" -opciones
```

**Opciones comunes**:
- `-verify`: Realiza una verificación completa de las partes.
- `-ssd`: Usa múltiples hilos para la verificación (recomendado para SSDs).

**Ejemplo**:
```bash
zpaqfranz testbackup "backup_???.zpaq" -verify
```

---

## Opciones Avanzadas

**zpaqfranz** incluye una amplia gama de opciones avanzadas para personalizar el comportamiento de los comandos. Algunas de las más destacadas incluyen:

- **Hashing**: Soporta múltiples algoritmos de hash como SHA-1, SHA-256, BLAKE3, XXH3, etc.
- **Multithreading**: Optimiza el rendimiento en sistemas con múltiples núcleos.
- **Gestión de multipartes**: Permite la creación y gestión de archivos multipartes para backups grandes.
- **Verificación de integridad**: Incluye opciones para verificar la integridad de los archivos comprimidos y extraídos.

---

## Ejemplos de Uso

1. **Crear un backup comprimido**:
   ```bash
   zpaqfranz a backup.zpaq /ruta/al/directorio -key mi_contraseña -m5 -test
   ```

2. **Crear un backup multiparte**:
   ```bash
   zpaqfranz a "backup_???.zpaq" /ruta/al/directorio -key mi_contraseña -m5 -chunk 500M
   ```

3. **Extraer un backup**:
   ```bash
   zpaqfranz x backup.zpaq -to /ruta/de/destino -force
   ```

4. **Verificar la integridad de un backup**:
   ```bash
   zpaqfranz t backup.zpaq -verify
   ```

5. **Eliminar archivos duplicados**:
   ```bash
   zpaqfranz d /ruta/al/directorio -kill
   ```

6. **Comparar dos directorios**:
   ```bash
   zpaqfranz c /ruta/directorio1 /ruta/directorio2 -checksum
   ```

7. **Consolidar archivos multipartes**:
   ```bash
   zpaqfranz m "backup_???.zpaq" backup_completo.zpaq -verify
   ```

8. **Verificar un backup multiparte**:
   ```bash
   zpaqfranz testbackup "backup_???.zpaq" -verify
   ```

---

## Consideraciones de Seguridad

- **Contraseñas**: Utiliza siempre contraseñas seguras para proteger tus archivos comprimidos.
- **Verificación**: Siempre verifica la integridad de los archivos después de comprimirlos o extraerlos.
- **Backups**: Realiza backups regulares y almacénalos en ubicaciones seguras.

---

## Soporte y Actualizaciones

- **Soporte**: Para obtener soporte, visita el [repositorio oficial de zpaqfranz](https://github.com/fcorbelli/zpaqfranz) o consulta la documentación oficial.
- **Actualizaciones**: Para actualizar **zpaqfranz**, utiliza el comando `zpaqfranz update -force`.

---

## Créditos

**zpaqfranz** fue creado por **fcorbelli**. Puedes encontrar más información sobre el proyecto en su repositorio oficial:  
[https://github.com/fcorbelli/zpaqfranz](https://github.com/fcorbelli/zpaqfranz)

---

## Redes Sociales

Mis redes sociales:

- **GitHub**: [https://github.com/YadeWira](https://github.com/YadeWira)
- **SoundCloud**: [https://soundcloud.com/sy-wira](https://soundcloud.com/sy-wira)
- **YouTube**: [https://www.youtube.com/@LinuxPana](https://www.youtube.com/@LinuxPana)

---

**zpaqfranz** es una herramienta poderosa y versátil para la gestión de backups y archivos. Con esta guía, deberías estar listo para aprovechar al máximo sus funcionalidades. ¡Buena suerte!

