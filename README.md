# Guía EPG combinada (gatotv.com + m3u4u)

Este repositorio arma **una sola guía XMLTV** todos los días, combinando:

1. **`channels/mis_canales_channels.xml`** → tus 131 canales mexicanos/latinos,
   scrapeados en vivo desde gatotv.com con el grabber de
   [iptv-org/epg](https://github.com/iptv-org/epg).
2. **Tu guía de m3u4u.com** → se descarga en vivo desde tu URL de m3u4u
   (la que subiste, `m3u4u-...-EPG-Demo.xml`, es una foto fija de esa guía;
   acá se vuelve a pedir fresca cada día).

El resultado (`guide.xml` / `guide.xml.gz`) se commitea de vuelta a este
mismo repositorio todos los días a las 08:00 UTC (2 AM Ciudad de México),
y queda disponible en una URL fija y gratuita.

## Puesta en marcha (una sola vez)

1. **Creá un repositorio nuevo en GitHub**, público (así las Actions salen
   gratis e ilimitadas — en un repo privado también funciona, pero con un
   tope de minutos gratis al mes; de sobra para esto, pero público es más
   simple). No hace falta que la guía combinada sea "secreta": son solo
   horarios de programación de TV.

2. **Subí el contenido de este zip** a la raíz de ese repositorio
   (con `git push` o arrastrando los archivos en la web de GitHub —
   "Add file → Upload files").

3. **Conseguí tu URL "en vivo" de m3u4u** (no el archivo que subiste, que
   es una descarga puntual):
   - Entrá a m3u4u.com → **EPGs → Manager**.
   - Click en la flecha de descarga de tu lista de EPG.
   - Click en **Copy link** para copiar la URL directa (algo como
     `https://m3u4u.com/xml/xxxxx-xxxxx-xxxxx`).

4. **Guardá esa URL como secret del repositorio** (no la pongas suelta en
   ningún archivo):
   - En GitHub: **Settings → Secrets and variables → Actions → New
     repository secret**.
   - Nombre: `M3U4U_EPG_URL`
   - Valor: la URL que copiaste en el paso 3.

5. **Activá el workflow**: pestaña **Actions** del repo → si te pide
   habilitarlas, aceptá → entrá a "Actualizar guía EPG combinada" →
   **Run workflow** (botón a la derecha) para probarlo ahora mismo sin
   esperar al horario programado.

6. Cuando termine (unos minutos), vas a tener `guide.xml` en la raíz del
   repo. Tu URL final, para pegar en tu panel Xtream o en lo que consuma
   la guía, es:
