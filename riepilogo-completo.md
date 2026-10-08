# Riepilogo completo: lavoro su menu.html

Documento di passaggio per una nuova chat. Contiene richieste, decisioni, modifiche tecniche, verifiche e punti aperti.

## 1. Contesto

- Repository: `poggioaissi/ISSI`. File principale: `menu.html` (circa 370 KB, un solo file con CSS e JavaScript inline).
- Contenuto: menù di Poggio a Issi in 5 lingue (IT, EN, FR, ES, DE), con tre pannelli (Pranzo, Cena, Bevande), carta dei vini, birre, gin, cocktail e avviso allergeni.
- Branch di lavoro: `claude/peaceful-ptolemy-k8g65g`.
- Pull request aperta: https://github.com/poggioaissi/ISSI/pull/2 (da branch a `main`, non ancora unita).
- Le altre pagine del repository (`index.html`, `giacenze.html` e simili) usano le stesse chiavi `localStorage` (`poggio-lang`, `poggio-theme`): vanno tenute invariate.

## 2. Decisioni dell'utente (valgono per il seguito)

- Il tema predefinito resta chiaro. Il sistema non segue `prefers-color-scheme`; decide l'utente se cambiarlo.
- La logica oraria resta com'è: a Cena non si vede il pannello Pranzo e viceversa, salvo il pulsante "Menù completo". È voluta perché i clienti ordinavano dal menù sbagliato.
- L'italiano è la fonte dei testi. EN, FR, ES e DE si allineano a lui.
- Le spiegazioni utili per i turisti nelle traduzioni restano (Coccole "non carne cruda", Pappardelle "tradizione toscana", aggettivi come "traditional").
- Nessun trattino lungo (U+2014) nei testi nuovi. Nel file ne esistono 45 già prima; non sono stati toccati.
- Le 5 frasi che sembravano note per il personale vanno rese neutre (fatto).

## 3. Cronologia

1. Analisi di `menu.html` su velocità, efficienza e usabilità.
2. Applicazione delle correzioni (commit `763438b`, già su `main` tramite la PR n. 1).
3. Controllo finale delle traduzioni rispetto all'italiano (commit `330a3b6`).
4. Frasi da personale rese neutre (commit `5ce54b5`).
5. Apertura della PR n. 2 con gli ultimi due commit.
6. File di riepilogo (`riepilogo-menu.md`) e questo documento.

## 4. Ottimizzazioni tecniche (commit 763438b, su main)

Velocità
- Font Google caricati senza bloccare il primo disegno (`media="print"` con `onload`) e senza il corsivo del font display.
- Rimossi `backdrop-filter` da barra fissa e chip, i `will-change` permanenti e le due `transition: all`.
- Overlay nascosti con `visibility:hidden` e ritardo di transizione.
- Listener di scroll limitato a un frame (`requestAnimationFrame`).
- Misura nel test: DOMContentLoaded da 263 ms a circa 91 ms (font bloccati nel test). Il peso compresso resta circa 77 KB: le 5 lingue nello stesso file non si possono ridurre senza file separati, scelta scartata per non complicare gli aggiornamenti.

Efficienza e codice
- Script del `<head>` con accesso protetto a `localStorage` (try/catch) esposto come `window.__ls`; imposta anche `lang` e `data-lang` su `<html>`.
- I 106 `onclick` inline sono sostituiti da attributi `data-act` e da un solo gestore di click delegato. Azioni: `lang`, `meal`, `section`, `sub`, `wd`, `close`, `backdrop`, `all`, `reset`.
- Tre script uniti in uno, in fondo alla pagina. Eliminati i wrapper a catena di `toggleSub`; logica comune per i tre popup (allergeni, suggerimento vini, suggerimento birre).
- Fascia oraria con `Intl.DateTimeFormat` creato una sola volta e `hourCycle: "h23"` (evita l'ora 24 a mezzanotte).
- Tolto il ritardo di 120 ms sul cambio lingua; `<html lang>` segue la lingua scelta.

Usabilità e accessibilità
- Rimosso `maximum-scale=1` (zoom consentito).
- `:focus-visible` con contorno visibile; Invio e Spazio sulle schede vino (`role="button"`).
- Popup con `role="dialog"`, `aria-labelledby`, focus sul pulsante, Escape per chiudere, sfondo `inert`, focus ripristinato alla chiusura.
- Aree di tocco da 44 px; `scroll-margin-top` di 124 px sulle sezioni (la barra fissa è alta circa 110 px).
- `aria-pressed` su lingua e pasto, `role="img"` sul logo, `role="group"` sui gruppi di pulsanti.

## 5. Allineamento delle traduzioni (commit 330a3b6, nella PR n. 2)

Metodo: analisi con BeautifulSoup dei gruppi di testo (`lang-it` fino a `lang-de`), confronto automatico (parole in comune, numeri) e lettura a mano riga per riga. Risultato: circa 70 righe su 140 delle schede vino non corrispondevano all'italiano. Riscritte 72 righe in EN, FR, ES, DE a partire dall'italiano (profilo sensoriale, vinificazione, abbinamento).

Errori sui piatti corretti
- Gnudi (Pranzo): FR "Boulettes" e ES "Albóndigas" (polpette) diventano "Gnudi" con spiegazione.
- La Stracciamare (tutte le lingue) e Burro e Acciughe 2.0 (EN, Pranzo e Cena): ripristinato il pesto.
- Burro e Acciughe 2.0: "brioche" sostituito con pane in cassetta, perché il brioche implica uova (allergene).
- Polpo Burger ES: "col rizada" diventa "col de Saboya" (verza).
- Issi Burger: scottona resa come "heifer beef patty" (EN), "novilla" (ES), "Färsen-Patty" (DE).
- Gran Tagliere: mostarde spiegate in FR e DE. Gin Rosa DE: "rosa Grapefruit".

Testi mancanti o non tradotti
- Titolo "Abbinamenti consigliati dal Gingegnere" aggiunto in FR, ES, DE in Hamburger, Pinse e Griglia.
- "con" delle 5 voci "G&T con ..." tradotto con spans `lang-xx`.
- "Condividi" uniforme tra Pranzo e Cena (To Share, À Partager, Para Compartir, Zum Teilen); FR "Du Grill" diventa "Du Gril".
- Correzioni di tedesco (Muschelnnudeln, Frischkäse).

## 6. Frasi da personale rese neutre (commit 5ce54b5, nella PR n. 2)

Tutte nella Carta dei Vini, dentro la scheda del vino. Riscritte in IT, EN, FR, ES, DE.

| Vino | Nuova frase italiana |
|---|---|
| Selvabianca (2 liste) | "Il vino del territorio." |
| Lumis Magnum, profilo | "Il Magnum è ideale per tavoli grandi o celebrazioni." |
| Lumis Magnum, abbinamento | "Ideale per festeggiamenti e tavoli da 4 o più persone." |
| Champagne Zero | "Un vino per palati esperti e appassionati." |
| Gewurztraminer | "Perfetto per chi ama scoprire sapori nuovi." |

## 7. Verifiche eseguite

- Test automatici con Playwright e Chromium (34 controlli superati): fasce orarie (08:00, 12:00, 20:00, 00:30 ora di Roma), tema chiaro predefinito e salvataggio della scelta, cambio lingua, accordion, tab, "Menù completo" e ripristino, popup vini, birre e allergeni (focus, Escape, sfondo inerte), tastiera sulle schede vino, `localStorage` bloccato.
- Confronto del testo mostrato nelle 5 lingue prima e dopo: invariato, tranne le righe modificate di proposito.
- Nessun id duplicato; contatore dei trattini lunghi invariato (45).
- Ambiente di prova: `/opt/node-tools/node_modules/playwright`, browser in `/opt/pw-browsers`. Gli script di prova stanno nella cartella temporanea della sessione e non sono nel repository.

## 8. Punti aperti

- Unire la PR n. 2 a `main`: il testo online cambia solo dopo il merge. Non è noto come il sito legga i file.
- L'avviso "tradotto con intelligenza artificiale" esiste solo in FR, ES e DE, non in EN (né in IT).
- Le traduzioni delle schede vino sono riviste solo da Claude, non da madrelingua.
- Vermentino Colli di Luni DOC e Melanzana alla Issi sono nascosti con `display:none` e il commento "NASCOSTO TEMPORANEAMENTE" (non disponibili).
- La battuta "altro che Malfy Rosa!" resta solo in italiano.
- Non applicato: lettura più rigida dei testi non-vino (alcune traduzioni aggiungono aggettivi come "traditional", "classic").

## 9. Per riprendere in una nuova chat

1. Apri una sessione di Claude Code su `poggioaissi/ISSI`.
2. Primo messaggio: "Leggi `riepilogo-completo.md` sul branch `claude/peaceful-ptolemy-k8g65g` e riparti da lì."
