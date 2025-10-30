## 🛠️ Cómo ejecutar un test de conexión constante (Ping Continuo)

Este procedimiento se utiliza para diagnosticar la **estabilidad de tu conexión a Internet** a lo largo del tiempo, enviando peticiones constantes a un servidor de Google muy fiable (8.8.8.8).

---

### Pasos para Iniciar la Prueba

1.  **Abrir el diálogo Ejecutar:** Pulsa la combinación de teclas **`Windows` + `R`**. Aparecerá una pequeña ventana titulada "Ejecutar".
2.  **Abrir el Símbolo del Sistema (Consola):** Escribe `cmd` (sin comillas) en el campo y haz clic en **Aceptar**. Se abrirá una ventana de fondo negro o azul oscuro.
3.  **Iniciar el Ping Constante:** En la consola, escribe exactamente el siguiente comando, **respetando los espacios**, y pulsa la tecla **`Enter`**:
    ```
    ping 8.8.8.8 -t
    ```
    * El argumento **`-t`** es crucial, ya que indica a Windows que debe repetir el comando *indefinidamente*.
4.  **Verificación Inicial:** Comprueba que la consola empieza a mostrar mensajes de **"Respuesta desde 8.8.8.8: bytes=32 tiempo=Xms TTL=Y"**. Esto confirma que la prueba está funcionando.

---

### Ejecución y Estadísticas

* **Ejecución Prolongada:** Deja la ventana de la consola abierta para que el test se ejecute durante el período deseado, por ejemplo **una noche**.
* **Estadísticas Parciales (Sin detener):** Para ver el resumen de estadísticas **en cualquier momento sin parar la prueba**, pulsa la combinación de teclas **`CTRL` + `Pausa`**. La prueba continuará inmediatamente.
* **Detener la Prueba:** Para **detener el proceso** y ver el **informe final**, pulsa la combinación de teclas **`CTRL` + `C`**.

---

### 📊 Interpretación de los Resultados

Al detener la prueba (o al pulsar `CTRL` + `Pausa`), verás un resumen de las estadísticas.

| Situación | Significado |
| :--- | :--- |
| **Paquetes Perdidos > 0** | **Hay un problema de red.** Indica inestabilidad, lo que puede causar cortes o fallos intermitentes. **Cuanto mayor sea el porcentaje, más grave es.** |
| **Paquetes Perdidos = 0** | **No hubo problemas de conexión durante el período del test.** Solo confirma que la red fue estable *mientras duró la prueba*. |

> **Nota Adicional:** Revisa también el **tiempo máximo** (`Máximo = Xms`). Valores muy altos (`> 100ms`) o **variaciones bruscas** en el tiempo de respuesta sugieren **alta latencia o *jitter*** (variabilidad), que afecta a las aplicaciones en tiempo real (como las videollamadas).


### Notas

La denominación de las teclas puede cambiar según el teclado.

- La tecla **WINDOWS** es la que tiene el logotipo de Windows, **una bandera**. Suele haber dos iguales en el teclado.
- La tecla **ENTER** puede aparece como **ENTRAR** o como **INTRO**.
- La tecla **PAUSA** puede aparecer como **PAUSE** o como **BREAK**.
