# Sito test Google Analytics

Pagina HTML statica per provare l'integrazione diretta di Google Analytics 4
tramite il Google tag (`gtag.js`), seguendo lo snippet fornito.

## File del progetto

- `index.html`: pagina di test con il tag nel `<head>`.
- `_config.yml`: configurazione Jekyll presente nel progetto. La pagina HTML
  attuale non usa un layout Jekyll e contiene direttamente il tag.
- `readme.md`: istruzioni del progetto.

Non sono necessari dipendenze, installazione o compilazione.

## Configurazione del tag

L'ID di misurazione attualmente configurato è `G-6RK02D9042`.
Il blocco compare una sola volta, subito dopo l'apertura di `<head>` e quindi
prima di `</head>`:

```html
<!-- Google tag (gtag.js) -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-6RK02D9042"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-6RK02D9042');
</script>
```

Per cambiare proprietà, sostituisci l'ID sia nell'URL dello script sia nella
chiamata `gtag('config', ...)`. Verifica in Google Analytics, in
**Amministrazione → Stream di dati → stream Web**, che l'ID corrisponda alla
proprietà desiderata: il solo codice del sito non consente di confermarlo.

Questa configurazione usa direttamente il Google tag per GA4. Un contenitore
Google Tag Manager identificato da `GTM-...` richiede la propria configurazione.
Evita di aggiungere un secondo tag per la stessa proprietà senza verificare
eventuali duplicazioni degli eventi.

## Avvio locale

Dalla cartella del progetto, con Python 3 installato:

```sh
python3 -m http.server 8000 --bind 127.0.0.1
```

Apri [http://127.0.0.1:8000](http://127.0.0.1:8000) e interrompi il server con
`Ctrl+C` al termine della prova. Anche le visite locali possono inviare eventi
alla proprietà configurata.

## Verifica del tracciamento

1. Apri la pagina servita in HTTP oppure la versione pubblicata.
2. Negli strumenti sviluppatore del browser, apri **Network** e ricarica.
3. Cerca `gtag/js?id=G-6RK02D9042` e verifica che lo script venga caricato.
4. Cerca le richieste Analytics `g/collect`: verifica l'ID `tid=G-6RK02D9042`
   e l'evento `en=page_view`. Il caricamento dello script, da solo, non conferma
   l'invio degli eventi.
5. Usa [Tag Assistant](https://tagassistant.google.com/) sul sito pubblicato e
   controlla il report **In tempo reale** nella proprietà Google Analytics.

Se non vedi richieste, controlla gli errori nella console, la connessione e
gli eventuali blocchi delle estensioni del browser. La visualizzazione degli
eventi nei report richiede anche la corretta configurazione della proprietà.

## Pubblicazione

Il repository Git configurato è `Graclich/Graclich.github.io`. Le modifiche
locali devono essere incluse in un commit, inviate al repository e pubblicate
dal flusso GitHub Pages configurato prima di essere visibili online.

## Riferimenti

- [Installazione del Google tag](https://developers.google.com/tag-platform/gtagjs)
- [Configurazione ed esempi gtag.js](https://developers.google.com/tag-platform/gtagjs/configure)
- [Configurazione di Google Analytics](https://support.google.com/analytics/answer/9304153?hl=it)
