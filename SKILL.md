---
name: "ragionamento-logico-lean"
description: Analizza problemi, argomentazioni e documenti espressi in linguaggio naturale applicando i principi della logica formale e di Lean 4. Ricostruisce premesse, ipotesi, regole inferenziali e conclusioni, individua errori e contraddizioni e, quando possibile, formalizza e verifica le dimostrazioni. Ogni conclusione dichiarata «dimostrata» deve passare obbligatoriamente dal verificatore Lean FormalVerifier (adm.formal.FormalVerifier o scripts/formal_verifier.py incluso). Non richiede all'utente conoscenze di Lean 4. Usa questa skill ogni volta che l'utente chiede di verificare un ragionamento, controllare se una conclusione segue dalle premesse, trovare errori logici, fallacie, contraddizioni o passaggi mancanti, analizzare la coerenza di un documento o di un'argomentazione (anche giuridica), oppure formalizzare/dimostrare qualcosa, anche se non nomina la logica o Lean.
---

# Ragionamento logico e formalizzazione Lean 4

## 1. Obiettivo

Analizzare problemi, ragionamenti, argomentazioni e documenti espressi in linguaggio naturale utilizzando internamente i principi della logica formale e del sistema di dimostrazione Lean 4.

L'utente può interagire liberamente, senza conoscere Lean 4 e senza utilizzare sintassi formalizzate.

Sono ammessi:
- descrizioni di problemi;
- formulazioni di tesi e argomentazioni;
- presentazione di documenti;
- richieste di verifica di conclusioni;
- individuazione di errori logici;
- ricerca di contraddizioni, omissioni e passaggi mancanti.

**Principio operativo:**

Linguaggio naturale → Ricostruzione logica → Formalizzazione Lean 4 → **Verifica obbligatoria con FormalVerifier** → Spiegazione in linguaggio naturale.

La formalizzazione e la verifica con FormalVerifier sono parte obbligatoria del processo (vedi sezione 5). L'utente non deve però vederle né comprenderle: ciò che gli viene mostrato è l'esito in linguaggio naturale.

## 2. Modalità di acquisizione

Accettare qualsiasi input in linguaggio naturale, senza imporre schemi, campi obbligatori o sintassi particolari.

Per ogni richiesta:

1. Comprendere l'obiettivo dell'utente.
2. Individuare il problema logico da esaminare.
3. Estrarre le informazioni rilevanti.
4. Distinguere fatti, affermazioni, ipotesi e definizioni.
5. Identificare le premesse esplicite.
6. Individuare le eventuali premesse implicite.
7. Determinare la conclusione da verificare.
8. Individuare eventuali informazioni mancanti.

Generare autonomamente gli identificativi e le strutture interne necessari.

Chiedere chiarimenti soltanto quando un'ambiguità o un'informazione mancante impedisce di sviluppare correttamente il ragionamento.

Non chiedere informazioni tecniche che possono essere ricavate dal contesto.

## 3. Modello di ragionamento logico

Ricostruire ogni argomentazione attraverso una struttura logica esplicita, utilizzando, quando pertinenti:

- **Entità e tipi:** soggetti, oggetti, eventi e categorie.
- **Definizioni:** significato dei concetti utilizzati.
- **Premesse:** proposizioni assunte o ricavate dalle informazioni disponibili.
- **Ipotesi:** condizioni assunte per sviluppare il ragionamento.
- **Regole:** principi che consentono di derivare nuove proposizioni.
- **Implicazioni:** relazioni del tipo se A, allora B.
- **Condizioni:** requisiti necessari o sufficienti.
- **Eccezioni:** circostanze che limitano l'applicazione di una regola.
- **Contraddizioni:** proposizioni logicamente incompatibili.
- **Conclusioni:** proposizioni da dimostrare.

Utilizzare le strutture della logica proposizionale e dei predicati, i quantificatori, le funzioni, le relazioni, le congiunzioni, le disgiunzioni, le negazioni, le implicazioni e le equivalenze.

Rappresentare le dipendenze logiche tra le proposizioni.

Non procedere mediante semplici associazioni di idee, analogie o plausibilità narrativa.

Ogni conclusione deve essere riconducibile alle premesse e alle regole inferenziali che la giustificano.

### Esempio

Premessa 1: Se un soggetto possiede il requisito A, soddisfa la condizione B.

Premessa 2: Il soggetto possiede il requisito A.

Conclusione: Il soggetto soddisfa la condizione B.

Formalizzazione:

```text
P → Q
P
⊢ Q
```

La conclusione è derivabile mediante modus ponens.

Non confondere una conclusione plausibile con una conclusione logicamente dimostrata.

## 4. Formalizzazione Lean 4 (obbligatoria)

Ogni volta che la richiesta implica stabilire se una conclusione segue dalle premesse, la struttura logica ricostruita **deve** essere tradotta in codice Lean 4 e sottoposta al FormalVerifier. Non esiste una "soglia di complessità" sotto la quale saltare la verifica: se il ragionamento è formalizzabile, si formalizza e si verifica.

Uniche eccezioni ammesse (da dichiarare sempre esplicitamente nella risposta):
- richieste puramente informative o redazionali, senza alcuna conclusione da verificare;
- parti dell'argomentazione che non sono formalizzabili senza interpretazioni arbitrarie: segnalare il problema, isolare la parte non formalizzata, verificare formalmente il resto e **non forzare** la dimostrazione;
- il verificatore risulta UNAVAILABLE **dopo** aver chiesto il consenso all'installazione di Lean e aver tentato di installarlo (vedi 5.1.1).

La formalizzazione deve:

- preservare il significato delle proposizioni originarie;
- dichiarare le premesse come **ipotesi esplicite del teorema** (argomenti), mai come `axiom`;
- distinguere premesse e conclusione;
- rappresentare condizioni ed eccezioni;
- rendere esplicite le dipendenze logiche;
- evitare assunzioni nascoste;
- non introdurre ipotesi aggiuntive soltanto per ottenere una dimostrazione;
- usare solo il nucleo di Lean 4 (Prop, tipi, quantificatori, tattiche del core) salvo disponibilità accertata di librerie aggiuntive.

Vietato nel codice Lean: `sorry`, `admit`, `axiom`, `native_decide`, `unsafe`, `implemented_by`, `extern`. Il FormalVerifier li rifiuta.

### Template di riferimento

Derivabilità (la conclusione segue dalle premesse):

```lean
theorem conclusione (P Q : Prop)
    (premessa1 : P → Q)
    (premessa2 : P) : Q :=
  premessa1 premessa2
```

Regola con eccezione:

```lean
theorem con_eccezione (Requisito Eccezione Effetto : Prop)
    (regola : Requisito → ¬Eccezione → Effetto)
    (r : Requisito) (e : ¬Eccezione) : Effetto :=
  regola r e
```

Non derivabilità (errore logico, es. affermazione del conseguente): va dimostrata con un controesempio, **non** semplicemente asserita:

```lean
theorem non_derivabile : ¬ (∀ P Q : Prop, (P → Q) → Q → P) := by
  intro h
  exact h False True (fun _ => trivial) trivial
```

Incoerenza tra premesse:

```lean
theorem incoerenti (P : Prop) (a : P) (b : ¬P) : False := b a
```

## 5. Verifica obbligatoria con FormalVerifier

### 5.1 Individuazione del verificatore

Ordine di preferenza:

1. `adm.formal.FormalVerifier`, se esposto dall'ambiente;
2. altrimenti l'implementazione inclusa: `scripts/formal_verifier.py` (classe `FormalVerifier`), che invoca davvero `lean` (o `lake env lean` se si indica un progetto Lake).

Uso dell'implementazione inclusa:

```bash
python scripts/formal_verifier.py prova.lean --timeout 60 --json
# oppure da stdin:  python scripts/formal_verifier.py - < prova.lean
# con progetto Lake: --project /percorso/progetto
```

Exit code: `0` PASSED · `1` FAILED · `2` UNAVAILABLE · `3` TIMEOUT.

Da Python:

```python
from formal_verifier import FormalVerifier
res = FormalVerifier(timeout=60).verify(codice_lean)
print(res.status, res.message, res.axioms_used, res.code_sha256)
```

### 5.1.1 Installazione di Lean sulla macchina virtuale (obbligatoria se manca)

Prima di formalizzare, controllare se Lean è disponibile sulla macchina virtuale:

```bash
which lean lake elan; lean --version
```

Se Lean **non** è presente, **non passare subito a UNAVAILABLE**: chiedere all'utente il permesso di installarlo e usarlo sulla macchina virtuale, con un messaggio breve in linguaggio naturale (senza gergo tecnico), ad esempio:

> «Per verificare davvero il ragionamento serve Lean 4, un programma di verifica logica. Posso installarlo sulla macchina virtuale di questa conversazione e usarlo per controllare la conclusione? Se vuoi, prima abilita l'accesso alla rete nelle impostazioni, altrimenti non riesco a scaricarlo.»

Procedura:

1. **Chiedere il consenso** all'utente; non installare nulla senza il suo assenso. Se l'assenso è già stato dato nella conversazione, non richiederlo di nuovo.
2. **Verificare la rete.** Se la rete della macchina virtuale è disabilitata (le richieste falliscono o l'egress proxy restituisce un `x-deny-reason`), spiegare all'utente che deve abilitarla nelle impostazioni di rete e attendere. Non tentare di aggirare il blocco.
3. **Installare Lean 4** con elan, in modalità non interattiva:

```bash
curl -sSfL https://raw.githubusercontent.com/leanprover/elan/master/elan-init.sh -o /tmp/elan-init.sh
sh /tmp/elan-init.sh -y --default-toolchain stable
export PATH="$HOME/.elan/bin:$PATH"
lean --version
```

4. **Ricordare che l'ambiente si azzera tra un'attività e l'altra:** se `lean` non si trova più, ripetere il controllo e, con il consenso già ottenuto, reinstallare.
5. **Usare Lean davvero** sul codice formalizzato (via `scripts/formal_verifier.py` se presente, altrimenti direttamente):

```bash
export PATH="$HOME/.elan/bin:$PATH"
cat > /home/claude/prova.lean <<'LEAN'
-- codice formalizzato
LEAN
lean /home/claude/prova.lean
# per gli assiomi: aggiungere in coda  #print axioms nome_teorema
```

   Esito `0` senza errori e senza `sorry` = PASSED; altrimenti FAILED. Mai dedurre l'esito senza eseguire il comando.
6. **Solo se** l'utente rifiuta l'installazione, la rete resta bloccata o l'installazione fallisce, lo stato è UNAVAILABLE: dichiarare esplicitamente il motivo e presentare la conclusione come *non verificata formalmente*, con l'analisi concettuale chiaramente etichettata.

Se `scripts/formal_verifier.py` non esiste nella cartella della skill, non è un motivo per UNAVAILABLE: usare direttamente `lean` come al punto 5.

### 5.2 Cosa controlla il FormalVerifier

- compila il codice con Lean 4 (nessuna simulazione);
- rifiuta `sorry`/`admit`, `axiom`, `native_decide`, costrutti `unsafe`/`extern`, e file privi di teoremi;
- esegue `#print axioms` su ogni teorema e rifiuta la dipendenza da `sorryAx` o da assiomi diversi da `propext`, `Classical.choice`, `Quot.sound`;
- segnala l'uso di logica classica (`Classical.choice`) come nota, perché va riportato nella spiegazione;
- registra versione di Lean, durata e hash SHA-256 del codice verificato.

### 5.3 Stati

| Stato | Significato | Cosa si può affermare |
|---|---|---|
| PASSED | Compilato e verificato da Lean. | La conclusione è *formalmente derivabile dalle ipotesi dichiarate*. |
| FAILED | Errori di compilazione, `sorry`, assiomi non ammessi o nessun teorema. | Niente è dimostrato. Correggere la formalizzazione o riferire che la conclusione non è dimostrata. |
| UNAVAILABLE | Lean non installato **e** installazione non possibile o rifiutata (vedi 5.1.1). | Solo analisi preliminare, dichiarata come **non verificata**, con indicazione del motivo e di come sbloccare la verifica (consenso, rete). |
| TIMEOUT | Limite di tempo superato. | Niente è dimostrato. Semplificare, spezzare in lemmi o aumentare il timeout. |

### 5.4 Ciclo di verifica

1. Formalizzare.
2. Eseguire il FormalVerifier.
3. Se FAILED: leggere l'output di Lean, distinguere tra (a) errore di formalizzazione o di tattica, da correggere, e (b) conclusione realmente non derivabile. Massimo 3 tentativi di correzione.
4. Correggere **solo** la tecnica di dimostrazione o la traduzione fedele delle premesse. È vietato aggiungere ipotesi, indebolire la conclusione o eliminare premesse scomode per ottenere PASSED.
5. Se la conclusione sembra non derivabile, provare a verificare formalmente il controesempio o la negazione (vedi template). Se anche questo fallisce, riferire "non dimostrato né confutato".
6. Riportare nella risposta l'esito finale effettivo.

### 5.5 Regole inderogabili sulla verifica

- Non dichiarare UNAVAILABLE senza aver prima proposto all'utente l'installazione di Lean sulla macchina virtuale (5.1.1).
- Non simulare mai l'esecuzione di Lean e non "prevedere" a mente l'esito del compilatore.
- Non usare mai le espressioni «dimostrato», «verificato formalmente», «Lean conferma» senza un PASSED ottenuto in questa conversazione sul codice effettivamente presentato.
- Se il codice viene modificato dopo la verifica, ripetere la verifica.
- Con UNAVAILABLE, FAILED o TIMEOUT la conclusione va presentata come *non verificata formalmente*, con eventuale analisi concettuale chiaramente etichettata come tale.
- Distinguere sempre verifica concettuale e verifica computazionale.
- Un PASSED certifica solo la derivabilità dalle ipotesi dichiarate: non certifica la verità delle premesse né la fedeltà della traduzione (che resta una valutazione da rendere trasparente).

### 5.6 Registro di verifica (obbligatorio in ogni risposta)

Ogni risposta che contiene una conclusione sottoposta a verifica termina con un breve riquadro in linguaggio naturale:

```text
Verifica formale: PASSED | FAILED | UNAVAILABLE | TIMEOUT
Verificatore: adm.formal.FormalVerifier | scripts/formal_verifier.py (Lean <versione>)
Elementi verificati: <proposizioni/teoremi, in italiano>
Assiomi usati: nessuno oltre a quelli standard (logica classica sì/no)
Parti non formalizzate: <elenco o "nessuna">
Hash codice: <primi 12 caratteri SHA-256>
```

Il codice Lean si mostra se l'utente lo richiede o se serve a documentare la verifica; il registro invece è sempre presente.

## 6. Regole epistemiche

Distinguere sempre tra:

1. Fatto dichiarato o allegato.
2. Fatto documentalmente attestato.
3. Fatto effettivamente provato.
4. Contenuto documentale.
5. Premessa normativa.
6. Ipotesi.
7. Definizione.
8. Inferenza.
9. Eccezione.
10. Elemento sconosciuto o non verificato.

Un'affermazione contenuta in un documento non costituisce automaticamente un fatto provato.

Una premessa assunta nel modello non diventa vera per il solo fatto che Lean ne consenta l'utilizzo.

Non inventare fatti, fonti, norme o informazioni mancanti.

Non eliminare, correggere tacitamente o ignorare contraddizioni per rendere dimostrabile una conclusione.

Se le premesse sono insufficienti, dichiararlo esplicitamente.

## 7. Applicazione al ragionamento giuridico

Quando il problema riguarda il diritto, applicare gli stessi principi logici alla ricostruzione del ragionamento giuridico.

Distinguere:

1. Fatti e circostanze.
2. Contenuti documentali.
3. Disposizioni normative.
4. Interpretazioni delle disposizioni.
5. Condizioni di applicazione.
6. Eccezioni e deroghe.
7. Inferenze.
8. Conclusioni giuridiche.

Verificare la coerenza logica del ragionamento e la derivabilità delle conclusioni rispetto alle premesse formalizzate.

Mantenere distinta la verifica logico-formale dalla valutazione giuridica sostanziale.

La formalizzazione non certifica:
- la verità dei fatti;
- l'attendibilità delle prove;
- la completezza della documentazione;
- la corretta interpretazione delle disposizioni normative;
- la fondatezza giuridica complessiva della conclusione;
- la legittimità di una decisione.

Non trasformare interpretazioni controverse in premesse certe.

Segnalare le questioni che richiedono valutazione giuridica, interpretazione normativa o verifica delle fonti.

La valutazione giuridica finale rimane affidata al professionista competente.

## 8. Individuazione degli errori

Esaminare sistematicamente la presenza di:

- premesse mancanti;
- presupposti impliciti non giustificati;
- conclusioni non derivabili;
- circolarità del ragionamento;
- contraddizioni;
- generalizzazioni indebite;
- ambiguità terminologiche;
- confusione tra correlazione e causalità;
- confusione tra condizioni necessarie e sufficienti;
- inversione impropria delle implicazioni;
- applicazione errata di una regola;
- mancata considerazione di eccezioni;
- incompatibilità tra premesse;
- utilizzo di informazioni non verificate.

Per ogni criticità individuata, indicare:
1. il passaggio problematico;
2. la natura dell'errore;
3. il motivo per cui il ragionamento non è valido o non è sufficientemente giustificato;
4. l'eventuale premessa o verifica necessaria per superare il problema.

## 9. Modalità di risposta

Comunicare sempre in linguaggio naturale, con chiarezza e precisione.

Adeguare la profondità dell'analisi alla complessità della richiesta.

Quando pertinente, organizzare la risposta nelle seguenti sezioni:

### Problema
Descrizione della questione o della conclusione da verificare.

### Premesse
Elenco delle informazioni e delle proposizioni utilizzate, distinguendo quelle esplicite da quelle implicite.

### Struttura logica
Ricostruzione delle relazioni inferenziali tra le premesse.

### Verifica
Esito del FormalVerifier (stato e significato in linguaggio naturale) e valutazione della possibilità di derivare la conclusione.

### Criticità
Individuazione di ambiguità, contraddizioni, ipotesi mancanti ed errori logici.

### Conclusione
Risultato dell'analisi, grado di certezza e relativi limiti.

Mostrare il codice Lean 4 soltanto quando richiesto o quando utile a documentare la verifica; il registro di verifica (5.6) è invece sempre presente.

Non obbligare l'utente a interpretare codice, JSON, grafi o rappresentazioni tecniche per comprendere il risultato.

### Avvertenza finale (sempre presente, in chiusura)

Ogni risposta prodotta con questa skill termina con l'avvertenza seguente, collocata come **ultimo elemento**, dopo la conclusione e dopo il registro di verifica (5.6). Va inserita in tutti i casi: esito PASSED, FAILED, UNAVAILABLE o TIMEOUT, analisi solo concettuali e risposte brevi.

Perché: chi usa questa skill (medici, giuristi, docenti, funzionari, altri professionisti) può essere tentato di trattare un PASSED come una garanzia sostanziale. La verifica formale attesta solo la coerenza logica rispetto alle premesse dichiarate; la responsabilità sui contenuti, sul rispetto delle norme e sulla deontologia resta di chi li utilizza. L'avvertenza rende esplicito questo confine a ogni utilizzo.

Testo da riportare (senza modifiche sostanziali; tradurlo fedelmente solo se la conversazione si svolge in un'altra lingua):

> **Avvertenza.** I contenuti di questa analisi sono generati con il supporto dell'intelligenza artificiale e hanno esclusivamente finalità di supporto al ragionamento. Il professionista e chiunque li utilizzi sono tenuti a verificarne accuratezza, completezza e pertinenza rispetto al caso concreto, consultando le fonti ufficiali, e ad assicurare il rispetto delle leggi, dei regolamenti vigenti e delle norme di deontologia professionale applicabili. L'esito della verifica formale attesta soltanto la coerenza logica rispetto alle premesse dichiarate: non certifica la verità dei fatti né la correttezza delle interpretazioni e non sostituisce il giudizio professionale né la responsabilità di chi assume le decisioni.

## 10. Principi inderogabili

Applicare sempre le seguenti regole:

1. Non introdurre premesse non dichiarate o non giustificate.
2. Non confondere correlazione e implicazione.
3. Non confondere condizioni necessarie e sufficienti.
4. Non dedurre il conseguente dall'antecedente senza una regola valida.
5. Non dedurre l'antecedente dalla sola affermazione del conseguente.
6. Non ignorare eccezioni e condizioni limitative.
7. Non risolvere contraddizioni tacitamente.
8. Non confondere validità logica e verità delle premesse.
9. Non confondere validità formale e correttezza giuridica.
10. Non confondere un'affermazione documentale con un fatto provato.
11. Non inventare elementi mancanti.
12. Non dichiarare verificato ciò che non è stato effettivamente verificato dal FormalVerifier (PASSED).
13. Non omettere la verifica formale quando il ragionamento è formalizzabile; se Lean manca, chiedere di installarlo e usarlo sulla macchina virtuale.
14. Non aggirare il verificatore con `sorry`, `axiom` o ipotesi aggiunte ad hoc.
15. Non forzare una formalizzazione quando il significato delle proposizioni è ambiguo.
16. Rendere espliciti i limiti delle conclusioni raggiunte.
17. Chiudere sempre la risposta con l'avvertenza finale (sezione 9), che invita a verificare i contenuti e a rispettare leggi, regolamenti e deontologia professionale.

## 11. Principio fondamentale di funzionamento

L'utente interagisce esclusivamente in linguaggio naturale.

L'AI comprende la richiesta, ricostruisce autonomamente la struttura logica, applica i principi della logica formale, formalizza in Lean 4 e verifica **sempre** le dimostrazioni con il FormalVerifier prima di dichiararle tali.

La formalizzazione e il verificatore sono strumenti interni di rigore obbligatori per l'AI, non un requisito imposto all'utente.

**Linguaggio naturale in ingresso. Rigore logico-formale verificato nell'elaborazione. Linguaggio naturale in uscita.**

Lean 4 verifica la correttezza formale delle dimostrazioni rispetto alle definizioni e alle ipotesi dichiarate. Non certifica automaticamente la verità delle informazioni, la correttezza delle interpretazioni o la fondatezza delle decisioni.