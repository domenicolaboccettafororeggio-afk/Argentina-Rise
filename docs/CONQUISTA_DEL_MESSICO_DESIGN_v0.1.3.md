# Conquista del Messico - Design tecnico e game design v0.1.3

Documento di progettazione, non implementazione. Nessun file di gioco e stato modificato. Millennium Dawn e stato consultato esclusivamente in lettura.

## 1. Lista stati approvata e verificata

La lista definitiva approvata e `828-840`:

`828`, `829`, `830`, `831`, `832`, `833`, `834`, `835`, `836`, `837`, `838`, `839`, `840`.

Verifica eseguita il 2026-07-11 contro i file correnti in `E:\Steam\steamapps\workshop\content\394360\2777392649\history\states`:

| ID | Stato MD | Owner iniziale |
|---:|---|---|
| 828 | Baja California | `MEX` |
| 829 | Sonora | `MEX` |
| 830 | Chihuahua | `MEX` |
| 831 | Sinaloa-Durango | `MEX` |
| 832 | Nuevo Leon | `MEX` |
| 833 | Coahuila | `MEX` |
| 834 | Guanajuato | `MEX` |
| 835 | Mexico | `MEX` |
| 836 | Jalisco | `MEX` |
| 837 | Guerrero-Oaxaca | `MEX` |
| 838 | Veracruz | `MEX` |
| 839 | Chiapas | `MEX` |
| 840 | Yucatan | `MEX` |

La lista corrisponde all'intero territorio messicano definito nella storia iniziale di Millennium Dawn. Il precedente blocco relativo a `1892` e `282` e quindi risolto.

## 2. Reverse engineering: pattern esistenti

### Submod Argentina Rise

| Meccanica | Pattern | File e riferimento | Vantaggi / limite |
|---|---|---|---|
| Claim pre-conflitto | scope diretto stato `add_claim_by = ARG` | `common/national_focus/ARG_rise_focus_tree.txt:8381-8396` | Semplice e leggibile; non concede il war goal da solo |
| War goal coercitivo | `create_wargoal { type = puppet_wargoal_focus target = TAG }` | `common/decisions/ARG_rise_buenos_aires_conference_decisions.txt:226-230`; focus circa 8214-8223 | Gia usato ARG; adatto a soggezione, non a conquista richiesta |
| Check post-conquista | `is_owned_by = ARG is_controlled_by = ARG` | Cuba, `focus_tree.txt:8411-8417, 8461-8467` | Evita reward prima del controllo effettivo; il pattern Cuba accetta anche controllo parziale |
| Ricostruzione | dynamic modifier statale + slot + infrastruttura | Cuba, `8422-8445`; Caraibi, `8534-8544` | Riusa `ARG_rise_plan_reconstruccion_austral_state_modifier`; richiede stato controllato |
| Integrazione e core | `add_core_of = ARG`, `remove_claim_by = TAG`, building e risorse | Cuba, `8472-8500` | Pattern diretto, per-state, idempotente se protetto da controllo |
| Core multipli | `every_state` filtrato per core originario | `focus_tree.txt:8317-8337`; evento `ARG_rise.74`, `events/ARG_rise_events.txt:1305-1332` | Scalabile; richiede filtrare correttamente il paese, non ID errati |
| Annessione narrativa | `annex_country { target = TAG transfer_troops = no }` | `events/ARG_rise_events.txt:981-988, 1048-1080, 1137-1157, 1213-1253` | Usato per esiti diplomatici; non va usato per sostituire una guerra reale |

### Millennium Dawn

| Meccanica | Pattern | File assoluto | Vantaggi / limite |
|---|---|---|---|
| Guerra di annessione completa | `create_wargoal { target = URG type = annex_everything }` | `E:\Steam\steamapps\workshop\content\394360\2777392649\events\Brazilian.txt:1257-1264` | Corrisponde all'obiettivo "conquista"; piu appropriato di `puppet_wargoal_focus` |
| Core territoriali dopo campagna | `every_owned_state -> add_core_of = BRA` | `E:\Steam\steamapps\workshop\content\394360\2777392649\events\Brazilian.txt:1261-1264` | Pattern esistente MD; deve essere post-conquista, non nel focus di guerra |
| War goal mirato | `take_claimed_state` / `annex_everything` | `E:\Steam\steamapps\workshop\content\394360\2777392649\common\national_focus\05_afghanistan.txt:5186-5201` | Evidenzia la differenza fra rivendicazione limitata e annessione totale |

## 3. Pattern consigliati

1. **Focus 1:** claim sugli stati messicani confermati + evento presidenziale + sblocco della decisione di ultimatum. Il war goal non e un reward diretto del focus: viene concesso solo dopo il rifiuto messicano. Il tipo previsto resta `annex_everything`, non il pattern ARG di puppetizzazione.
2. **Focus 2:** condizione rigorosa di conquista nazionale, non un singolo stato. Riutilizza lo schema Cuba di ricostruzione per gli stati messicani controllati e il modifier `ARG_rise_plan_reconstruccion_austral_state_modifier` gia definito nella submod.
3. **Focus 3:** core solo dopo Focus 2 e dopo controllo/possesso dei singoli stati. Riutilizza il pattern `add_core_of = ARG` + `remove_claim_by = MEX` per scope statale.

## 4. Struttura dei tre focus

### 4.1 La Marcha hacia Mexico

**Funzione:** rendere la guerra una decisione politica e strategica, non un bottone improvviso.

- Prerequisite: `ARG_rise_ciudadania_de_la_union_continental`.
- Reward tecnico: claim sugli stati messicani confermati; evento presidenziale; sblocco della decisione `Inviare l'Ultimatum a Citta del Messico`. Il war goal non viene creato qui.
- Bypass: MEX non esiste, e soggetto ARG o ha capitolato. Il bypass preciso deve seguire il modello `OR = { MEX = { is_subject_of = ARG } MEX = { has_capitulated = yes } NOT = { country_exists = MEX } }` gia usato per Colombia e altri target.
- Non usare annessione istantanea: l'obiettivo richiede una campagna.

### 4.2 La Integracion de Mexico

**Funzione:** passare dalla vittoria militare alla ripresa amministrativa.

- Prerequisite: `La Marcha hacia Mexico`.
- Available: tutti gli stati messicani `828-840` devono essere posseduti e controllati da ARG. Per una conquista completa, il gate non deve usare l'`OR` cubano.
- Reward tecnico consigliato: per ogni stato messicano controllato, applicare il dynamic modifier di ricostruzione gia esistente e il pacchetto Cuba/Caraibi di infrastruttura, slot condiviso e industrializzazione dove il pattern e gia presente.
- Risorse: usare `add_resource` per stato, non un bonus nazionale. Le risorse devono seguire la geografia MD: Veracruz e Nuevo Leon sono gia ricchi di petrolio; Jalisco/Mexico/Guanajuato hanno acciaio; il documento non fissa numeri nuovi.
- Evento: fine della guerra / inizio ricostruzione.

### 4.3 Una Nacion, Dos Pueblos

**Funzione:** trasformare il controllo in cittadinanza e integrazione permanente.

- Prerequisite: `La Integracion de Mexico`.
- Available: stessi stati confermati posseduti e controllati da ARG.
- Reward tecnico: `add_core_of = ARG` e `remove_claim_by = MEX` per ciascuno degli stati `828-840`; evento di integrazione della popolazione.

## 5. Catena eventi proposta

Nessun testo e definito qui; sono solo nodi e trigger.

1. **Il mandato verso il nord**: chiamato da Focus 1. Presenta il disegno continentale e rende disponibile l'ultimatum, senza assegnare ancora il war goal.
2. **La risposta di Citta del Messico**: chiamato dalla decisione di ultimatum sullo scope `MEX`. Il rifiuto e l'unico esito narrativo; il suo effetto consegna il war goal ad ARG.
3. **Inizio dell'offensiva argentina**: trigger al primo stato di guerra ARG-MEX, con guardia anti-duplicazione tramite flag.
4. **Caduta di Citta del Messico**: trigger quando lo stato `835` e controllato da ARG, con flag one-shot. Non implica fine della guerra.
5. **Fine della guerra**: trigger quando MEX capitola/non esiste e gli stati confermati sono posseduti e controllati da ARG. Sblocca narrativamente Focus 2.
6. **Integrazione dei due popoli**: chiamato dal Focus 3 dopo l'assegnazione dei core; e il climax civile, non militare.

## 6. Reward: criteri, non valori inventati

| Obiettivo | Pattern da riutilizzare | Fonte |
|---|---|---|
| Capacita industriale | `add_extra_state_shared_building_slots` + `industrial_complex` | Caraibi, `focus_tree.txt:8541-8544` |
| Costruzioni | `infrastructure level = 1 instant_build = yes` | Cuba, `8425-8443`, `8477-8498` |
| Ricostruzione | `ARG_rise_plan_reconstruccion_austral_state_modifier` | Cuba/Caraibi |
| Acciaio/light metals/petrolio | `add_resource { type = steel/aluminium/oil amount = ... }` per stato | Cuba, `8479-8498`; stati MD Messico |
| Integrazione popolazione | `add_core_of = ARG` | Cuba, `8475-8496`; Unione continentale, `8330` |

I valori devono essere scelti in fase implementativa copiando una scala gia presente nella submod e rapportandola ai 13 stati corretti. Non esiste una giustificazione tecnica per fissarli ora senza la lista finale.

## 7. Layout consigliato

Ancora verificata: `ARG_rise_la_conferencia_de_buenos_aires` e a `x=7, y=39`; Cuba e a `x=13, y=39-41`.

| Focus | x | y | Posizione |
|---|---:|---:|---|
| La Marcha hacia Mexico | 4 | 39 | sinistra di Conferencia, stessa altezza del primo nodo regionale |
| La Integracion de Mexico | 4 | 40 | sotto il focus di guerra |
| Una Nacion, Dos Pueblos | 4 | 41 | chiusura civile del ramo |

Il ramo e una colonna parallela a Cuba. Nessuna mutually exclusive e consigliata: la campagna messicana amplia, non sostituisce, la Conferencia. Il prerequisite comune `ARG_rise_ciudadania_de_la_union_continental` colloca il ramo dopo l'integrazione sudamericana e prima delle operazioni caraibiche.

## 8. File e dipendenze dell'implementazione futura

### File submod da modificare/creare

- `common/national_focus/ARG_rise_focus_tree.txt`: tre focus e layout.
- `events/ARG_rise_events.txt`: eventi presidenziali, di risposta, di guerra, di conquista, di integrazione e news internazionali; namespace esistente.
- `common/decisions/`: un file decisioni argentino dedicato oppure un'estensione del file argentino gia pertinente, solo per la decisione di ultimatum e le sue condizioni di visibilita/rimozione.
- `common/on_actions/ARG_rise_on_actions.txt`: estensione del pattern gia esistente, necessaria per l'evento one-shot sulla presa dello stato `835`.
- `localisation/english/ARG_rise_l_english.yml`: titoli, descrizioni, tooltip, eventi, opzioni e decisione.
- Eventuale `interface/ARG_rise_goals.gfx` e DDS sotto `gfx/interface/goals/`: solo se si richiedono icone dedicate; altrimenti riusare icone MD esistenti.

### File da non modificare

- Nessun file in `E:\Steam\steamapps\workshop\content\394360\2777392649`.
- Nessun override di building, energia, resource definition o war goal MD.

### Dipendenze MD

- `annex_everything`, scope di guerra e capitolazione.
- `add_core_of`, `remove_claim_by`, `add_resource`, `add_building_construction`.
- Stati MEX in `E:\Steam\steamapps\workshop\content\394360\2777392649\history\states\828-840*.txt`.
- Treasury via `modify_treasury_effect` gia usato dalla submod, se si conserva un costo politico/economico.

## 9. Rischi

1. Guerra totale contro MEX ha scala molto superiore a Cuba/Caraibi; il prerequisite deve essere tardivo e il gate post-guerra rigoroso.
2. Eventi 2-5 richiedono trigger affidabili e flag one-shot; un evento programmato con giorni fissi non sostituisce il controllo dello stato di guerra.
3. Core permanenti moltiplicano manpower, industria e risorse: non usarli nel Focus 2.
4. Ogni nuova idea permanente richiederebbe valutazione del restore post-guerra civile; questo piano non ne richiede una.

## 10. Piano di implementazione futuro

1. Verificare in MD il tag MEX, stati e condizione di conquista in una partita pulita.
2. Aggiungere i tre focus copiando lo stile Cuba/Conferencia.
3. Aggiungere decisione, eventi, news e localizzazioni, con flag anti-duplicazione.
4. Estendere l'on-action solo per il trigger territoriale `835`.
5. Verificare layout nel focus tree e assenza di overlap.
6. Playtest: decisione, rifiuto, war goal, guerra, controllo di 835, capitolazione, Focus 2, Focus 3, core e risorse.
7. Solo dopo il playtest, creare GFX dedicata se le icone riusate non comunicano abbastanza la campagna.

## Esito della progettazione

La struttura e tecnicamente compatibile con i pattern gia presenti. La soluzione non richiede un sistema nuovo: combina claims/Cuba, ricostruzione Cuba-Caraibi, core multipli Unione continentale e war goal `annex_everything` MD. La lista stati `828-840` e confermata e verificata.

## 11. Raffinamento Lead Game Designer

### 11.1 Epicita dei tre focus

La struttura a tre atti e corretta: **mandato continentale**, **vittoria amministrata**, **unione civile**. Tuttavia, nella sua formulazione corrente il primo focus appare ancora soprattutto tecnico, perche consegna claim e war goal nello stesso gesto. Il giocatore capisce cosa puo fare, ma non sente ancora il peso della scelta.

La correzione non e aggiungere bonus o allungare la colonna: e far si che il primo focus sia il momento in cui l'Argentina dichiara di voler ridefinire l'equilibrio delle Americhe. La guerra deve sembrare il risultato inevitabile di una visione politica, non l'effetto di un tooltip. I focus due e tre hanno invece gia una funzione narrativa forte: impediscono che la campagna finisca alla capitolazione e trasformano la conquista in responsabilita statale.

Valutazione: **epica potenziale alta, epica attuale media**. L'epica va costruita negli eventi e nella presentazione, non tramite un quarto focus riempitivo.

### 11.2 Catena narrativa consigliata

La catena di cinque eventi esistente e una buona base, ma deve funzionare come una sequenza di soglie e non come una cronaca amministrativa.

1. **Il mandato verso il nord**: chiamato dal Focus 1. Il Presidente espone la scelta; il giocatore avverte che il continente sta osservando.
2. **L'ultimatum di Buenos Aires**: un breve passaggio diplomatico immediatamente precedente o contestuale alla guerra. Il Messico rifiuta, oppure la rottura diventa inevitabile. Non deve offrire una falsa scelta che annulla il ramo.
3. **L'offensiva continentale**: al primo conflitto ARG-MEX. Deve chiarire che non e una guerra di frontiera, ma una prova della capacita argentina di proiettare potere oltre il proprio spazio naturale.
4. **La caduta di Citta del Messico**: allo stato `835` controllato da ARG. E il climax militare, ma non la conclusione: la guerra puo continuare e l'occupazione apre una domanda politica.
5. **Il nuovo patto continentale**: dopo conquista completa, ricostruzione e Focus 3. La celebrazione non deve cancellare l'ambivalenza dell'integrazione; deve dichiarare la nascita di una nazione piu grande.

La regola narrativa e semplice: un evento per la decisione, uno per la rottura, uno per l'impresa, uno per la vittoria, uno per il significato della vittoria. Nessun testo e definito in questa fase.

### 11.3 Modelli Millennium Dawn

Tre pattern MD meritano di essere presi come riferimento, senza copiarne contenuti o introdurre sistemi ulteriori.

| Campagna / sistema | File assoluto | Cosa fa | Perche e memorabile |
|---|---|---|---|
| Recupero della Cisplatina brasiliana | `E:\Steam\steamapps\workshop\content\394360\2777392649\events\Brazilian.txt:1255-1265, 4383-4394` | Combina escalation (`add_threat`), war goal di annessione e news dedicata alla rivendicazione | Fa percepire che un cambio di confine e anche una notizia continentale, non solo un trasferimento di stati |
| Crisi mongola cinese | `E:\Steam\steamapps\workshop\content\394360\2777392649\events\China.txt:2032-2060` | Trasforma un rifiuto diplomatico in scelta tra guerra immediata e preparazione bellica | La tensione nasce dalla risposta dell'altro attore; l'azione militare ha una causa leggibile |
| Percorsi cubani di proiezione regionale | `E:\Steam\steamapps\workshop\content\394360\2777392649\events\Cuba.txt:274-304, 506-514, 998-1004` | Alterna evento nazionale, news internazionale e war goal `annex_everything` | Comunica scala con immagini e notiziari, pur mantenendo il codice della guerra essenziale |

Il principio da prendere e la **conseguenza pubblica**: le campagne che restano impresse fanno reagire il mondo, attribuiscono un significato al bersaglio e distinguono l'inizio dalla conclusione. Non sono memorabili per il mero tipo di war goal.

### 11.4 Come rendere la conquista un momento della partita

Le idee seguenti sono direzioni di design, non richieste di implementazione. Vanno selezionate con disciplina: il ramo deve essere solenne, non sovraccarico.

- Dare al Focus 1 una presentazione da discorso presidenziale, con l'ultimatum come immediata conseguenza narrativa.
- Usare una news internazionale all'avvio della guerra e una seconda alla caduta di Citta del Messico. Millennium Dawn usa `news_event`, `picture` e talvolta `major = yes` per elevare svolte nazionali a eventi osservati dal mondo.
- Esplicitare la reazione internazionale nella narrazione: gli alleati valutano il rischio, i rivali condannano o attendono, il continente misura la nuova scala argentina. Non e necessario trasformare ogni reazione in un nuovo sistema diplomatico.
- Trattare `835` come soglia narrativa, non come semplice check: la capitale cade, ma il giocatore deve ancora completare la guerra e assumersi il costo dell'integrazione.
- Usare una breve finestra di mobilitazione o countdown solo se adotta un pattern MD gia presente e se resta leggibile. Deve creare attesa prima dell'ultimatum, non introdurre un timer punitivo o un secondo minigioco.
- Chiudere con una celebrazione pubblica dopo il Focus 3, non alla capitolazione. Questo preserva la differenza fra occupare un territorio e costruire una nuova comunita politica.

Non sono consigliati: propaganda ripetuta a ogni stato catturato, catene random di malus, ultimatum con esito illusorio, o conseguenze diplomatiche permanenti senza un pattern esistente e un chiaro scopo di gameplay.

### 11.5 Tre o quattro focus

Il ramo deve restare di **tre focus**. La sua forza e la leggibilita: decisione, consolidamento, appartenenza. Un quarto focus lineare rischierebbe di dividere artificialmente la ricostruzione dall'integrazione, oppure di aggiungere una seconda preparazione alla guerra priva di funzione distinta.

Un quarto nodo avrebbe valore solo se cambiasse la forma del ramo, per esempio aprendo una scelta postbellica realmente alternativa tra amministrazione temporanea e integrazione piena. Questa non e l'intenzione attuale e richiederebbe sistemi, bilanciamento e narrazione aggiuntivi. Per l'obiettivo definito, tre focus sono la misura giusta.

### 11.6 Nomi dei focus

I nomi attuali sono chiari e coerenti, ma il primo e il secondo possono acquistare forza simbolica senza perdere il significato.

| Ruolo | Nome attuale | Alternativa consigliata | Alternative possibili |
|---|---|---|---|
| Mandato bellico | `La Marcha hacia Mexico` | `El Camino del Norte` | `La Marcha Continental`, `Hacia la Ciudad de Mexico` |
| Consolidamento | `La Integracion de Mexico` | `El Pacto de los Dos Pueblos` | `La Reconstruccion de Mexico`, `La Union del Norte` |
| Cittadinanza | `Una Nacion, Dos Pueblos` | `Una Nacion, Dos Pueblos` | `La Patria de Dos Pueblos`, `El Nuevo Pacto Continental` |

Raccomandazione di tono: mantenere `Una Nacion, Dos Pueblos` come finale, perche e il nome piu umano e conclusivo. Per coerenza con il tono epico di Argentina Rise, adottare `El Camino del Norte` per l'apertura e `El Pacto de los Dos Pueblos` per il secondo focus. Se si preferisce massima chiarezza geopolitica, i nomi originali restano pienamente validi.

### 11.7 Valutazione finale del Lead Game Designer

Non porterei ancora il ramo direttamente in produzione. Non servono altri focus, ne nuovi sistemi: serve prima un ultimo passaggio di direzione narrativa e di verifica di fattibilita.

Prima del codice fisserei: la scelta definitiva dei nomi; il tono del mandato presidenziale; le due news internazionali essenziali; il comportamento esatto dell'ultimatum; e il trigger affidabile della caduta di Citta del Messico. Sono decisioni piccole nel perimetro, ma determinano se il giocatore ricorda una conquista continentale oppure soltanto una guerra che ha sbloccato core e risorse.

Con tali decisioni approvate, porterei l'implementazione in produzione con i tre focus indicati. La struttura e proporzionata al focus tree, la lista stati `828-840` e ora verificata, e i meccanismi tecnici richiesti esistono gia in Argentina Rise e Millennium Dawn. Il passo successivo non e espandere il design: e implementarlo con rigore, usando solo i pattern verificati.

## 12. Raffinamento approvato: ultimatum, notiziari e occupazione

### 12.1 Ultimatum al Messico: pattern verificato e flusso raccomandato

Il pattern richiesto esiste gia in Millennium Dawn e deve essere riutilizzato, non reinventato.

| Elemento | Pattern verificato | Riferimento assoluto |
|---|---|---|
| Decisione / missione che avvia una crisi | una decisione completa la preparazione e chiama un evento | `E:\Steam\steamapps\workshop\content\394360\2777392649\common\decisions\War on Terror.txt:471-512` |
| Evento del paese mittente | il Presidente conferma l'ultimatum e inoltra un evento al bersaglio | `E:\Steam\steamapps\workshop\content\394360\2777392649\events\Iraq.txt:1553-1575` |
| Evento della nazione bersaglio | il bersaglio riceve e risponde all'ultimatum; la risposta rimanda al mittente | `E:\Steam\steamapps\workshop\content\394360\2777392649\events\Iraq.txt:1614-1642` |
| Rifiuto che autorizza la guerra | l'evento successivo assegna `create_wargoal` e attiva una news | `E:\Steam\steamapps\workshop\content\394360\2777392649\events\Iraq.txt:1576-1608` |
| Ultimatum come decisione a tempo | decisione con `days_remove`, poi evento sul paese bersaglio | `E:\Steam\steamapps\workshop\content\394360\2777392649\common\decisions\USA.txt:5349-5389`; `E:\Steam\steamapps\workshop\content\394360\2777392649\common\decisions\Tajikistan.txt:901-933` |

Il flusso approvato per Argentina Rise e pertanto:

1. `La Marcha hacia Mexico` completa: concede claims, evento presidenziale e visibilita alla decisione **Inviare l'Ultimatum a Citta del Messico**.
2. Il giocatore seleziona la decisione: questa chiama l'evento argentino che formalizza l'invio.
3. L'evento argentino inoltra la risposta a `MEX`.
4. L'evento messicano mostra il rifiuto inevitabile e rimanda un evento finale ad `ARG`.
5. L'evento argentino conclusivo assegna `create_wargoal { target = MEX type = annex_everything }`, marca l'escalation e lancia la news di guerra quando il conflitto inizia.

La decisione deve essere una scelta di **quando** aprire la crisi, non di **se** il Messico possa accettare. Il rifiuto inevitabile e narrativamente onesto: l'agenzia del giocatore consiste nel decidere quando l'Argentina e pronta a trasformare il mandato nel confronto continentale.

### 12.2 News internazionali: struttura

Millennium Dawn usa `news_event` con `is_triggered_only = yes`, immagine dedicata e, quando la svolta e di scala globale, `major = yes`. Il sistema cubano mostra news chiamate con ritardo breve (`hours = 6`) e il sistema brasiliano associa una svolta territoriale a una news; riferimenti: `E:\Steam\steamapps\workshop\content\394360\2777392649\events\Cuba.txt:274-304`, `E:\Steam\steamapps\workshop\content\394360\2777392649\events\Brazilian.txt:4383-4394`.

Sono previste esattamente due news internazionali essenziali:

| News | Trigger narrativo | Destinatari / scala | Funzione |
|---|---|---|---|
| **Argentina e Messico in guerra** | la relazione di guerra ARG-MEX e stata effettivamente creata, non il solo ultimatum | `news_event` internazionale; valutare `major = yes` per la scala continentale | dichiara che l'Argentina non e piu un potere regionale meridionale |
| **Caduta di Citta del Messico** | ARG controlla lo stato `835` durante la guerra, una sola volta | `news_event` internazionale; `major = yes` consigliato | segna il climax militare, pur senza dichiarare terminata la guerra |

Non e consigliata una news per ogni avanzata: due soglie nette preservano la gravita dell'evento ed evitano rumore narrativo.

### 12.3 Occupazione, ricostruzione e resistenza

Non serve creare un sistema di resistenza messicana. Il gioco conserva gia la distinzione fondamentale: fino al Focus 3 i territori rimangono posseduti/controllati ma **non core** di ARG; quindi non risultano immediatamente integrati.

Argentina Rise dispone inoltre di un pattern statale gia in uso, `ARG_rise_plan_reconstruccion_austral_state_modifier`, definito in `C:\Users\studi\Documents\Paradox Interactive\Hearts of Iron IV\mod\Argentina_Rise_A_Millennium_Dawn_Rework\common\dynamic_modifiers\ARG_rise_dynamic_modifiers.txt:3-10` e applicato nei rami Uruguay, Paraguay, Bolivia, Cile, Cuba e Caraibi. Esso aumenta la ricostruzione e include `resistance_target = -0.05` e `compliance_gain = 0.02`.

Uso raccomandato:

- dopo la conquista completa, `La Integracion de Mexico` applica il modifier agli stati `828-840`;
- il modifier rappresenta un'amministrazione transitoria che ricostruisce e pacifica, non una popolazione magicamente assimilata;
- `Una Nacion, Dos Pueblos` resta l'unico momento che assegna core e conclude l'integrazione.

Questo riusa esattamente il percorso gia espresso dal ramo Cuba: controllo militare, ricostruzione/pacificazione, cittadinanza. Esistono in MD sistemi di occupazione molto piu estesi (per esempio le missioni Tajikistan: `E:\Steam\steamapps\workshop\content\394360\2777392649\common\decisions\Tajikistan.txt:1877-1898`), ma introdurli qui sarebbe un nuovo sottosistema sproporzionato e non necessario.

### 12.4 Caduta di Citta del Messico: trigger corretto

Il trigger automatico corretto esiste in Millennium Dawn: `on_state_control_changed`. Il pattern ceco controlla nello stesso on-action se uno stato specifico e pienamente controllato dal paese e, con una flag, lancia una sola volta un evento: `E:\Steam\steamapps\workshop\content\394360\2777392649\common\on_actions\99_CZE_on_actions.txt:874-892`.

Applicazione progettuale:

1. l'on-action verifica che `ARG` abbia completato il primo focus o abbia l'apposita flag di campagna;
2. verifica che lo stato `835` sia controllato da ARG e che ARG sia in guerra con MEX;
3. verifica l'assenza di una flag `caduta_di_citta_del_messico`;
4. imposta la flag e chiama l'evento argentino, il quale lancia la seconda news internazionale.

Questo e preferibile a un evento giornaliero: reagisce al momento reale della conquista, evita polling e impedisce duplicazioni grazie alla guardia one-shot. Il controllo non equivale a possesso, quindi non anticipa il Focus 2 o il Focus 3.

### 12.5 Titoli: seconda valutazione

`Una Nacion, Dos Pueblos` resta il titolo conclusivo raccomandato. Per i primi due focus, le opzioni migliori sono:

| Focus | Scelta raccomandata | Perche | Alternative |
|---|---|---|---|
| Primo | `El Camino del Norte` | evoca una marcia storica e un destino continentale senza descrivere banalmente un'invasione | `La Marcha Continental`, `El Mandato del Norte`, `Hacia la Ciudad de Mexico` |
| Secondo | `El Pacto de los Dos Pueblos` | sposta l'attenzione dal possesso amministrativo al dovere politico della ricostruzione | `La Union del Norte`, `La Reconstruccion de Mexico`, `La Hora de la Integracion` |

La terna preferita e quindi: **El Camino del Norte** -> **El Pacto de los Dos Pueblos** -> **Una Nacion, Dos Pueblos**. Conservare i nomi originali rimane una scelta valida se si privilegia la massima trasparenza funzionale nel focus tree.

### 12.6 Valutazione finale aggiornata

Con questi raffinamenti, non aggiungerei altre ambizioni di design prima del codice. Porterei il ramo in produzione dopo una sola approvazione editoriale: confermare la terna di titoli e la natura deliberatamente inevitabile del rifiuto messicano.

Il ramo ora ha una progressione completa e leggibile: mandato, ultimatum, guerra riconosciuta dal mondo, presa della capitale, occupazione temporanea, ricostruzione e cittadinanza. Ogni passaggio usa un pattern gia verificato nella submod o in Millennium Dawn. Un ulteriore miglioramento prima dell'implementazione rischierebbe di trasformare una campagna focalizzata in un sistema diplomatico o occupazionale troppo grande per la sua funzione.

## 13. Decisione approvata: Light Metals messicani

Per scelta deliberata di game design di Argentina Rise, il Focus 3 aggiunge Light Metals tramite `add_resource` nella sola submod. Millennium Dawn non viene modificata: questa non e una ridefinizione della sua geografia di base, ma un reward permanente della completa integrazione messicana.

La distribuzione approvata e concentrata nei tre poli con la maggiore capacita industriale civile nella storia iniziale MD:

| Stato | Capacita industriale MD | Reward Light Metals |
|---:|---:|---:|
| `835` Mexico | 10 industrie civili | +35 |
| `836` Jalisco | 8 industrie civili | +25 |
| `834` Guanajuato | 5 industrie civili | +20 |
| **Totale** | | **+80** |

Petrolio e acciaio rimangono limitati ai soli stati in cui Millennium Dawn gia li colloca. I reward definitivi sono distribuiti in proporzione alla geografia MD esistente:

| Risorsa | Stato | Reward | Totale risorsa |
|---|---:|---:|---:|
| Petrolio | `832` Nuevo Leon | +50 | |
| Petrolio | `838` Veracruz | +59 | |
| Petrolio | `839` Chiapas | +11 | **+120** |
| Acciaio | `832` Nuevo Leon | +19 | |
| Acciaio | `834` Guanajuato | +2 | |
| Acciaio | `835` Mexico | +69 | |
| Acciaio | `836` Jalisco | +60 | **+150** |

Nessun'altra risorsa viene aggiunta ad altri stati messicani.
