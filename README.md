# gematsu-calendar

Mirror auto-actualizable del calendario de eventos de
[Gematsu](https://www.gematsu.com/events/).

En lugar de descargar e importar manualmente el `.ics` cada vez que aparecen
eventos nuevos, este repo lo hace por ti: un workflow de GitHub Actions
descarga el feed iCal oficial **una vez al día** y lo publica en GitHub Pages
como un endpoint estable al que te suscribes desde Google Calendar.

## Cómo funciona

```
GitHub Actions (cron diario)
        │  descarga https://www.gematsu.com/events/?ical=1
        ▼
   public/gematsu.ics  ──► GitHub Pages
        │
        ▼
   https://backmind.github.io/gamatsu-calendar/gematsu.ics
        │  (suscripción por URL)
        ▼
     Google Calendar  ──► siempre al día, sin imports manuales
```

El workflow está en [`.github/workflows/update-calendar.yml`](.github/workflows/update-calendar.yml):

- Se ejecuta a diario (`cron: '0 6 * * *'`, 06:00 UTC) y también a mano
  (botón **Run workflow** en la pestaña *Actions*).
- Descarga el iCal, valida que sea un calendario real (no una página de error)
  y lo despliega en GitHub Pages.

## Puesta en marcha (una sola vez)

1. **Habilita GitHub Pages con origen "GitHub Actions":**
   `Settings` → `Pages` → *Build and deployment* → **Source: GitHub Actions**.
2. **Lanza el workflow por primera vez:**
   pestaña `Actions` → *Update Gematsu Calendar* → **Run workflow**.
   (A partir de ahí se ejecutará solo cada día.)
3. **Suscríbete en Google Calendar:**
   - Copia la URL del feed:
     `https://backmind.github.io/gamatsu-calendar/gematsu.ics`
   - Google Calendar → *Otros calendarios* → **+** → *Suscribirse mediante URL*
   - Pega la URL → **Añadir calendario**.

A partir de ahí, Google re-sincroniza el calendario periódicamente y los nuevos
eventos de Gematsu aparecen automáticamente.

## Notas

- **Frecuencia de refresco en Google:** Google Calendar decide cuándo
  re-sincroniza los calendarios suscritos por URL (suele ser cada varias horas,
  no instantáneo). Para eventos que no son de última hora es más que suficiente.
- **Ajustar la hora/frecuencia:** edita la línea `cron` del workflow
  (ver [crontab.guru](https://crontab.guru)).
- **Si la descarga falla,** el despliegue se aborta y la última versión válida
  publicada sigue activa.
