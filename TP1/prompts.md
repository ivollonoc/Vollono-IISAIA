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

---

## 2 — Pulir el layout: confirmar abajo, país aparte, buscador de país · v2

```
Reacomodá el layout de las ruedas y agregá una sola función nueva: poder
buscar el país escribiendo.

Layout:
- El <button> "Confirmar" de cada rueda deja de estar al costado y pasa a
  estar abajo de todo, debajo del contador. Cada rueda queda como una
  pila vertical: nombre, ▲, ventana, contador, ▼, "Confirmar", y abajo el
  valor confirmado.
- Las ruedas de día, mes y año quedan agrupadas a la izquierda. La rueda
  de país se separa del grupo y se va contra el borde derecho, con un
  espacio visible en el medio.
- Fecha y país se distinguen por color: las tres ruedas de fecha
  mantienen el gris/beige actual; la de país usa otra tinta —borde y
  encabezado en un tono frío, dentro de la misma paleta de trámite viejo,
  sin redondeo ni sombras nuevas—. El color dice "esto es otro tipo de
  dato", no decora.

Buscar país:
- Arriba de la ventana de la rueda de país hay un campo de texto. Estado
  nuevo: busquedaPais (el texto tipeado, o "").
- Mientras busquedaPais tiene texto, debajo del campo aparece un
  desplegable con los países de opciones que empiezan con ese texto (sin
  distinción de mayúsculas ni acentos), como máximo 8, uno por línea.
- Click en una opción del desplegable: la rueda de país mueve indice a
  esa opción y la muestra en la ventana. El desplegable se cierra y
  busquedaPais vuelve a "".
- El desplegable también se cierra al vaciar el campo. No queda abierto
  tapando las otras ruedas.

Tres reglas: elegir del desplegable mueve indice pero no toca confirmado
—el país confirmado sigue siendo el anterior hasta que se toca
"Confirmar"—; elegir del desplegable es un salto directo, no un paso, así
que no dispara el enfriamiento de 250ms ni lo espera; y reacomodar el
layout no cambia el orden de estado.ruedas ni los índices —país sigue
siendo la rueda 3, lo que cambió es el CSS—.
```

**Qué intentaba lograr:** que la pieza se lea de una. Con las cuatro ruedas iguales y pegadas no se entiende que fecha y país son cosas distintas, y el "Confirmar" al costado le comía ancho a la ventana. Separar país, bajarle el botón a cada rueda y pintarlo distinto es puro layout: la mecánica hostil —una opción por vez, enfriamiento de 250ms— no se toca.

**Por qué el buscador de país:** 194 países de a uno con ▲/▼ no es difícil, es tedio sin techo. El campo de texto le pone un piso: tipeás "Ar" y saltás cerca. No lo vuelve cómodo —seguís teniendo que confirmar, y día/mes/año siguen siendo a mano— pero deja de ser impracticable.

**Por qué las tres reglas:** son los bugs que este cambio invita. Si el desplegable escribe `confirmado`, saltea el paso de confirmar y rompe la simetría con las otras ruedas. Si el salto respeta el enfriamiento, el desplegable se siente roto. Y si "reacomodar el layout" se interpreta como "reordenar el array", `confirmado` de país termina apuntando a otro valor — la confusión clásica entre el estado y su orden en el DOM.

**Qué devolvió:** las ruedas en pila con "Confirmar" abajo de todo, la de país contra el borde derecho con encabezado y borde en azul grisáceo (`#2f4f6f`) y fondo apenas azulado, y el buscador con desplegable de hasta 8 nombres. El orden de `estado.ruedas` quedó igual; lo único que se sumó al estado es `busquedaPais`. La ventana de país ahora admite dos renglones para los nombres largos, sin mostrar más de una opción.
