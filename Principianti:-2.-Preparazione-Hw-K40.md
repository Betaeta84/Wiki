Una volta disimballato il K40 (ma vale per qualsiasi altra macchina laser), si potrebbe essere tentati di collegarlo direttamente alla rete elettrica e iniziare a usare il laser, ma per una serie di motivi **non è consigliabile**.

Come si può vedere dal lungo elenco che segue, il K40 non arriva esattamente pronto per l'uso immediato e la quantità di tempo e il livello di abilità tecnica necessari per prepararlo correttamente all'uso non sono banali.

Inoltre, a causa del numero di elementi presenti, non possiamo fare molto di più che darvi una panoramica, per cui dovrete fare ulteriori ricerche su ciascun elemento (altrimenti non arriveremmo mai a parlare di Meetk40t). (Se trovate risorse utili durante le vostre ricerche, vi chiediamo di aggiungere i link a questa pagina wiki per aiutare i futuri lettori a risparmiare tempo nelle loro ricerche).

Una volta letta e assimilata questa pagina e preparato l'hardware, il passo successivo del Tutorial per principianti è l'[installazione di MeerK40t](./Principianti:-3.-Installazione-MeerK40t).

Se si desidera tornare all'[indice della sezione principianti](./Principianti:-0.-Index) fare clic su [qui](./Principianti:-0.-Index).

## Contenuti
* [Questioni di sicurezza](#safety-matters)
* [Fine corsa](#fine-corsa)
* [Qualità della combustione](#qualitàdellacombustione)
* [Miglioramenti](#enhancements)
* [Carbonizzazione](#charring)

## Questioni di sicurezza
Si spera che abbiate già letto la [pagina Wiki - La sicurezza è importante](./Principianti:-1.-Questioni-di-Sicurezza) per avere una comprensione di base delle principali aree di potenziale pericolo per la vostra salute quando utilizzate il vostro K40 (o altra macchina laser).

La sezione di questa pagina Wiki è stata pensata per fornire informazioni su come controllare e/o modificare l'hardware del K40 al fine di ridurre questi rischi e poter utilizzare il K40 in modo sicuro.

La qualità costruttiva dei K40 può essere estremamente variabile e alcuni dei problemi più comuni di un K40 appena disimballato possono essere pericolosi o addirittura mortali.

Non possiamo fornire qui le risposte a tutti i problemi, ma vogliamo darvi almeno un'indicazione sulle cose da fare per evitare che il vostro K40 vi fulmini, vi bruci gli occhi, si danneggi o semplicemente vi deluda con risultati molto scarsi.

1. [Sicurezza elettrica](#sicurezza-elettrica)
2. [Fumi](#fumi)
3. [Raffreddamento](#raffreddamento)
4. [Protezione degli occhi](#protezione-occhi)
5. [Taglio laser delle palpebre](#lid-laser-cut-off)
6. [Rischio di incendio](#rischio-di-incendio)

### Sicurezza elettrica
#### Tensioni del laser
I laser CO2 funzionano a tensioni **estremamente** elevate, tali da fermare il cuore, e i condensatori dell'alimentatore K40 possono mantenere una carica per un periodo di tempo considerevole dopo lo spegnimento o la disconnessione dalla rete elettrica. 
***Si prega di prestare molta attenzione quando ci si avvicina alle parti ad alta tensione dell'elettronica.*** 
Lasciare che i condensatori si scarichino per diverse ore dopo aver scollegato l'apparecchio dalla rete elettrica.

#### Collegamenti elettrici
Alcuni riferiscono che il cablaggio all'interno del K40 può essere mal collegato (l'autore della versione inglese di questa pagina riferisce "_io ho trovato diversi fili non ben serrati quando ho ricevuto il mio_"). Pertanto, prima di collegarlo, seguire tutti i fili e assicurarsi che tutti i terminali siano ben serrati.

Non dimenticare di controllare anche il cablaggio all'interno dell'unità di ventilazione per verificare che i collegamenti siano corretti.

#### Messa a terra
È **essenziale** che il K40 sia collegato a terra in modo corretto, sia per evitare folgorazioni accidentali se lo chassis metallico viene accidentalmente collegato alla rete elettrica o alle tensioni del laser, sia perché la durata del laser può essere notevolmente ridotta se non è collegato a terra in modo corretto.

Se si utilizza il K40 in un paese in cui tutte le spine sono dotate di una messa a terra adeguata, di solito è sufficiente come base; in caso contrario (e se avete dei dubbi, fatelo controllare da un elettricista), è necessario installare un picchetto di terra vicino al K40 e collegarlo alla connessione di terra sul retro.

Ma la cosa più importante è verificare che il collegamento a terra sul retro non sia stato isolato dal telaio con la vernice. Svitatelo, raschiate la vernice intorno al foro e riavvitatelo. Verificare con un ohmmetro che la resistenza tra il telaio e il pin della messa a terra della rete sia di 0,0 ohm.

#### Collegamenti ad alta tensione
Controllate con attenzione i cavi dell'alta tensione che collegano il tubo laser - (sempre dalla versione inglese: "_i nostri erano a posto_"), ma secondo internet molte persone hanno segnalato connessioni di scarsa qualità.

### Fumi
La tossicità dei fumi di una lavorazione laser dipende dal materiale che si sta lavorando. Alcuni materiali possono essere davvero molto tossici, mentre altri producono solo odori sgradevoli. Sono pochi, se non addirittura nessuno, i materiali che si possono lavorati in modo completamente sicuro e privi di odori senza il bisogno avere un sistema di estrazione dei fumi.

La ventola di aspirazione in dotazione è generalmente considerata inadeguata; inoltre, essendo montata sul retro del laser, crea una pressione positiva nel tubo di scarico verso l'esterno e qualsiasi perdita in tale tubo provocherà l'ingresso dei fumi nella stanza di lavoro. Consigliamo pertanto l'aggiunta di una seconda ventola in linea all'estremità del tubo di scarico per creare una pressione negativa nel tubo ed evitare questo problema. Si consiglia inoltre di usare del nastro isolante in schiuma per sigillare le varie aperture della macchina e per riempire l'ampio spazio tra la ventola standard e il telaio.

Il tubo di scarico, in plastica fornito in dotazione è generalmente considerato del tutto inutile. Considerando che uno in alluminio espandibile costa solo pochi euro/dollari, è sicuramente una miglioria  fortemente consigliata

Il condotto metallico che va dal pannello posteriore all'area del letto è generalmente considerato non ottimale. Si estende oltre il bordo posteriore del telaio e può impedire l'inserimento di materiali che occupano l'intera profondità dell'area (anche se la parte posteriore di quest'area non è raggiungibile dal laser). Alcuni consigliano di rimuovere completamente questo condotto per migliorare il flusso d'aria, ma il maggior volume d'aria movimentato può essere compensato dal fatto che l'aria viaggia intorno ai lati del telaio anziché attraverso il letto del laser. Un'alternativa è tagliare questo condotto in modo che non sporga oltre il bordo posteriore del portale.

### Raffreddamento
Il tubo laser del K40 (e di altri laser a CO2) è raffreddato ad acqua, flusso del liquido è **essenziale** per evitare guasti dovuti al surriscaldamento. Nella versione inglese l'autore delle pagine afferma "non ho mai provato a sparare con il mio laser K40 senza acqua nel tubo, ma alcuni sostengono che questo possa uccidere istantaneamente il tubo". Per esperienza personale posso dire che non all'istante, anche se il surriscaldamento danneggia il tubo in modo irreversibile, ma nel giro di pochi minuti si _spacca_ lasciando uscire la CO2. La "sopravvivenza" del tubo, senza raffreddamento dipende da vari fattori legati alla temperatura e al tempo e potenza d'uso. L'assenza di flusso provoca rapidamente la **ROTTURA**. 

La maggior parte dei laser della famiglia K40 (ma anche altri) è dotata di tubi per l'acqua e di una qualche forma di pompa per la circolazione dell'acqua stessa. Solitamente non viene fornito alcun serbatoio o un sistema di raffreddamento. Per evitare di ridurre in modo significativo la durata del tubo laser, è importante mantenerlo al di sotto dei 25°C durante l'uso (in genere si consiglia una temperatura compresa tra i 15°C e i 20°C); pertanto, anche se questa sezione non riguarda la sicurezza, la creazione di un serbatoio d'acqua e la decisione su come mantenere l'acqua all'interno del suddetto intervallo di temperatura sono una parte essenziale della messa in funzione della macchina laser.

Come serbatoio si può utilizzare un contenitore di plastica da 50 litri come questo:

![image](https://user-images.githubusercontent.com/3001893/127775469-04944050-076a-4864-a1d8-904f013b60e9.png)

Praticando due fori su un'estremità del coperchio per far passare i tubi, con rondelle di gomma su entrambi i lati per formare una sorta di guarnizione, quindi attaccare la pompa al tubo di ingresso e montare pesanti rondelle zincate sull'altro (tenute da altre rondelle di gomma) per appesantire l'uscita e tenerla sotto l'acqua ed evitare che l'aria risalga nel tubo quando la pompa è spenta.

È inoltre essenziale che l'acqua utilizzata sia non conduttiva. Il modo più semplice per ottenere questo risultato è utilizzare acqua deionizzata (acquistabile in bottiglie da 5 litri presso negozi e supermercati - presso il LIDL è reperibile circa 0,70 € a bottiglia. In alternativa distillata o osmotizzata con conducibilità < 5-10 microSiemens. Se l'acqua del rubinetto è sufficientemente dolce o addolcita potrebbe essere adatta anche quella (è tuttavia consigliabile un test di conducibilità). Potrebbe essere necessario aggiungere all'acqua un prodotto chimico [antialghe](Come-antialghe) non conduttivo, ma il cambio periodico dell'acqua, consigliato, riduce al minimo questo problema.

Qualora vi trovate in un clima in cui è difficile mantenere la temperatura dell'acqua al di sotto dei 20°C-25°C, avrete bisogno di una qualche forma di raffreddamento. Per raffreddare l'acqua possiamo usiamo i pacchetti di ghiaccio del nostro congelatore domestico, ma molte persone investono in tecnologia per raffreddare l'acqua. Ecco un elenco non esaustivo di possibili soluzioni:

* Aggiungere al secchio bottiglie d'acqua congelate.
* Se l'acqua del rubinetto è sufficientemente dolce e pura, fatela scorrere nel secchio e aggiungete un tubo di troppo pieno.
* Riciclate un refrigeratore per dispenser d'acqua da ufficio.
* Riciclate un frigorifero in miniatura.
* Utilizzare pannelli Peltier/elettrotermici (Ho provato ma per laser da una 50W i risultati sono inconsistenti).
* Utilizzare un radiatore per il raffreddamento ad acqua della CPU del computer.
* Utilizzare un “refrigeratore d'acqua” CW-3000 (scambio aria-aria, limitata efficienza in caso di temperature ambientali sostenute) o CW-5200 (sicuramente più efficiente, dotato di sistema di refrigerazione autonomo) .
* Usare un refrigeratore d'acqua per acquari.
* Riciclare un condizionatore d'aria da finestra.

Poiché l'acqua si espande quando si congela, se vivete in un clima più freddo dovete anche assicurarvi che l'acqua nel tubo laser non si congeli, altrimenti si romperà il vetro. È possibile aggiungere un antigelo non conduttivo al serbatoio dell'acqua o scaricare l'acqua dal tubo quando c'è il rischio di congelamento.

Come antialghe e anticongelante si può aggiungere il 10-20% di PURO glicole etilenico o glicole propilenico. Non utilizzare fluidi per automobili o simili in quanto additivati con sostanze elettricamente conduttive. Cambiare l’acqua periodicamente. L’inevitabile aumento della conduttività peggiora le prestazioni dell’alimentatore, soprattutto quando opera a bassa potenza o a impulsi (es. incisione e marcatura)

