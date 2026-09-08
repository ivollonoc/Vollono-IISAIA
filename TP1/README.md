# TP 1 — Selector de fecha y país de a una opción

Un selector de datos de nacimiento donde cada valor —día, mes, año y país— se elige de una "rueda" que muestra una sola opción por vez, sin lista y sin buscador, y que además no deja avanzar más rápido de un paso cada 250 ms. Funciona bien y usarlo es horrible, que era la idea.

## Cómo se ejecuta

Doble click en `index.html`. Un solo archivo, sin dependencias.

## Qué me propuse construir

Una bad UI hostil por estructura y no por engaño. No esconde nada —el contador debajo de cada ventana dice "opción 47 de 194", así que siempre sabés cuánto falta— y aun así cuesta, porque la única forma de recorrer 194 países es apretar ▼ de a uno, con un freno de 250 ms entre clicks que hace que mantener apretado no sirva de nada.

Esta es la **v1**, construida solo a partir del primer prompt: las cuatro ruedas, el enfriamiento y la confirmación por rueda. Los prompts 2 y 3 —favoritos y salto para darle algo de agencia al usuario, y el envoltorio de trámite— están en la guía local `prompts_guia.md` (no versionada), todavía sin aplicar.

## Decisiones que tomé yo

**DOM en vez de `<select>` o `<input>`.** La más importante. Un selector de fecha pedido a secas sale con tres `<select>` nativos: anda, pero el estado se lo queda el navegador. Acá cada rueda, su opción visible y su contador son divs, y se puede abrir el inspector a mirar `estado.ruedas[i].indice` y lo que muestra la ventana al mismo tiempo.

**Una sola opción visible, sin degradé.** Nada de dejar asomar la opción de arriba y la de abajo. La ventana muestra un valor y nada más; que hay más se sabe solo por el contador. Es lo que vuelve la rueda un túnel en lugar de una lista.

**Enfriamiento de 250 ms.** Sin esto, mantener apretado el botón recorre la lista en un segundo y la pieza deja de doler. El freno está a la vista —las flechas se apagan mientras dura— pero no se puede saltear.

**Wrap-around al final de la rueda.** El prompt no decía qué pasa después del día 31 o del país 194. Elegí que diera la vuelta al principio. La otra opción —que se clave en el extremo— era más honesta como señal de "llegaste al final", pero te dejaba sin forma rápida de volver si te pasaste.

**Confirmación por rueda.** Cada rueda se confirma sola y se puede volver a confirmar otro valor. "Continuar" recién se habilita con las cuatro confirmadas. Sirve para que el estado `confirmado` quede visible y separado del `indice` que estás moviendo.

## Qué salió mal y cómo lo corregí

El prompt 1 tenía dos huecos que no vi al escribirlo:

- **No dije qué pasa al llegar al final de una rueda.** El modelo eligió wrap-around; lo dejé, pero la decisión la tomó él, no yo.
- **No dije de dónde salía la lista de países.** La escribió el modelo, unos 194 nombres en español, y no la crucé contra ninguna fuente oficial. Para la entrega la revisé a mano y la ordeno en runtime con `localeCompare("es")`, pero puede tener faltantes o nombres discutibles.

Las dos ambigüedades no rompieron nada, y ahí está la trampa: juzgando por el resultado, uno se queda con que el prompt estaba bien escrito. Faltó releerlo cruzando la sección de estructura con la de comportamiento antes de mandarlo.

## Prompts

El registro versionado está en [prompts.md](prompts.md): arranca con el prompt 1 y crece un tramo por versión. Los tres prompts pensados de antemano viven en `prompts_guia.md`, que queda fuera del repo.
