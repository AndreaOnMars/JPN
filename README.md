# JPN – www.alessandrolana.it

Landing page del libro fotografico **JPN** di Alessandro Lana.

Il sito è statico e viene pubblicato con **GitHub Pages**: ogni volta che aggiorni il ramo `main`, una GitHub Action pubblica il contenuto della cartella `site/` su https://www.alessandrolana.it.

Su TopHost restano il dominio, i DNS e la posta (info@alessandrolana.it), che non viene toccata.

```
site/                    → i file pubblicati
  index.html
  404.html               → qualsiasi indirizzo inesistente (es. vecchi link WordPress) porta alla home
  CNAME                  → www.alessandrolana.it
  img/                   → foto in WebP (600 / 1000 / 1200 px …)
  js/lenis.min.js        → scorrimento morbido (copia locale, nessun CDN)
  favicon…, site.webmanifest, og-image.jpg, robots.txt, sitemap.xml
.github/workflows/pages.yml → la pubblicazione automatica
```

---

## Prima pubblicazione (da fare una volta)

### 1. GitHub: repository e pubblicazione

1. Su github.com: **New repository**, per esempio `jpn-site`, **pubblico**.
   - Con l'account gratuito GitHub Pages funziona solo con repository pubblici.
   - I file del sito sono comunque pubblici per natura.
2. **Add file → Upload files**: trascina dentro tutto il contenuto di questa cartella, mantenendo `site/` e `.github/`.
   - La cartella `.github` sul Mac è nascosta: nel Finder premi `Cmd + Shift + .` per vederla, oppure usa GitHub Desktop.
   - Conferma con **Commit changes** su `main`.
3. **Settings → Pages → Build and deployment → Source: "GitHub Actions".**
4. Vai su **Actions**: il workflow "Deploy to GitHub Pages" parte da solo (se no, **Run workflow**).
   - In un minuto il sito è online su `https://<tuo-utente>.github.io/jpn-site/`.
   - Puoi già controllare lì che tutto funzioni.
5. **Settings → Pages → Custom domain**: scrivi `www.alessandrolana.it` e premi **Save**. Il controllo DNS fallirà finché non fai il passo 2.

### 2. TopHost: DNS (Utility → Gestione DNS)

Cambi **solo** i record del sito. Non toccare MX, TXT (`v=spf1…`), `mail`, `pop`, `sql`, `ftp`: servono alla posta e al pannello.

| Azione | Nome | Tipo | Valore |
|---|---|---|---|
| elimina | `www` | A | 217.64.195.187 |
| elimina | `www` | AAAA | 2001:4b78:1001:0:0:0:0:1701 |
| aggiungi | `www` | CNAME | `<tuo-utente>.github.io` |
| modifica | `@` | A | da 217.64.195.187 a `185.199.108.153` |
| aggiungi | `@` | A | `185.199.109.153` |
| aggiungi | `@` | A | `185.199.110.153` |
| aggiungi | `@` | A | `185.199.111.153` |
| elimina | `@` | AAAA | 2001:4b78:1001:0:0:0:0:1701 |

`<tuo-utente>` è il nome del tuo account GitHub, in minuscolo (es. `andreamarson.github.io`).

La propagazione richiede da qualche minuto a qualche ora.

### 3. HTTPS

Quando in **Settings → Pages** compare "DNS check successful", spunta **Enforce HTTPS**. Il certificato lo crea e lo rinnova GitHub, gratis. Può volerci fino a un'ora dopo la verifica DNS.

Da qui:
- `alessandrolana.it` porta automaticamente a `https://www.alessandrolana.it`;
- i vecchi indirizzi WordPress portano alla home.

### 4. Pulizia su TopHost

Dopo il cambio DNS il vecchio WordPress non è più raggiungibile dal dominio. Svuotare la cartella `htdocs` resta comunque consigliato, per sicurezza. Il database puoi eliminarlo o lasciarlo.

### 5. Dopo la messa online

- **Google Search Console**: aggiungi la proprietà `https://www.alessandrolana.it/` e invia `sitemap.xml`.
- **Anteprime social**: prova il link su WhatsApp o con il Sharing Debugger di Facebook. Usano `og-image.jpg`.
- **Consigliato: verifica il dominio su GitHub** (Settings del tuo profilo → Pages → Add a domain). Ti chiede un record TXT da aggiungere su TopHost e impedisce a chiunque altro di usare il tuo dominio su GitHub Pages.

---

## Aggiornare il sito

Modifica o sostituisci i file dentro `site/`, dal sito di GitHub (icona matita o Upload files) o da GitHub Desktop, e fai il commit su `main`. In un minuto circa la modifica è online.

Il browser conserva i file per circa 10 minuti, quindi dopo un aggiornamento può servire un ricaricamento.
