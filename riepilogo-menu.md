# Riepilogo lavoro su menu.html

## Contesto

- Repository: `poggioaissi/ISSI`, file `menu.html` (menù di Poggio a Issi in 5 lingue: IT, EN, FR, ES, DE).
- Branch di lavoro: `claude/peaceful-ptolemy-k8g65g`.
- Pull request aperta: https://github.com/poggioaissi/ISSI/pull/2 (non ancora unita a `main`).

## Fatto

- Ottimizzazioni di velocità, efficienza e usabilità: già su `main`.
- Traduzioni EN, FR, ES, DE allineate all'italiano: 72 descrizioni vino, errori sui piatti (gnudi, pesto, pan bauletto, verza, scottona, mostarde), titoli e "G&T con" tradotti. Nella PR.
- 5 frasi da personale rese neutre nella carta dei vini (Selvabianca, Lumis Magnum x2, Champagne Zero, Gewurztraminer). Nella PR.

## Regole da mantenere

- Tema predefinito chiaro: sceglie l'utente se cambiarlo.
- La logica oraria Pranzo/Cena/Bevande con il pulsante "Menù completo" è voluta e non va toccata.
- L'italiano è la fonte dei testi.
- Nessun trattino lungo nei testi nuovi.
- Verifica con Playwright e Chromium: confronto del testo nelle 5 lingue e test delle interazioni.

## Da decidere o controllare

- Unire la PR n. 2 a `main` (il testo online cambia solo dopo il merge).
- L'avviso "tradotto con intelligenza artificiale" esiste solo in FR, ES e DE, non in EN.
- Le traduzioni delle schede vino non sono riviste da madrelingua.
- Vermentino Colli di Luni DOC e Melanzana alla Issi sono nascosti con `display:none` (non disponibili).
