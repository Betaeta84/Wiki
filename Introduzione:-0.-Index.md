# Benvenuti in MeerK40t!
MeerK40t è il software scalabile per CNC laser della famiglia K40 (e non solo). Ci auguriamo che troviate questo software **gratuito** utile e facile da imparare e utilizzare.
Stiamo lavorando per renderlo ancora più semplice e intuitivo, e migliorare la documentazione. Naturalmente è necessario molto lavoro e ci auguriamo che siate disposti ad aiutarci in questo progetto. 
## Panoramica
**Congratulazioni per aver trovato MeerK40t.** Forse avete iniziato a lavorare con il software fornito con il vostro laser K40, o avete provato prima K40 Whisperer, o forse la notizia del nostro programma si sta diffondendo e su questo intendete migrare o iniziare. 
Magari avrete sentito parlare del software Lightburn, leader del settore, che è lo standard a cui aspirare, ma che non è in grado di pilotare il K40. Indipendentemente da come siete arrivati qui, un GRANDE benvenuto.

MeerK40t è in grado di sostituire il software fornito con i laser K40 come pure il software K40 Whisperer. È stato progettato specificamente per pilotare la popolare classe di macchine laser K40 (costruite in Cina) che hanno una scheda controller Lhymicro. Sebbene MeerK40t aspiri ad essere in grado di pilotare altre schede di controllo (ad esempio quelle basate su Gerbil), questi driver devono essere ancora essere scritti, ma se avete una scheda di controllo diversa, vi ringraziamo per essere passati di qui, ma vi consigliamo di controllare di tanto in tanto se la vostra scheda è nel frattempo entrata nella [lista](https://github.com/meerk40t/meerk40t?tab=readme-ov-file#supported-devices) di quelle supportate.

Ci auguriamo che Meerk40t sia un software migliore delle alternative disponibili per la serie di macchine K40 e che vi permetta di ottenere di più senza dover spendere tempo e fatica **e, soprattutto** senza costringervi a spese ulteriori, oltre la macchina in se, per aggiornare il controller e acquistare Lightburn.

Cosa rende MeerK40t diverso dalle alternative?
### Percorsi e immagini
MeerK40t supporta 4 tipi di lavorazioni:
* Tagli vettoriali / incisioni vettoriali - essenzialmente la stessa cosa. Il laser segue il percorso vettoriale, la differenzia sta solo nell'intensità della "bruciatura"
* Raster vettoriale - Il laser "brucia" tutti i punti all'interno di un percorso vettoriale (chiuso).
* Immagine raster - La lavorazione di un'immagine raster avviene utilizzando i singoli pixel.

MeerK40t mira a supportare pienamente lo standard SVG 1.1 (comprese le estensioni specifiche per i principali editor, laddove ciò abbia senso).

### Controllo dell'alimentazione
Con la macchine K40 è possibile impostare la potenza del laser dal pannello frontale, ma la scheda controller è in grado solo di accendere e spegnere il laser.

Fortunatamente il controller può accendere e spegnere il laser molto velocemente. Infatti può farlo 1.000 volte per pollice (circa 400 per cm), il che è sufficiente per accendere e spegnere il laser più volte nell'ampiezza del diametro del raggio laser stesso.

MeerK40t utilizza questa capacità per variare l'intensità del laser:
* Costantemente ogni 2 o 3 o ... 1/1000 di pollice
* Per i raster, in modo variabile a seconda del livello di scala di grigio dei pixel

I dettagli tecnici di questa [funzione di modulazione degli impulsi](./Tech:-Raster-pulse-modulation-PPI) sono disponibili [qui](./Tech:-Raster-pulse-modulation-PPI).

### Precisione
MeerK40t utilizza una serie speciale di [algoritmi di tracciatura delle curve di Zingl-Bresham](./Tech:-Zingl-Bresenham-Curve-Plotting) per definire perfettamente le forme. Se il vostro file SVG ha percorsi curvi (che tecnicamente sono archi ellittici o curve di Bezier), una tracciatura   precisa delle forme produrrà un risultato migliore di una approssimazione con una serie di corte linee rette. Il livello di di precisione di Meerk40t è di 1/1000.

### Scelta dei driver
MeerK40t utilizza il driver LibUsb o il driver predefinito di Windows CH341DLL. Ciò significa che MeerK40t può funzionare insieme ad altri software laser che utilizzano uno di questi driver.

### Linea di comando
MeerK40t dispone di una completa [interfaccia a riga di comando](./Help:-Command-Line-Interface). Se si desidera integrare con la macchina laser con un flusso di lavoro automatizzato o semplicemente si preferisce utilizzare la riga di comando, si dovrebbe essere in grado di eseguire la maggior parte dei progetti senza utilizzare l'interfaccia grafica.
MeerK40t dispone anche di una [Linea di comando della console](./Help:-Console-Commands) interna dove è possibile eseguire funzioni più avanzate rispetto a quelle disponibili attraverso la GUI.

## Documentazione
Il nostro obiettivo è quello di avere una documentazione completa che copra:
* [Principianti](./Beginners:-0.-Index) - Guida alla configurazione dell'hardware, all'installazione di MeerK40t e alla realizzazione del primo lavoro.
* [Manuale dettagliato](./Doc:-0.-Index) - Un insieme dettagliato di pagine indicizzate e strutturate gerarchicamente che descrivono in dettaglio tutte le funzionalità del MeerK40t.
* [Ulteriore aiuto]() - Additional - Aiuto aggiuntivo su aree specifiche.
* [Installazione](./Beginners:-2.-Installing-MeerK40t) - Una pagina che fornisce i link alle istruzioni dettagliate per l'installazione di MeerK40t sulle principali piattaforme.

Tuttavia siamo ancora lontani dal raggiungere questo obiettivo, quindi qualsiasi aiuto possiate dare per migliorare la nostra documentazione sarà molto gradito.

## Cercasi aiuto
Il codice è una parte enorme di un programma informatico, ma una piccola parte di una comunità.

Se siete preoccupati di non essere in grado di contribuire perché non sapete scrivere codice Python, non dovreste farlo perché ci sono molti altri modi per contribuire:
* Aggiornare e migliorare queste pagine del [Wiki](./Tech:-Creating-a-wiki-page) per trasmettere le vostre esperienze ad altri.
* Scrivere la documentazione o migliorarla per aiutare gli altri, nella comprensione, a tutti i livelli.
* Pensare e organizzare le priorità a lungo termine.
* Imparare e condividere nuove informazioni.
* Chiedere, schedare e discutere [Github issues](/meerk40t/meerk40t/issues), anonmalie e funzionamenti inattesi o suggerire nuove funzioni o migliorie. Testare, affrontare e analizzare i problemi degli altri. 
* Aggiornare i nuovi collaboratori
* Aiutare gli utenti, non solo qui ma anche nella comunità più ampia. Le comunità più grandi sono più solide e ottengono risultati migliori.
* Automatizzare i flussi di lavoro e/o definire i flussi di lavoro. 
* Tradurre le stringhe dell'applicazione in altre [lingue](./Tech:-Foreign-Language-Translations) (ciò richiedere non solo una certa fluidità, ma anche una buona conoscenza del gergo laser).
* Testare nuove funzionalità sperimentali sulla tua macchina o aiutare a ricercare le caratteristiche/velocità ecc. della scheda controller o di altre schede controller che potremmo voler supportare

Per maggiori dettagli si veda [Tech: Help Wanted](https://github.com/meerk40t/meerk40t/wiki/Tech:-Help-wanted).

Infine, se state usando MeerK40t per scopi commerciali (il programma viene fornito gratuitamente, indipendentemente dal fatto che siate hobbisti o professionisti), e non siete in grado di contribuire, donando un po' del vostro tempo a una qualsiasi delle attività di cui sopra, prendete in considerazione l'idea di [sponsorizzare economicamente @tatarize](/sponsors/tatarize). Un modo per ringraziarlo per tutti i suoi sforzi e per contribuire a pagare le spese che il progetto comporta (come i certificati del codice o l'hardware di ricerca e sviluppo). 

**Nota:** l'uso di questo software è libero e non ci sono termini di licenza o legali che vi obblighino a ricompensare @tararize in questo modo. _La scelta è vostra_, ma ci sembra _etico_ condividere con lui una piccola parte dei vostri profitti quando beneficiate _economicamente dei suoi sforzi_.

### Codifica
Se si desidera scrivere un modulo per uso pubblico o privato:

* Vedere la documentazione del [Kernel API System](https://github.com/meerk40t/meerk40t/wiki/Tech:-Kernel-API-System).

Se si desidera scrivere un'interfaccia grafica per i propri moduli, potrebbe essere necessario aggiungere delle icone.

* [Aggiunta di icone a MeerK40t](https://github.com/meerk40t/meerk40t/wiki/Tech:-Adding-Icons-to-a-MeerK40t-Module)

* [Guida alla progettazione di UI e UX](https://github.com/meerk40t/meerk40t/wiki/Tech:-UI-and-UX-Design-Guide)

## Documentazione dettagliata per la scheda di controllo.

Se si desidera una documentazione dettagliata per la scheda di controllo, le ricerche che abbiamo intrapreso per comprendere l'hardware si possono trovare in questo wiki.

Ci sono pagine specifiche per il [Lhymicro M2-Nano Hardware](./Tech:-Lhymicro-M2-Nano-Hardware) e i [Lhymicro Control Protocols](./Tech:-Lhymicro-Control-Protocols) usati da software come MeerK40t per controllare il K40.

La sezione Tech del Wiki contiene anche altre ricerche sull'hardware del K40, ad esempio [Physical Speed Measurements](./Tech:-Physical-Speed-Measurements)

## Spazio dei nomi SVG
Per il salvataggio dei file, MeerK40t, utilizza una forma modificata di SVG. Qui le specifiche [Namespace](./Namespace).

# Riconoscimenti

* Autore primario: Tatarize
* Codifica aggiuntiva: jpirnay, JKRamarz, MartonMiklos, Jaredly, FrogMaster, Sophist-UK, GSBoylan ([Dettagli](https://github.com/meerk40t/meerk40t/graphs/contributors)) 
* Traduzioni: Peter9811 (spagnolo), jpirnay (tedesco), betaeta (italiano)
* Wiki: Tatarize, JoerLane (guru di M2-Nano), Tiger12506, Olivier33m, Sophist-UK

Se hai contribuito, per favore modifica questa pagina e aggiungi il tuo nome alla lista di cui sopra.

---
### Autori
Il team di MeerK40t è grato per l'aiuto di @taterize, @joerlane e @Sophist-UK nella creazione di questa pagina. Se **tu** pensi che possa essere ulteriormente migliorata, sentiti libero di modificare la pagina e di aggiungere il tuo userid a questo elenco. 😃