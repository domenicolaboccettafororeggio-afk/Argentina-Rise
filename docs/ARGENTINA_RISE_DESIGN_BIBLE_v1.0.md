# Argentina Rise - Design Bible v1.0

Questo documento e la bussola creativa del progetto. Non sostituisce il codice: distingue cio che e **IMPLEMENTATO** da cio che e **PIANIFICATO** e guida ogni futura aggiunta.

## 1. Visione

Argentina Rise e una storia giocabile di ricostruzione nazionale dentro Millennium Dawn. Non e una semplice espansione di statistiche per l'Argentina e non vuole replicare MD: prende le sue strutture economiche, politiche e geopolitiche per raccontare una domanda specifica: come puo un paese in crisi tornare a scegliere il proprio destino?

La sua identita nasce dal passaggio dalla vulnerabilita alla direzione. Il giocatore parte dentro le fratture della crisi, costruisce capacita statale, sceglie una risposta politica e proietta poi quell'identita nel continente e nel mondo. L'obiettivo della versione 1.0 e un'Argentina che si senta trasformata, non soltanto potenziata.

## 2. Filosofia di game design

1. Ogni ramo deve avere una fantasia di potere riconoscibile prima di avere una lista di reward.
2. Un focus deve cambiare il significato dello Stato, il problema del giocatore o l'orizzonte della partita; un bonus senza contesto non e contenuto.
3. Gli eventi raccontano la frattura, la scelta e la conseguenza. I focus costruiscono capacita e direzione.
4. Le idee persistenti devono rappresentare una condizione politica o istituzionale, non essere un deposito anonimo di numeri.
5. Una biforcazione deve modificare tono, priorita e stile di gioco, non solo icone o drift ideologico.
6. Millennium Dawn e la piattaforma: privilegiare Treasury, energia, influenza, welfare, party arrays, special projects e altri sistemi gia esistenti prima di inventare sistemi paralleli.
7. La potenza deve essere guadagnata in una curva: sopravvivere, stabilizzare, scegliere, trasformare, proiettare.
8. Le ricompense ad alta scala richiedono un climax narrativo e una conseguenza strategica.
9. I rami devono convergere solo su interessi nazionali condivisi; non devono cancellare il valore della scelta politica.
10. Il codice reale e la fonte dell'implementazione. Un documento di design descrive soltanto il pianificato finche non esiste nel gioco.

## 3. Identita dei grandi rami

### Gli Anni della Cenere - IMPLEMENTATO

- Politica: un paese amministrato sotto pressione, privo di una direzione stabile.
- Economia: sopravvivenza, riparazione e recupero di controllo.
- Narrazione: il giocatore non conquista: resiste abbastanza a lungo da meritare una scelta.
- Fantasia/emozione: responsabilita sotto pressione; ansia che diventa possibilita.
- Trasformazione: da Stato reattivo a Stato che puo pianificare.

### The Pampas Engine - IMPLEMENTATO

- Identita: il motore materiale della ricostruzione.
- Economia: il territorio produttivo smette di essere sfondo e diventa leva nazionale.
- Narrazione: campi, filiere, logistica e lavoro si unificano in una capacita di governo.
- Fantasia/emozione: far ripartire qualcosa di enorme con strumenti imperfetti.
- Ruolo: e il ponte obbligato fra crisi e scelta politica; non va svuotato con rami alternativi prematuri.

### El Nuevo Triunfo - IMPLEMENTATO, DA TESTARE

- Politica: ordine, autorita, disciplina, organizzazione nazionale.
- Economia: produttivita guidata, controllo e capacita di mobilitazione.
- Militare: autonomia strategica e proiezione di potere.
- Narrazione: rabbia dispersa che diventa macchina politica.
- Fantasia/emozione: costruire uno Stato forte e deliberatamente assertivo.
- Stile: verticale, deciso, nazionale, tecnocratico; deve far sentire il giocatore artefice di una nuova dottrina statale.

### El Proyecto Nacional - IMPLEMENTATO PARZIALMENTE

Il codice conferma un gate post-Pampas, il flag di percorso, l'evento iniziale, una catena discendente e un hook daily. La sua identita definitiva non e ancora pienamente differenziata.

- Politica desiderata: ricomposizione, rappresentanza, Stato attivo e patto sociale.
- Economia desiderata: mercato interno, federalismo produttivo, capacita pubblica e protezione della ricostruzione.
- Narrazione desiderata: il paese non sceglie l'ordine come comando, ma come accordo da rifondare.
- Fantasia/emozione: ricucire una nazione senza negarne i conflitti.
- Stato: da crisi di legittimita a progetto condiviso.

### Rivoluzione Libertaria - PIANIFICATO

Non sono stati trovati focus, eventi, decisioni o idee esplicitamente legati a Milei/libertarismo.

- Identita desiderata: rottura con l'apparato, responsabilita individuale, mercato e demolizione delle rendite.
- Emotione: liberazione, rischio, velocita, conflitto con abitudini consolidate.
- Stile: scelte nette, semplificazione, riallocazione e inserimento internazionale.
- Stato: da amministratore esteso a garante forte di regole, stabilita e concorrenza.

### Rivoluzione Popolare - PIANIFICATO

Non e stata trovata una biforcazione comunista/populista esplicita nel codice.

- Identita desiderata: sovranita sociale, organizzazione popolare, solidarieta e capacita produttiva collettiva.
- Emotione: mobilitazione, dignita, conflitto con interessi consolidati, conquista di protezione.
- Stile: costruire consenso e capacita territoriale prima della proiezione.
- Stato: da arbitro debole a infrastruttura sociale e produttiva della nazione.

### Conferencia de Buenos Aires - IMPLEMENTATO, DA TESTARE

- Identita: Argentina come centro di gravita regionale.
- Fantasia: non occupare soltanto il continente, ma convocarlo, negoziarlo e riorganizzarlo.
- Emotione: autorevolezza crescente e responsabilita continentale.
- Trasformazione: da Stato nazionale a potenza che definisce istituzioni regionali.

### Programma nucleare - IMPLEMENTATO

- Identita: sovranita energetica e modernita pesante.
- Fantasia: decidere che l'infrastruttura del secolo e nazionale.
- Emotione: ambizione tecnica, fiducia in una scala industriale nuova.
- Trasformazione: da consumatore vincolato a produttore di capacita energetica.

### Fusione nucleare - PIANIFICATO/DA VERIFICARE

Esistono dipendenze MD per special project e facility, ma la comparsa del progetto Fusion nella UI dopo il percorso previsto non e stata verificata in gioco.

- Identita desiderata: il futuro non viene inseguito, viene prodotto.
- Fantasia: passare da potenza regionale a laboratorio strategico del XXI secolo.
- Emotione: meraviglia tecnologica con responsabilita nazionale.

## 4. Curva emotiva

| Ramo | Inizio | Sviluppo | Crisi | Climax | Conclusione |
|---|---|---|---|---|---|
| Cenere | smarrimento | resistenza | esaurimento delle vecchie soluzioni | scelta di ricostruire | speranza disciplinata |
| Pampas | potenziale disperso | coordinamento | costo della trasformazione | motore nazionale in moto | diritto di scegliere un percorso |
| Nuevo Triunfo | rabbia | organizzazione | conflitto con inerzia e opposizione | autorita nazionale consolidata | Stato assertivo |
| Proyecto Nacional | sfiducia | ricomposizione | conflitto fra interessi e rappresentanza | nuovo patto politico | nazione governabile insieme |
| Libertaria | soffocamento | rottura | costo sociale e politico della liberalizzazione | nuovo ordine economico | Stato piu leggero, societa piu esposta |
| Popolare | abbandono | mobilitazione | confronto con privilegi e scarsita | potere sociale organizzato | Stato-protezione produttivo |
| Conferencia | isolamento | diplomazia | equilibrio fra influenza e autonomia altrui | architettura regionale | leadership continentale |
| Nucleare/Fusione | dipendenza | investimento | scala e responsabilita | sovranita energetica/scientifica | modernita nazionale |

## 5. Trasformazione dello Stato

Argentina Rise deve mostrare mutazioni leggibili. La crisi crea uno Stato che reagisce. Pampas crea uno Stato che coordina. Nuevo Triunfo crea uno Stato che comanda. Proyecto Nacional deve creare uno Stato che ricompone. Libertaria deve creare uno Stato che limita le proprie funzioni per rafforzare regole e mercato. Popolare deve creare uno Stato che organizza protezione e produzione. Conferencia trasforma lo Stato in un attore istituzionale continentale. Nucleare e fusione lo trasformano in un produttore di futuro.

## 6. Trasformazione del giocatore

Il giocatore deve cambiare domanda nel corso della partita:

1. "Come non crollo?"
2. "Come rimetto in funzione il paese?"
3. "Quale Argentina sto costruendo?"
4. "Come faccio valere questa Argentina nel continente?"
5. "Quale capacita lascio come eredita?"

La progressione e riuscita se le scelte politiche modificano non solo l'efficienza, ma il modo in cui il giocatore legge territorio, lavoro, diplomazia, energia e conflitto.

## 7. Cosa non deve mai diventare Argentina Rise

- Una copia argentina di contenuti generici MD.
- Un catalogo di bonus senza eventi, conseguenze o trasformazione.
- Due rami politici con lo stesso gameplay e nomi diversi.
- Un tree pieno di focus riempitivi che esistono solo per allungare la catena.
- Un sistema parallelo quando MD offre gia un contratto riutilizzabile.
- Un progetto che confonde pianificato e implementato.
- Una successione di enormi reward senza ritmo, costo narrativo o climax.
- Una mod in cui la scelta politica viene annullata da convergenze troppo ampie.

## 8. Roadmap creativa verso 1.0

### Fase I - Rendere leggibile il presente

Consolidare la curva Cenere -> Pampas -> scelta e validare in gioco Nuevo Triunfo, Proyecto Nacional, Conferencia ed energia. Il fine creativo e sapere cosa il giocatore percepisce oggi, non aggiungere volume.

### Fase II - Dare identita completa a Proyecto Nacional

Costruire il tronco comune: rappresentanza, federalismo, ricostruzione sociale-produttiva e crisi della legittimita. Deve essere abbastanza forte da far desiderare la scelta successiva.

### Fase III - Due rivoluzioni, due giochi

Implementare Libertaria e Popolare come risposte incompatibili alla medesima domanda nazionale. Condividere solo interessi strategici finali, non il tono, gli eventi o la fantasia di potere.

### Fase IV - Integrare continente e capacita

Connettere ogni esito politico alla Conferencia, all'espansione, alla strategia energetica e alla capacita militare/scientifica con conseguenze diverse.

### Fase V - Climax 1.0

Rifinire i finali: ogni percorso deve lasciare una mappa, una societa e un ruolo internazionale riconoscibilmente diversi.

## 9. Checklist per ogni futura implementazione

- Il focus/evento/idea appartiene a una fantasia di potere dichiarata?
- Trasforma lo Stato o il modo di giocare?
- Ha un inizio, una tensione e una conseguenza narrabile?
- Riutilizza un sistema MD esistente prima di proporre qualcosa di nuovo?
- E chiaro se e IMPLEMENTATO o PIANIFICATO?
- Gli ID, flag, idee ed eventi hanno un proprietario di ramo?
- Se assegna un'idea permanente, e stata considerata la logica di restore post-civil-war?
- Se tocca Treasury, energia, party arrays, influenza o special projects, sono state verificate le dipendenze MD?
- Il ramo conserva differenza reale rispetto ai percorsi concorrenti?
- La localizzazione spiega la trasformazione, non solo il bonus?
- Esiste un test di gioco mirato per la nuova catena?

## 10. Manifesto

Argentina Rise esiste perche l'Argentina non deve essere un paese con qualche focus in piu. Deve essere un problema storico, politico ed emotivo che il giocatore attraversa dall'interno.

Qui la ricostruzione non e un intermezzo prima della potenza: e il luogo in cui la potenza acquista un significato. Un campo, una fabbrica, un sindacato, una provincia, un reattore, una conferenza o una scelta di governo valgono solo quando cambiano la risposta alla domanda fondamentale: chi decide il futuro del paese, e per quale paese?

La mod deve rispettare Millennium Dawn come mondo e sistema, ma deve parlare con una voce argentina. Deve usare strutture esistenti per raccontare trasformazioni nuove. Deve preferire una scelta che cambia la partita a dieci bonus che la rendono soltanto piu grande.

Ogni ramo deve far sentire al giocatore di avere costruito uno Stato riconoscibile. Ogni evento deve rendere la scelta umana. Ogni focus deve lasciare una traccia nel paese. Ogni climax deve essere meritato.

Argentina Rise non e la promessa di un'Argentina perfetta. E la promessa che l'Argentina abbia, finalmente, un destino giocabile.

## Fonti

- `docs/PROJECT_STATUS_v0.1.3.md`
- `docs/FOCUS_TREE_REVERSE_ENGINEERING_v0.1.3.md`
- `docs/ARGENTINA_RISE_FORENSIC_ARCHITECTURE_v0.1.3.md`
- `common/national_focus/ARG_rise_focus_tree.txt`
- `events/ARG_rise_events.txt`
- `common/ideas/ARG_rise_ideas.txt`
