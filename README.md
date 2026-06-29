# sottosopra-tickets

Landing biglietti per i **manifesti** del festival Sottosopra 2026, una per città (per il tracking via QR).
Servita su **https://tickets.sottosopra.com** (GitHub Pages + record CNAME su Porkbun).

## Come funziona
- `_template.html` = modello unico. Contiene il token `__CITTA__` negli UTM dei link DICE.
- Ogni città è una cartella: `/<città>/index.html` → URL `https://tickets.sottosopra.com/<città>`.
- Stessa pagina per tutte; cambia solo `utm_campaign=<città>` → DICE attribuisce gli acquisti per città, e Cloudflare Web Analytics conta le scansioni per città (la pageview di `/<città>`).
- I **QR** stanno in `/qr/<città>.svg` e codificano l'URL della città. Puntano al *nostro* dominio: i link DICE dietro si possono cambiare senza ristampare.

## Aggiungere una città (es. "padova")
```bash
mkdir -p padova
sed 's/__CITTA__/padova/g' _template.html > padova/index.html
qrencode -t SVG -o qr/padova.svg "https://tickets.sottosopra.com/padova"
git add -A && git commit -m "Aggiunta città: padova" && git push
```

## ⚠️ Regole
- Il sottodominio `tickets.sottosopra.com` è **stampato nei QR** → non cambiarlo mai.
- **Non stampare** un manifesto finché la sua landing non risponde e il QR non è stato **scansionato per davvero** da un telefono.
- Giovedì 30 (gratis) per ora NON è incluso (nessun evento DICE).
