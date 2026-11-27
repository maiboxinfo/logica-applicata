# Ragionamento logico e formalizzazione Lean 4

Nov 27, 2026 · @Gaetano Gnasso

| Voce | Dettaglio |
| --- | --- |
| Autore | Ing. Gaetano Gnasso |
| Licenza | MIT License |
| Tipologia | Skill per intelligenza artificiale – analisi logica, formalizzazione e verifica delle dimostrazioni con Lean 4 |

## 1. Descrizione

La skill **Ragionamento logico e formalizzazione Lean 4** (`ragionamento-logico-lean`) è un progetto sviluppato dall'Ing. Gaetano Gnasso per migliorare il rigore logico, la coerenza argomentativa e l'affidabilità delle analisi prodotte dai sistemi di intelligenza artificiale.

La skill è ottimizzata per Claude. Serve ad analizzare problemi, documenti, argomentazioni e ragionamenti scritti in linguaggio naturale, compresi i testi giuridici. Applica i principi della logica formale e usa Lean 4, un linguaggio di programmazione e dimostratore interattivo di teoremi, per formalizzare le dimostrazioni e verificarle al computer. Si attiva automaticamente quando l'utente chiede di controllare un ragionamento, di capire se una conclusione segue dalle premesse, di trovare fallacie, contraddizioni o passaggi mancanti, o di valutare la coerenza di un testo. Non serve nominare la logica o Lean.

L'obiettivo è rendere accessibili strumenti avanzati di verifica logica anche a chi non conosce la programmazione o la logica matematica formale. Tutta la parte tecnica resta interna alla skill. L'utente scrive e legge solo in linguaggio naturale, e il codice Lean viene mostrato soltanto su richiesta o quando serve a documentare la verifica.

### 1.1 Principio di funzionamento

1. **Linguaggio naturale.** La skill capisce l'obiettivo dell'utente e individua il problema logico da risolvere.
2. **Ricostruzione logica.** Distingue fatti, affermazioni, ipotesi e definizioni. Individua le premesse esplicite e quelle implicite, la conclusione e le informazioni mancanti. Poi costruisce la struttura del ragionamento: entità, regole, condizioni, eccezioni ed eventuali contraddizioni. Chiede chiarimenti solo quando un'ambiguità impedisce davvero di andare avanti.
3. **Formalizzazione Lean 4.** Le premesse diventano ipotesi di un teorema e la conclusione diventa la tesi da dimostrare. Non si introducono assiomi aggiuntivi.
4. **Verifica formale.** Il codice viene compilato e controllato dal verificatore. Se la verifica fallisce, la skill corregge la tecnica di dimostrazione o la traduzione, fino a un massimo di tre tentativi.
5. **Spiegazione in linguaggio naturale.** Il risultato viene riportato in forma chiara e comprensibile, con la distinzione tra ciò che è stato dimostrato e ciò che resta incerto o non formalizzabile.

### 1.2 Regole di integrità della formalizzazione

Per evitare dimostrazioni solo apparenti, la skill segue vincoli rigorosi:

- il codice non può contenere scorciatoie o costrutti che aggirano il controllo (`sorry`, `admit`, `axiom`, `native_decide`, `unsafe`, `implemented_by`, `extern`);
- è vietato aggiungere ipotesi per far riuscire una dimostrazione, indebolire la conclusione o eliminare premesse scomode;
- per affermare che una conclusione *non* segue dalle premesse bisogna dimostrarlo con un controesempio;
- le parti che non si possono formalizzare senza interpretazioni arbitrarie vengono isolate e segnalate esplicitamente.

### 1.3 Il verificatore formale

Ogni conclusione presentata come «dimostrata» deve passare dal verificatore **FormalVerifier**. La skill usa `adm.formal.FormalVerifier`, se disponibile, oppure richiama direttamente `lean` o `lake`. Se Lean non è installato, la skill chiede il consenso dell'utente prima di installarlo tramite `elan` e non procede mai di nascosto.

Il verificatore compila il codice, rifiuta i costrutti vietati, controlla quali assiomi sono stati usati e registra la versione di Lean, la durata e l'impronta (hash) del codice. L'esito può essere di quattro tipi:

| Esito | Significato |
|---|---|
| **PASSED** | La conclusione deriva dalle ipotesi dichiarate |
| **FAILED** | Nulla è stato dimostrato |
| **TIMEOUT** | Tempo scaduto: nulla è stato dimostrato |
| **UNAVAILABLE** | Verificatore non disponibile: l'analisi è solo preliminare e non verificata |

La skill non simula mai l'esito di Lean. Usa la parola «dimostrato» solo dopo un PASSED ottenuto nella stessa conversazione sul codice effettivamente compilato. Se il codice cambia, la verifica va ripetuta.

### 1.4 Struttura della risposta

Le analisi sono organizzate in sezioni: **Problema, Premesse, Struttura logica, Verifica, Criticità, Conclusione**. Ogni risposta si chiude con un **registro della verifica formale** obbligatorio, che riporta:

- l'esito della verifica;
- il verificatore usato e la versione di Lean;
- gli elementi verificati;
- gli assiomi usati, indicando se si è fatto ricorso alla logica classica;
- le parti non formalizzate;
- i primi 12 caratteri dell'hash SHA-256 del codice, per la tracciabilità.

### 1.5 Ambito giuridico

Sui testi giuridici la skill distingue fatti, documenti, norme, interpretazioni, eccezioni e conclusioni. Tiene separata la verifica logico-formale, cioè se la conclusione segue dalle premesse, dalla valutazione giuridica di merito, che spetta sempre al professionista competente.

### 1.6 Limiti e avvertenze

Un esito PASSED attesta solo la **coerenza logica** della conclusione rispetto alle premesse dichiarate. Non certifica che le premesse siano vere, che la traduzione dal linguaggio naturale sia fedele, che la documentazione sia completa, che le interpretazioni siano corrette o che le decisioni siano legittime.

Per questo ogni risposta termina con un'avvertenza. Ricorda che l'analisi è uno strumento di supporto, che i contenuti vanno controllati sulle fonti ufficiali e che vanno rispettati leggi, regolamenti e deontologia professionale.


## 2. Licenza MIT – Libertà di utilizzo

Questa skill è distribuita con licenza MIT (MIT License), una licenza open source permissiva che consente un'ampia libertà di utilizzo, modifica e redistribuzione.

La licenza MIT permette a chiunque di:

- utilizzare liberamente la skill, anche per finalità commerciali;
- installarla e integrarla in sistemi di intelligenza artificiale;
- modificarne il contenuto e adattarlo alle proprie esigenze;
- realizzare versioni personalizzate o opere derivate;
- incorporarla in progetti software proprietari o open source;
- distribuirla nuovamente, anche a pagamento;
- integrarla in prodotti, servizi e applicazioni commerciali.

Non è necessario richiedere preventivamente un'autorizzazione all'autore per gli utilizzi consentiti dalla licenza.

## 3. Obblighi di attribuzione dell'autore

La libertà di utilizzo concessa dalla licenza MIT non elimina l'obbligo di conservare l'avviso di copyright e il testo della licenza in tutte le copie o parti sostanziali del software.

È inoltre fortemente raccomandato, per correttezza intellettuale e trasparenza, citare l'autore originale quando la skill viene utilizzata, modificata, integrata o redistribuita.

Attribuzione dell'opera:

> **Ragionamento logico e formalizzazione Lean 4** Autore originale: Ing. Gaetano Gnasso Licenza: MIT License

L'attribuzione non implica che l'autore approvi, certifichi o garantisca le modifiche apportate da terzi. Chi distribuisce una versione modificata dovrebbe indicarlo chiaramente, per esempio con la dicitura «Versione modificata da \[nome\], basata sull'opera originale dell'Ing. Gaetano Gnasso».

## 4. Come citare la skill

Per documentazione tecnica, repository, pubblicazioni, progetti di ricerca o applicazioni che utilizzano la skill, si consiglia una delle seguenti forme di citazione.

**Citazione breve** (README, crediti, interfacce):

> Ragionamento logico e formalizzazione Lean 4 – Ing. Gaetano Gnasso – MIT License

**Citazione estesa** (documentazione, relazioni, pubblicazioni):

> Gnasso, G. (2026). *Ragionamento logico e formalizzazione Lean 4* \[Skill per intelligenza artificiale\]. Distribuita con licenza MIT.

**Formato BibTeX** (articoli e tesi):

```text
@software{gnasso_ragionamento_lean_2026,
  author  = {Gnasso, Gaetano},
  title   = {Ragionamento logico e formalizzazione Lean 4},
  year    = {2026},
  note    = {Skill per intelligenza artificiale},
  license = {MIT}
}
```

Se la skill è pubblicata in un repository, aggiungere l'indirizzo (URL) e la versione utilizzata.

## 5. Esclusione di garanzia e responsabilità d'uso

La skill è fornita «così com'è», senza garanzie di alcun tipo, come previsto dalla licenza MIT.

La verifica con Lean 4 attesta soltanto che una conclusione è derivabile dalle premesse dichiarate. Non certifica:

- la verità dei fatti o delle premesse;
- la fedeltà della traduzione dal linguaggio naturale alla formalizzazione;
- la corretta interpretazione di norme, linee guida o documenti;
- la fondatezza giuridica, clinica o tecnica delle conclusioni.

I contenuti prodotti hanno finalità di supporto al ragionamento. Il professionista e chiunque li utilizzi sono tenuti a verificarne accuratezza, completezza e pertinenza rispetto al caso concreto, consultando le fonti ufficiali, e ad assicurare il rispetto delle leggi, dei regolamenti vigenti e delle norme di deontologia professionale applicabili. Le decisioni e le relative responsabilità restano in capo a chi le assume.

## 6. Testo integrale della licenza MIT

Il testo ufficiale della licenza è in inglese e va riportato senza modifiche in un file `LICENSE` incluso con la skill.

```text
MIT License

Copyright (c) 2026 Gaetano Gnasso

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
