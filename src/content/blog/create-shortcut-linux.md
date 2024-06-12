---
title: "agregar una acceso directo a la seccion de Apps en Zorin OS"
description: "Guía para crear un acceso directo de Android Studio en Zorin OS"
pubDate: "May 14 2024"
heroImage: "/blog-placeholder-2.jpg"
---

### Paso 1: Crear el archivo de acceso directo

1. Abre tu editor de texto favorito.
2. Copia y pega el siguiente contenido:

```plaintext
[Desktop Entry]
Type=Application
Categories=Development
Name=Android Studio
Icon=/home/fmarcosdev/development/android-studio/bin/studio.png
Exec=/home/fmarcosdev/development/android-studio/bin/studio.sh
Terminal=false
StartupNotify=true
Comment=Develop Android Apps
```

Paso 2: Dar permisos al archivo
Abre una terminal.
Ejecuta el siguiente comando para dar permisos de ejecución al archivo que acabas de crear:

chmod +x /ruta/donde/guardaste/android-studio.desktop

Paso 3: Mover el archivo al directorio correcto
Abre una terminal.
Ejecuta el siguiente comando para mover el archivo android-studio.desktop al directorio de aplicaciones:

sudo mv /ruta/donde/guardaste/android-studio.desktop /usr/share/applications/

Paso 4: Verificar y usar el acceso directo
Ve al menú de aplicaciones de Zorin OS.
Busca "Android Studio" en la lista de aplicaciones. Deberías ver el icono de Android Studio y su nombre.
Haz clic en él para iniciar Android Studio.

¡Eso es todo! Ahora deberías tener un acceso directo funcional para Android Studio en tu sistema Zorin OS. ¡Puedes compartir esta guía en tu blog para ayudar a otros usuarios! Si tienes alguna pregunta, no dudes en preguntar.
