# Argentina Rise - Focus Tree Reverse Engineering v0.1.3

Analisi statica del tree corrente. Sorgente autorevole: `common/national_focus/ARG_rise_focus_tree.txt` (circa 1-9300). Nessun file di gioco e stato modificato per questa analisi.

## 1. Metodo e limiti

Il tree contiene 353 ID `focus`, 389 blocchi `prerequisite`, 6 `mutually_exclusive`, 29 `available`, 352 `completion_reward` e 41 reward vuote. La lettura del grafo usa ID, prerequisite, `relative_position_id`, coordinate e reward effettive. Per ogni focus i campi da usare nel design sono: ID, x/y, prerequisite, mutex, bypass, available, reward ed eventi/idee chiamati; il blocco sorgente e l'unica rappresentazione completa e aggiornata per ogni singolo nodo. Raggiungibilita runtime, bypass dinamici e validita scope restano **NON VERIFICATI** senza test HOI4.

## 2. Architettura globale

Il tree non e una singola linea: e una grande costellazione di rami verticali collegati da gate politici. Il primo macro-gate e `ARG_rise_the_pampas_engine`; dopo di esso l'evento `ARG_rise.4` sceglie il flag per `ARG_rise_el_nuevo_triunfo` oppure `ARG_rise_el_proyecto_nacional` (`events/ARG_rise_events.txt:88-99`; focus circa 2209 e 3342). Le coordinate mostrano una composizione a colonne: politica/ricostruzione a ovest e centro, energia/industria al centro, rami regionali, militari, ricerca e spazio progressivamente verso est.

### Catalogo del grafo

| Campo | Evidenza nel codice | Uso progettuale |
|---|---|---|
| Nodo | `id = ARG_rise_*` | Identita del focus |
| Livello | `y` e catena `prerequisite` | Ritmo e profondita visiva |
| Collegamento | `prerequisite`, `relative_position_id`, `mutually_exclusive` | Parentela, biforcazione, convergenza |
| Condizioni | `available`, `bypass`, `cancel_if_invalid` | Accesso reale |
| Figli | tutti i focus che citano l'ID nei prerequisite | Larghezza e terminalita |
| Contenuto | `completion_reward` | Classificazione reward |

## 3. Macro-rami rilevati

| Ramo | Nucleo/gate | Ampiezza e ritmo | Stato |
|---|---|---|---|
| Crisi iniziale | focus iniziali, eventi `ARG_rise.2-.4` | introduttivo, lineare fino a Pampas | Implementato, da testare |
| The Pampas Engine | `ARG_rise_the_pampas_engine` | grande collo di bottiglia narrativo | Implementato |
| El Proyecto Nacional | `ARG_rise_el_proyecto_nacional` circa 2209 | colonna politica con discendenti | Parziale, non solo placeholder |
| El Nuevo Triunfo | `ARG_rise_el_nuevo_triunfo` circa 3342 | ramo piu esteso e ramificato | Quasi completo, da testare |
| Industria/terra | `ARG_rise_el_diablo_de_la_tierra` circa 1745 | molto lineare, progressione a livelli | Implementato |
| Energia | nodo energia circa 1613 e rami solare/idro/nucleare | tre linee parallele | Implementato |
| Nucleare/fusione | focus 1676, 4721-4825 e riferimenti facility | lineare, alto impatto | Implementato; Fusion UI non verificata |
| Conferencia | focus, decisioni e categoria dedicate | rete diplomatica/regionale | Implementato, da testare |
| Espansione/Cuba | focus tardivi, eventi e DDS dedicati | contenuto territoriale a valle | Implementato, da testare |
| Militare/navale/spazio | colonne tardive, facility e progetti | rami specialistici lunghi | Implementato parzialmente |

## 4. Statistiche statiche

- Focus totali: **353**.
- Reward vuote: **41**; sono placeholder o nodi strutturali finche non verificati individualmente.
- Focus con segnali di reward economica: **circa 482 riferimenti effetto nel file**, non un conteggio disgiunto di focus.
- Il tree contiene molti reward misti: una classificazione esclusiva economica/militare/politica/tecnologica e fuorviante, poiche un focus puo assegnare PP, Treasury, idea, tecnologia e building insieme.
- Focus terminali, intermedi, iniziali e profondita massima: **NON VERIFICATO**; il tree Paradox usa blocchi annidati e una misura affidabile richiede parser sintattico dedicato, non una regex approssimata.

## 5. Ritmo e colli di bottiglia

### Crisi -> Pampas Engine

La progressione e intenzionalmente seriale: introduce crisi, ricostruzione e poi un singolo gate politico. Vantaggio: trasformazione leggibile. Rischio: il giocatore percepisce un'attesa lunga prima dell'identita di percorso.

### El Nuevo Triunfo

Ha il ritmo piu ricco: il gate richiede il flag `ARG_rise_path_nuevo_triunfo`, poi apre politica, societa, economia, espansione, esercito e ricerca. La presenza di idee `ARG_rise_new_triumph_pressure_*` (`ARG_rise_ideas.txt: circa 160-305`) rende la trasformazione dello Stato percepibile e non soltanto numerica. I 41 reward vuoti globali sono il principale punto da ispezionare prima di dichiararlo finito.

### Proyecto Nacional

Il gate e speculare (`ARG_rise_path_proyecto_nacional`), mutualmente esclusivo con Nuevo Triunfo e con evento iniziale `ARG_rise.5`. Ha almeno `ARG_rise_la_crisis_de_representacion` come discendente immediato (circa 2236-2246), quindi non e un nodo isolato. Il suo ritmo appare piu corto e meno identitario del concorrente: e il candidato naturale a espansione futura.

### Energia e nucleare

Il ramo parte da `ARG_rise_el_advenimiento_del_aliento_primordial` e si divide. Il nucleare e strettamente lineare: cinque focus, ciascuno con impatto elevato. Questo concentra potere e riduce scelte interne, ma e coerente con una fantasia di programma statale centralizzato.

## 6. Narrazione e fantasia di potere

| Ramo | Trasformazione narrativa | Fantasia/stile |
|---|---|---|
| Crisi/Pampas | sopravvivenza -> ricostruzione | gestione economica e stabilizzazione |
| Nuevo Triunfo | protesta -> organizzazione -> autorita nazionale | Stato nazionalista disciplinato, espansivo e tecnocratico |
| Proyecto Nacional | crisi di rappresentanza -> ricomposizione politica | Stato attivo e democratico-popolare, ancora poco differenziato |
| Energia/nucleare | sovranita energetica -> industria pesante | modernizzazione infrastrutturale ad alta scala |
| Conferencia | influenza -> integrazione continentale | leadership regionale diplomatica |
| Cuba/espansione | intervento -> integrazione | proiezione geopolitica e ricostruzione |
| Militare/spazio | capacita nazionale -> autonomia strategica | potenza regionale tecnologica |

Il tree trasforma lo Stato in modo convincente quando combina flag, eventi, idee evolutive e decisioni; e piu debole quando la catena e soltanto una successione di building/bonus.

## 7. Confronto tra rami

- **Piu ricco:** El Nuevo Triunfo, per estensione, idee evolutive e contenuto discendente.
- **Piu lineare:** programmi terra/infrastruttura e nucleare.
- **Piu aperto:** l'area successiva ai gate politici e i rami regionali/Conferencia.
- **Piu povero di identita verificata:** Proyecto Nacional; esiste codice, ma non emerge una biforcazione ideologica completa.
- **Rischio di sproporzione:** i reward nucleari hanno scala eccezionalmente alta rispetto a rami economici incrementali; 25 reattori sono assegnati in cinque focus.

## 8. El Nuevo Triunfo: audit dedicato

### Confermato

- Gate, flag ed evento di scelta: `ARG_rise.4`, flag `ARG_rise_path_nuevo_triunfo`.
- Focus radice, prerequisite e reward: `ARG_rise_el_nuevo_triunfo` circa 3342-3365.
- Idee pressure definite e restore post-guerra civile: `ARG_rise_ideas.txt` circa 160-305; `ARG_rise_restore_effects.txt:440-443` e seguenti.
- Localizzazione del nodo e dei requisiti: `localisation/english/ARG_rise_l_english.yml:111-112, 1266-1270`.

### Da rifinire/verificare

- Nessun elenco affidabile di focus mancanti puo essere inferito dal codice: i nodi assenti sono **NON VERIFICATI**.
- Eventi e decisioni specificamente orfani: **NON VERIFICATI** senza grafo di chiamata runtime.
- 41 reward vuote nel tree richiedono audit individuale; non attribuirle automaticamente al solo Nuevo Triunfo.
- Localizzazione globale: due duplicati noti (`ARG_rise.10.a`, `ARG_rise.10.d`) possono influenzare la catena evento.

## 9. El Proyecto Nacional: solo codice

- Nodo reale: `ARG_rise_el_proyecto_nacional`, x=21 y=14, costo 10, prerequisite Pampas Engine, flag richiesto `ARG_rise_path_proyecto_nacional`, mutex con Nuevo Triunfo (`focus_tree.txt:2209-2233`).
- Reward: PP +50, flag `ARG_rise_proyecto_nacional_started`, evento `ARG_rise.5`, variazione party array.
- Discendente diretto confermato: `ARG_rise_la_crisis_de_representacion`, relative position sul nodo, x=0 y=1, costo 5, evento `ARG_rise.6` (`circa 2236-2246`).
- On-action daily: condizione su flag `ARG_rise_proyecto_nacional_started`, party 6 e assenza leader; chiama `ARG_rise.9` (`common/on_actions/ARG_rise_on_actions.txt:23-33`).
- Evento `ARG_rise.9`: ha trigger legato al flag e installazione leader; il leader esatto e nel file eventi, non dedotto qui.
- Localizzazioni del gate e primo sviluppo: `ARG_rise_l_english.yml:1271-1277`.
- Idee/decisioni esclusive del futuro ramo Milei o comunista: **ASSENTI NEL CODICE RILEVATO**.

Stato reale: **implementato come percorso iniziale con almeno una catena politica e un hook daily; incompleto come alternativa ideologica pienamente differenziata**.

## 10. Struttura consigliata per il futuro Proyecto Nacional

Questa e una proposta architetturale, non una proposta di reward o meccaniche.

- **30-40 focus**, per avvicinare massa e longevita di Nuevo Triunfo senza duplicarlo.
- **Profondita 8-10** dal gate `El Proyecto Nacional` alla conclusione di ciascun finale.
- **Larghezza 3** nella fase di consolidamento: istituzioni, economia sociale-produttiva, federalismo/territorio.
- **Primo punto di biforcazione** dopo 4-5 focus: identita libertaria/Milei versus identita comunista-populista.
- **Due sottorami da 8-10 focus** ciascuno, con differenza narrativa netta e poche sovrapposizioni.
- **Convergenza limitata** su sovranita nazionale, infrastruttura e politica estera; evitare convergenza ideologica che annulli la scelta.
- **Climax** separato per ciascuna identita, poi 2-3 focus terminali di Stato/continente.
- **Lunghezza totale consigliata:** 350-500 giorni per percorso principale, proporzionata ai rami tardivi esistenti.

## 11. Rischi di design da evitare

- Non usare il solo numero di focus come prova di profondita: verificare prerequisite reali e reward vuote.
- Non duplicare la fantasia autoritaria/tecnocratica di Nuevo Triunfo.
- Non concentrare tutti i reward ad alto impatto in una sola catena lineare.
- Mantenere gli eventi come trasformazioni di stato, non sole finestre narrative.
- Esplicitare sempre flag, idee e decisioni dipendenti per evitare rami visibili ma non giocabili.

## 12. Riferimenti principali

- `common/national_focus/ARG_rise_focus_tree.txt`
- `events/ARG_rise_events.txt`
- `common/ideas/ARG_rise_ideas.txt`
- `common/on_actions/ARG_rise_on_actions.txt`
- `common/scripted_effects/ARG_rise_restore_effects.txt`
- `common/decisions/ARG_rise_buenos_aires_conference_decisions.txt`
- `localisation/english/ARG_rise_l_english.yml`
- MD: `E:\Steam\steamapps\workshop\content\394360\2777392649\common\buildings\00_buildings.txt`
- MD: `E:\Steam\steamapps\workshop\content\394360\2777392649\common\scripted_effects\!_energy_effects.txt`
