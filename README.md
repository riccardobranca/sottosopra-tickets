# sottosopra-tickets

Landing biglietti del festival Sottosopra 2026, riusabile come **link-in-bio / hub biglietti** su più canali.
Servita su **https://tickets.sottosopra.com** (GitHub Pages + record CNAME su Porkbun).

## Modello: una pagina per CANALE (non più per città)
Stessa landing, declinata per **scopo/canale**, così sappiamo da dove arrivano i click. Cambia solo l'UTM dei link DICE.

| URL | Canale | UTM (source / medium) | QR? |
|---|---|---|---|
| `/manifesti` | Manifesti (QR unico su tutti) | `manifesto` / `qr` | ✅ `qr/manifesti.svg` |
| `/locandine` | Locandine / flyer | `locandina` / `qr` | ✅ `qr/locandine.svg` |
| `/ig` | Link-in-bio Instagram (al posto di Linktree) | `instagram` / `bio` | — (link cliccabile) |
| `/` | Accesso diretto / generico | `tickets` / `direct` | — |

- I QR puntano al **nostro** dominio → i link DICE dietro si cambiano senza ristampare.
- DICE attribuisce gli acquisti per canale (UTM); Cloudflare Web Analytics conta le aperture per path.
- ⚠️ Niente più dettaglio per-comune (scelta di costo stampa: un solo QR su tutti i manifesti).

## Aggiungere un canale (es. "newsletter")
```bash
mkdir -p newsletter
sed -e 's/__SRC__/newsletter/g' -e 's/__MED__/email/g' _template.html > newsletter/index.html
# (QR solo se serve stamparlo)
qrencode -t SVG -o qr/newsletter.svg "https://tickets.sottosopra.com/newsletter"
git add -A && git commit -m "Canale: newsletter" && git push
```

## ⚠️ Regole
- `tickets.sottosopra.com` è **stampato nei QR** → non cambiarlo mai.
- **Non stampare** finché la landing non risponde e il QR non è stato **scansionato davvero** da un telefono.
- Mai committare segreti (chiavi/token). Il token di Cloudflare Web Analytics è pubblico per design: ok.
- Giovedì 30 (gratis) per ora NON incluso (nessun evento DICE).
