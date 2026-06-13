audio y video a nuevo video:

```sh
ffmpeg -i "GMT20250425-221958_Recording_1686x768.mp4" -i "GMT20250425-221958_Recording_1686x768.m4a" -map 0:v -map 1:a -c:v copy -c:a aac -shortest "combined_output.mp4"
```
```sh
ffmpeg -i "videoplayback.mp4" -i "videoplayback.m4a" -map 0:v -map 1:a -c:v copy -c:a aac -shortest "combined_output.mp4"
```


### Explicación del comando:
*   **`-i "GMT202...mp4"`**: Primer archivo de entrada (el que proporciona el video).
*   **`-i "GMT202...m4a"`**: Segundo archivo de entrada (el que proporciona el audio).
*   **`-map 0:v`**: Toma el flujo de video del primer archivo (el `.mp4`).
*   **`-map 1:a`**: Toma el flujo de audio del segundo archivo (el `.m4a`).
*   **`-c:v copy`**: Copia el video sin codificarlo (es mucho más rápido).
*   **`-c:a aac`**: Codifica el audio en formato AAC (estándar para archivos `.mp4`).
*   **`-shortest`**: Hace que el video resultante tenga la duración del video más corto (evita que queden segundos extra si el audio es más largo que el video).
*   **`"combined_output.mp4"`**: El nombre del archivo de salida.

Esto generará un archivo llamado `combined_output.mp4` en la misma carpeta con el video del primero y el audio del segundo.

