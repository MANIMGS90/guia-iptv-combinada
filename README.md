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

   https://raw.githubusercontent.com/TU_USUARIO/TU_REPO/main/guide.xml

   (si tu rama por defecto se llama `master` en vez de `main`, cambiá esa
   parte de la URL). También queda `guide.xml.gz` por si el lado que lo
   consume prefiere la versión comprimida.

A partir de ahí, se actualiza sola todos los días — no hay que volver a
tocar nada salvo que quieras ajustar algo (ver abajo).

## Ajustes opcionales

- **Cuántos días de programación pedir a gatotv.com**: en
  `.github/workflows/update-epg.yml`, el flag `--days=3` del paso
  "Descargar guía de gatotv.com para mis canales". Más días = guía más
  larga pero la Action tarda más y le pega más pedidos a gatotv.com.
- **Horario de actualización**: la línea `cron: "0 8 * * *"` (formato
  UTC). Por ejemplo `"0 12 * * *"` sería las 6 AM de Ciudad de México.
- **Agregar más canales de gatotv.com**: sumá líneas al mismo formato en
  `channels/mis_canales_channels.xml` (mirá
  `sites/gatotv.com/gatotv.com.channels.xml` en el repo de iptv-org/epg
  para ver más `site_id` disponibles).

## Si algo falla

- La pestaña **Actions** del repo muestra el log completo de cada
  corrida — el paso que falla te dice por qué (por ejemplo, si el
  secret `M3U4U_EPG_URL` no está configurado, el workflow corta ahí con
  un mensaje explícito en vez de fallar oscuro).
- Si `npm run grab` falla para algún canal puntual de gatotv.com (pasa
  ocasionalmente si esa página cambia de formato), no frena a los demás
  canales — seguís teniendo guía para el resto.
- `scripts/merge_epg.py` avisa por el log si algún id de canal choca
  entre las dos fuentes (no debería pasar: gatotv.com usa ids numéricos
  y m3u4u usa nombres, pero queda anotado por si en el futuro sumás una
  tercera fuente).
