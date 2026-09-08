# Prompts — TP 1

El registro del proceso, en orden. Una sección por prompt, agregada en el commit de la versión que ese prompt produjo. La guía con los tres prompts pensados de antemano está en `prompts_guia.md`, que no se versiona.

---

## 1 — Prompt inicial · v1

```
Construí un selector de fecha y país de nacimiento donde cada valor se
elige de a uno, sin lista desplegable y sin buscador.

Estructura:
- <header> con el título "Datos de nacimiento" y una línea de progreso:
  "0 de 4 datos confirmados".
- <main> con cuatro ruedas en fila —día, mes, año y país—. Cada rueda es
  una ventanita que muestra UNA sola opción a la vez, con un <button> "▲"
  arriba, un <button> "▼" abajo y un <button> "Confirmar" al costado.
  Debajo de la opción visible, un contador "opción 12 de 195".
- <footer> con los cuatro valores confirmados hasta ahora y un <button>
  "Continuar", deshabilitado hasta que las cuatro ruedas estén
  confirmadas.

Estilo:
- Estética de trámite web viejo: fondo gris, bordes duros de 2px,
  tipografía monoespaciada, cero redondeo. Las ventanitas, hundidas.
- Los botones ▲/▼ ocupan poco y están pegados a la ventanita.
- Ninguna rueda muestra más de una opción: nada de degradé arriba y abajo
  insinuando que hay más. La única señal de que hay más es el contador.

Comportamiento:
- Estado: ruedas (array de 4 objetos con nombre, opciones, indice y
  confirmado), activa (índice de la rueda con foco, o null) y bloqueado
  (booleano que frena todo avance durante el enfriamiento entre pasos).
- Opciones: día 1 a 31, mes 1 a 12, año 1920 a 2010, país la lista de
  países en orden alfabético.
- Al click en "▼" de una rueda: esa rueda pasa a ser la activa; si
  bloqueado es false, indice avanza una posición y bloqueado se pone en
  true durante 250ms. "▲" hace lo mismo hacia atrás. Mantener apretado no
  acelera: sigue siendo un paso cada 250ms.
- Al click en "Confirmar": confirmado toma el valor de opciones[indice].
  Se puede volver a confirmar otro valor después.
- Cuando las cuatro ruedas tienen confirmado, "Continuar" se habilita y al
  click muestra en el footer los cuatro datos juntos.

Constraints:
- Un solo archivo HTML, con el CSS en un <style> y el JS en un <script>.
- Vanilla JS, sin frameworks ni dependencias externas.
- Las ruedas, las opciones y los contadores son elementos del DOM
  posicionados con CSS. No usar <select> ni <input>: quiero poder ver el
  estado reflejado en el DOM.
```

**Qué intentaba lograr:** el artefacto entero de una sola vez, nombrando las cinco capas — estructura con etiquetas semánticas, estilo, comportamiento expresado como estado, y constraints de empaque. La regla que carga el peso es "una sola opción a la vez": es lo que convierte un dato de diez segundos en un minuto de clicks.

**Qué devolvió:** las cuatro ruedas funcionando, con el enfriamiento de 250ms y el contador "opción N de M". Respetó los tres constraints: un archivo, sin dependencias, y ruedas como divs en lugar de `<select>`.

**Qué hice con eso:** lo acepté. Pero el prompt tenía dos huecos que no vi al escribirlo y que el modelo resolvió por su cuenta — están detallados en el README, porque son lo más interesante de esta entrega. No dije qué pasa al llegar al final de una rueda —¿el día 31 vuelve al 1 o se queda clavado?— y el modelo eligió que diera la vuelta. Tampoco dije de dónde salía la lista de países: la escribió él (quedaron 194), y no la verifiqué contra ninguna fuente.
