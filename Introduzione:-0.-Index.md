# Benvenuti in MeerK40t!
MeerK40t è il software scalabile per CNC laser della famiglia K40 (e non solo). Ci auguriamo che troviate questo software **gratuito** utile e facile da imparare e utilizzare.
Stiamo lavorando per renderlo ancora più semplice e intuitivo, e migliorare la documentazione. Naturalmente è necessario molto lavoro e ci auguriamo che siate disposti ad aiutarci in questo progetto. 
## Panoramica
**Congratulazioni per aver trovato MeerK40t.** Forse avete iniziato a lavorare con il software fornito con il vostro laser K40, o avete provato prima K40 Whisperer, o forse la notizia del nostro programma si sta diffondendo e su questo intendete migrare o iniziare. 
Magari avrete sentito parlare del software Lightburn, leader del settore, che è lo standard a cui aspirare, ma che non è in grado di pilotare il K40. Indipendentemente da come siete arrivati qui, un GRANDE benvenuto.

MeerK40t è in grado di sostituire il software fornito con i laser K40 come pure il software K40 Whisperer. È stato progettato specificamente per pilotare la popolare classe di macchine laser K40 (costruite in Cina) che hanno una scheda controller Lhymicro. Sebbene MeerK40t aspiri ad essere in grado di pilotare altre schede di controllo (ad esempio quelle basate su Gerbil), questi driver devono essere ancora essere scritti, ma se avete una scheda di controllo diversa, vi ringraziamo per essere passati di qui, ma vi consigliamo di controllare di tanto in tanto se la vostra scheda è nel frattempo entrata nella lista di quelle supportate.

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
* Costantemente ogni 2 o 3 o ... 1/1000 di pollice; oppure
* Per i raster, in modo variabile a seconda del livello di scala di grigio dei pixel.

I dettagli tecnici di questa [funzione di modulazione degli impulsi](./Tech:-Raster-pulse-modulation-PPI) sono disponibili [qui](./Tech:-Raster-pulse-modulation-PPI).

### Precisione
MeerK40t utilizza una serie speciale di [algoritmi di tracciatura delle curve di Zingl-Bresham](./Tech:-Zingl-Bresenham-Curve-Plotting) per definire perfettamente le forme. Se il vostro file SVG ha percorsi curvi (che tecnicamente sono archi ellittici o curve di Bezier), una tracciatura   precisa delle forme produrrà un risultato migliore di una approssimazione con una serie di corte linee rette. Il livello di di precisione di Meerk40t è di 1/1000.

### Scelta dei driver
MeerK40t utilizza il driver LibUsb o il driver predefinito di Windows CH341DLL. Ciò significa che MeerK40t può funzionare insieme ad altri software laser che utilizzano uno di questi driver.

### Linea di comando
MeerK40t dispone di una completa [interfaccia a riga di comando](./Help:-Command-Line-Interface). Se si desidera integrare con la macchina laser con un flusso di lavoro automatizzato o semplicemente si preferisce utilizzare la riga di comando, si dovrebbe essere in grado di eseguire la maggior parte dei progetti senza utilizzare l'interfaccia grafica.
