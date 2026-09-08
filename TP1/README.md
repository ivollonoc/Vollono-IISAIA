# TP 1 — Selector de fecha y país de a una opción
El HTML es un formulario con ingreso de datos de nacimiento (fecha y pais) pensado como un paso intermedio de una solicitud de datos.
Tiene un  selector de datos de nacimiento donde cada valor —día, mes, año y país— se elige de una "rueda" que muestra una sola opción por vez, sin lista, y que no deja avanzar más rápido de un paso cada 250 ms. Funciona bien pero no es amigable para el usuario. 
En la v2 se redibuja el layout y se agrega la posibilidad de filtrar y seleccionar dentro de un desplegable para la categoría de pais lo que reduce el tiempo necesario para ingresar ese input puntual.
En la v3 se agregan confrimaciones explícitas tras el ingreso de cada campo y se ingresan nuevos estados tras poner "Continuar" en el formulario en general. Marca un "enviando" que falla tras unos segundos y resetea al estado anterior.

## Cómo se ejecuta

Doble click en `index.html`. Un solo archivo, sin dependencias.

## Qué me propuse construir

Una bad UI hostil por estructura y no por engaño. No esconde nada —el contador debajo de cada ventana dice "opción 47 de 194", así que siempre sabés cuánto falta— y aun así cuesta, porque la única forma de recorrer 194 países es apretar ▼ de a uno, con un freno de 250 ms entre clicks que hace que mantener apretado no sirva de nada. Desde la v3 la piel es moderna y prolija, y eso hace que la fricción de fondo moleste más: ya no hay excusa de "es un formulario viejo".

**v1** — prompt 1: las cuatro ruedas, el enfriamiento y la confirmación por rueda.

**v2** — prompt 2, una pasada de layout: el botón "Confirmar" pasa a estar abajo de cada rueda; día, mes y año quedan agrupados a la izquierda y país se va contra el borde derecho, con otra tinta (encabezado y borde en azul grisáceo) para marcar que es otro tipo de dato. La única función nueva es un buscador arriba de la rueda de país: escribís y aparece un desplegable con los países que empiezan con ese texto; al elegir uno, la rueda salta a esa opción (pero seguís teniendo que confirmar). La mecánica hostil no se toca.

**v3** — prompt 3, una pasada de piel y de feedback: bordes redondeados, paleta clara y actual (blancos y grises fríos, líneas finas, sombra despegada) manteniendo la seriedad de trámite, un ✅ verde en cada rueda al confirmarla, y un "Continuar" que muestra "Enviando…" con spinner, cae en un error rojo a la derecha del botón a los ~1,8 s y se borra solo a los 3 s. La mecánica y el estado de las ruedas no cambian; se suma el estado `envio`.

## Decisiones que tomé yo

**DOM en vez de `<select>` o `<input>`.** La más importante. Un selector de fecha pedido a secas sale con tres `<select>` nativos: anda, pero el estado se lo queda el navegador. Acá cada rueda, su opción visible y su contador son divs, y se puede abrir el inspector a mirar `estado.ruedas[i].indice` y lo que muestra la ventana al mismo tiempo.

**Una sola opción visible, sin degradé.** Nada de dejar asomar la opción de arriba y la de abajo. La ventana muestra un valor y nada más; que hay más se sabe solo por el contador. Es lo que vuelve la rueda un túnel en lugar de una lista.

**Enfriamiento de 250 ms.** Sin esto, mantener apretado el botón recorre la lista en un segundo y la pieza deja de doler. El freno está a la vista —las flechas se apagan mientras dura— pero no se puede saltear.

**Wrap-around al final de la rueda.** El prompt no decía qué pasa después del día 31 o del país 194. Elegí que diera la vuelta al principio. La otra opción —que se clave en el extremo— era más honesta como señal de "llegaste al final", pero te dejaba sin forma rápida de volver si te pasaste.

**Confirmación por rueda.** Cada rueda se confirma sola y se puede volver a confirmar otro valor. "Continuar" recién se habilita con las cuatro confirmadas. Sirve para que el estado `confirmado` quede visible y separado del `indice` que estás moviendo.

**El buscador de país mueve `indice`, no `confirmado` (v2).** Elegir del desplegable deja la rueda apuntando al país, pero hay que tocar "Confirmar" igual que en las otras tres. Así el buscador es una ayuda de navegación y no un atajo que rompe la simetría, y el salto no dispara el enfriamiento porque no es un paso.

**Reacomodar el layout no tocó el estado (v2).** "Confirmar" abajo, país a la derecha y su color aparte son CSS: `estado.ruedas` sigue en el mismo orden y país sigue siendo el índice 3. Lo único que se sumó al estado es `busquedaPais`.

**El ✅ sigue al `confirmado`, no a la ventana (v3).** El tilde y el valor verde de abajo muestran el último valor confirmado de esa rueda. Si movés la rueda después de confirmar, quedan en el valor viejo hasta que toques "Confirmar" de nuevo — porque ese es el estado real, y la ventana es solo lo que estás mirando.

**El envío falla a propósito (v3).** "Continuar" no confirma nada: entra en `cargando`, cae en `error` y el cartel se borra solo a los 3 s. Un envío que sale bien al toque no genera ninguna tensión; hacer todo el recorrido tedioso para terminar sin saber si sirvió, sí. Es simulado (no hay backend), con `setTimeout`.

**Piel moderna, hueso igual (v3).** Bordes redondeados, paleta clara, sombra suave: la interfaz se ve actual, pero una opción por vez y el enfriamiento de 250 ms siguen intactos. El contraste es el punto.

## Qué salió mal y cómo lo corregí

El prompt 1 tenía dos huecos que no vi al escribirlo:

- **No dije qué pasa al llegar al final de una rueda.** El modelo eligió wrap-around; lo dejé, pero la decisión la tomó él, no yo.
- **No dije de dónde salía la lista de países.** La escribió el modelo, unos 194 nombres en español, y no la crucé contra ninguna fuente oficial. Para la entrega la revisé a mano y la ordeno en runtime con `localeCompare("es")`, pero puede tener faltantes o nombres discutibles.

Las dos ambigüedades no rompieron nada, y ahí está la trampa: juzgando por el resultado, uno se queda con que el prompt estaba bien escrito. Faltó releerlo cruzando la sección de estructura con la de comportamiento antes de mandarlo.

## Prompts

El registro versionado está en [prompts.md](prompts.md): arranca con el prompt 1 y crece un tramo por versión. Los tres prompts pensados de antemano viven en `prompts_guia.md`, que queda fuera del repo.
