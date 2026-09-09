<a href="https://github.com/zechub/zechub/edit/main/site/Zcash_Tech/Lightwallet_Nodes.md" target="_blank">
  <img src="https://img.shields.io/badge/Edit-blue" alt="Edit Page"/>
</a>


# Nodos de billeteras ligeras de Zcash

## TL;DR

* La mayoría de las personas usa Zcash mediante una billetera ligera, que no descarga toda la blockchain. En su lugar, se comunica con un servidor que ya ha realizado ese trabajo.
* Actualmente, dos programas dan servicio a las billeteras ligeras: **lightwalletd**, el servicio original escrito en Go, y **Zaino**, un indexador más reciente escrito en Rust.
* Tus claves nunca salen de tu dispositivo, y el servidor no puede gastar tus fondos ni leer los importes y memos dentro de transacciones totalmente blindadas.
* Lo que el servidor está en buena posición de conocer es tu dirección IP y el momento de tu actividad — las transacciones blindadas protegen lo que ocurre en la blockchain, no tu conexión con el servidor.
* Tor elimina el identificador de IP; está disponible en billeteras basadas en `zcash_client_backend`, y en ZODL es un ajuste en Configuración avanzada.
* Puedes cambiar el servidor que usa tu billetera o ejecutar el tuyo propio — tanto lightwalletd como Zaino son de código abierto.

## Explicación básica

La mayoría de las personas usa Zcash mediante una billetera ligera, que no descarga toda la blockchain. En su lugar, se comunica con un servidor que ya ha realizado ese trabajo. Esta página explica qué son esos servidores, qué pueden y no pueden ver sobre ti, cómo enrutar tu conexión a través de Tor y cómo cambiar el servidor que usa tu billetera.

Actualmente, dos programas dan servicio a las billeteras ligeras. **lightwalletd** es el servicio original, escrito en Go. **Zaino** es un indexador más reciente escrito en Rust, creado como parte del trabajo de desuso de zcashd.

### Qué hace un servidor de billetera ligera

Un servidor de billetera ligera se sitúa entre tu billetera y la blockchain de Zcash y le proporciona una vista de la cadena eficiente en ancho de banda. Hace tres cosas por ti.

Sirve bloques compactos. En lugar de bloques completos, envía una forma compacta que contiene solo lo que una billetera necesita para detectar un pago a su dirección blindada, detectar un gasto de sus notas y actualizar sus testigos.

Retransmite tus transacciones. Cuando envías, tu billetera entrega la transacción terminada al servidor, que la transmite a la red.

Responde consultas de la cadena, como la altura actual y la información de comisiones que necesita tu billetera.

Tu billetera sigue realizando el trabajo privado localmente. Guarda tus claves, descifra bloques por prueba para encontrar tus notas, y construye y firma transacciones en tu dispositivo.

### Qué puede y no puede ver el servidor

Esta es la parte en la que es fácil equivocarse. Tus claves nunca salen de tu dispositivo, pero eso no significa que el servidor no aprenda nada sobre ti.

La referencia aquí es el [modelo de amenazas de la aplicación de billetera Zcash](https://zcash.readthedocs.io/en/latest/rtd_pages/wallet_threat_model.html), que vale la pena leer completo si te importa este tema. Expone varios tipos de adversarios. El que importa para esta página es un adversario que puede observar el tráfico entre tu billetera e internet, y entre el servidor e internet. Quien opera el servidor se encuentra inherentemente en parte de esa posición, porque tu billetera se conecta directamente con esa persona.

Empecemos por lo que está protegido. Frente a todo adversario del modelo, incluido uno que haya comprometido el servidor, este "no puede conocer ninguno de los materiales criptográficos de clave del usuario (claves de gasto, claves de visualización, frase semilla, etc.)", no puede robar tus fondos y no puede hacer que envíes fondos que no pretendías enviar. Los importes y memos dentro de las transacciones totalmente blindadas permanecen cifrados.

Luego está lo que no está protegido. El modelo de amenazas enumera estas como debilidades conocidas frente a un adversario que observa el tráfico:

| Debilidad | Cómo |
|:--|:--|
| Saber quién eres | "El adversario conoce la dirección IP del usuario, lo que podría llevarlo a la identidad real del usuario" |
| Saber aproximadamente dónde estás | Buscar tu IP "en una base de datos de geolocalización para aproximar su ubicación" |
| Saber si y cuándo enviaste o recibiste una transacción blindada | Enviar "usa más ancho de banda, lo cual es visible aunque la conexión esté cifrada". El modelo señala que el propio servidor puede ver el acto de enviar y recibir |
| Contar cuántas transacciones has realizado a lo largo del tiempo | Los mismos patrones de ancho de banda, observados durante un período más largo |
| Detectar patrones de pago recurrentes | Observar cuándo ocurre la actividad |
| Determinar si una dirección es tuya | Un adversario que ya conoce una dirección "podría enviar fondos a esa dirección y observar si hay picos de ancho de banda" de tu billetera al obtenerla |

El modelo también señala que el caso habitual supone "una relación de confianza entre el usuario y el operador del servidor lightwalletd".

Así que este es el resumen honesto. Un servidor de billetera ligera no puede gastar tu dinero ni leer los importes o memos de tus transacciones blindadas. Lo que está en buena posición de conocer es tu dirección IP y el momento de tu actividad, y esas dos cosas juntas pueden revelar mucho sobre una persona. Las transacciones blindadas protegen lo que ocurre en la blockchain. Por sí solas, no ocultan tu conexión con el servidor.

## Visual / Analogía

Piensa en una biblioteca pública que guarda todos los periódicos jamás impresos. Un nodo completo es un lector que se lleva a casa todo el archivo. Una billetera ligera es un lector que pide al bibliotecario un resumen diario — una hoja delgada que contiene lo justo para detectar si algo le concierne.

El resumen está sellado: el bibliotecario lo prepara sin poder leer qué elementos te importan, y tú lo abres en casa con tu propia clave. Ese es el bloque compacto, y la apertura es el descifrado por prueba en tu dispositivo.

Pero el bibliotecario sigue viendo qué lector entró, a qué hora y qué tan grueso era el paquete que sacó. Esa es la dirección IP y el momento — visibles desde el mostrador, sin importar cuán bien sellado esté el sobre. Tor equivale a enviar un mensajero anónimo: el bibliotecario sigue entregando el mismo paquete, pero ya no sabe a qué casa va.

## Análisis profundo

### Enrutamiento a través de Tor

Tor rompe el vínculo entre tu dirección IP y el tráfico de tu billetera, lo que elimina el identificador más fuerte de la tabla anterior.

Existe soporte en las bibliotecas Rust en las que se basan muchas billeteras de Zcash. zcash_client_backend incluye un módulo Tor basado en [Arti](https://tpo.pages.torproject.net/core/arti/), la implementación de Tor en Rust, por lo que una billetera puede enrutar la sincronización, la transmisión de transacciones y las consultas de precios a través de Tor sin incluir un cliente Tor separado.

Los desarrolladores de Zaino plantean el mismo argumento, citando directamente el modelo de amenazas: existe "la necesidad de usar protocolos de transporte anónimos (como Nym o Tor) para ofuscar las identidades de los clientes frente a los servidores de indexación de Zcash".

En **ZODL**, Tor es un ajuste en Configuración avanzada. Las notas de lanzamiento de la billetera indican a los usuarios el modo de conexión manual "más habilitar Tor en Configuración avanzada" si "prefieren reducir la exposición de metadatos", y la aplicación ofrece activar Tor antes de restaurar una billetera, que es el momento en que una IP nueva quedaría vinculada de otro modo a todo el historial de una billetera.

Dos advertencias. Tor oculta tu IP al servidor, pero no cambia lo que el servidor aprende de las solicitudes que realizas. Y el enrutamiento cebolla añade latencia, por lo que la sincronización tarda más. Ejecutar tu propio servidor evita la cuestión de confianza de otra manera, ya que entonces tú eres el operador.

### Zaino, el indexador de Rust

[Zaino](/zcash-tech/zaino) es un indexador escrito en Rust por el equipo de Zingo, creado para reemplazar lightwalletd como parte del trabajo de desuso de zcashd. Da servicio a clientes ligeros, clientes completos y exploradores de bloques, leyendo datos de la cadena almacenados por "un validador completo Zebra o Zcashd".

Está en desarrollo activo, con la versión 0.8.0 publicada en agosto de 2026. Su objetivo es mantener la compatibilidad retroactiva con lightwalletd cuando sea posible, para que las billeteras puedan apuntar a él sin tener que reescribirse.

Zaino tiene su propia página con diagramas de arquitectura, por lo que esta página solo cubre su función como servidor de billetera ligera.

### Ejecutar el tuyo propio

La opción más sólida es ser tu propio operador, lo que elimina por completo la cuestión de confianza. Ambos servidores son de código abierto: [lightwalletd](https://github.com/zcash/lightwalletd) en Go y [Zaino](https://github.com/zingolabs/zaino) en Rust. Ambos leen desde un validador completo, por lo que también querrás [Zebra](/zcash-tech/zebra-full-node).

## Implicaciones prácticas

### Lista de servidores

El panel de [hosh.zec.rocks](https://hosh.zec.rocks/zec) rastrea servidores públicos y su estado, y es el lugar para comprobar qué está realmente activo. [status.zec.rocks](https://status.zec.rocks/) muestra el estado del servicio.

Servidores enumerados en ese panel al momento de escribir:

| Servidor | Notas |
|:--|:--|
| zec.rocks:443 | Los puntos de conexión regionales aparecen junto a él en na.zec.rocks, eu.zec.rocks, ap.zec.rocks y sa.zec.rocks |
| zec-node.cakewallet.com:443 | En el dominio de Cake Wallet |
| zec.0xrpc.io:443 | Operado por 0xRPC, que ofrece puntos de conexión públicos gratuitos para varias cadenas y solicita donaciones para cubrir la capacidad |
| zaino.unsafe.zec.rocks:443 | Una instancia de Zaino. Ten en cuenta el nombre de host; considérala experimental |
| testnet.zec.rocks:443 | Red de prueba, con una instancia de prueba de Zaino enumerada en zaino.testnet.unsafe.zec.rocks |

Consulta el panel en lugar de confiar en esta lista. Los operadores aparecen y desaparecen, y una página como esta envejece.

### Cambiar el servidor en tu billetera

Vale la pena hacerlo si quieres elegir un operador en quien confíes, distribuir la actividad entre operadores o apuntar al tuyo propio.

Las rutas de menú a continuación eran correctas cuando se actualizó esta página, pero las interfaces de las billeteras cambian, así que tómales como una pista y no como una ruta exacta. Busca Configuración avanzada o una opción de servidor.

#### ZODL

Anteriormente Zashi. El engranaje en la esquina superior derecha y luego Configuración avanzada. Tor está en la misma pantalla. ZODL también ofrece un acceso directo Cambiar servidor cuando un fallo de sincronización se debe a que el servidor está desactualizado.

#### Ywallet

El engranaje en la esquina superior derecha y luego la pestaña Zcash.

![Configuración del servidor de Ywallet](/content-images/b0a2910b-dbdf-4292-8e69-af5a386aa183-f51f098d19.webp)

#### Zingo

El menú de hamburguesa en la esquina superior izquierda, luego Configuración y después desplázate hacia abajo.

![Configuración del servidor de Zingo](/content-images/ea8f7672-e644-41a5-a422-db131740404a-2626f5fa79.webp)

#### eZcash

El menú de hamburguesa en la esquina superior izquierda, luego Configuración y después Avanzado.

![Configuración del servidor de eZcash](/content-images/655c0172-61a0-4322-b8cf-4eee4bb53b51-0b93df2e71.webp)

Esas capturas de pantalla se tomaron en marzo de 2025, y las aplicaciones han publicado versiones desde entonces, por lo que los botones pueden haberse movido.

## Errores comunes

**Pensar que el servidor puede leer tus transacciones**. No puede. Tus claves permanecen en tu dispositivo, y los importes y memos dentro de las transacciones totalmente blindadas permanecen cifrados — incluso frente a un adversario que haya comprometido el servidor.

**Interpretar "blindado" como "conexión anónima"**. Las transacciones blindadas protegen lo que ocurre en la blockchain. Tu dirección IP y el momento de tu actividad son una capa aparte, y esa capa es exactamente lo que ve el servidor.

**Suponer que Tor elimina todo rastro**. Tor oculta tu IP al servidor, pero no cambia lo que el servidor aprende de las solicitudes que realizas, y añade latencia a la sincronización.

**Confiar en una lista de servidores de una página wiki**. Los operadores aparecen y desaparecen. Consulta [hosh.zec.rocks](https://hosh.zec.rocks/zec) para saber qué está realmente en funcionamiento antes de apuntar tu billetera a algo.

## Resumen

Las billeteras ligeras te dan acceso al pool blindado sin el espacio en disco, lo que es un buen intercambio. Solo ten claro qué estás intercambiando. El servidor no puede tomar tus fondos ni leer tus importes blindados, pero está en buena posición de ver tu dirección IP y cuándo realizas transacciones. Enruta a través de Tor, elige deliberadamente a tu operador o ejecuta el tuyo propio.

## Páginas relacionadas

- [Quién puede ver tu pago de Zcash](/start-here/who-can-see-your-zcash-payment) — la perspectiva para principiantes sobre la misma pregunta.
- [Qué puede ver un explorador de bloques](/zcash-tech/what-a-block-explorer-can-see) — qué es visible en la cadena, en lugar de en el servidor.
- [Zaino](/zcash-tech/zaino) — diagramas de arquitectura y la función más amplia del indexador de Rust.
- [Nodo completo Zebra](/zcash-tech/zebra-full-node) — el validador del que lee un servidor de billetera ligera.
- [Sincronización de billeteras Zcash](/zcash-tech/zcash-wallet-syncing) — cómo tu billetera procesa los bloques compactos que envía un servidor.

**Última actualización:** agosto de 2026
