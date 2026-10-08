# Note sulle modifiche

## Contenuto originale del workbook

Il file conteneva 19 fogli:

`Parametri`, `Catalogo`, `Catalogo_L_libera`, `Lancio_vs_L`, `Catalogo_L_h`, `Lancio_vs_L_h`, `H_Catalogo`, `H_Lancio_vs_L`, `Riepilogo`, `Curve`, `Note`, `CFD_L500`, `CFD_L1000`, `CFD_L1500`, `H_CFD_L500`, `H_CFD_L1000`, `H_CFD_L1500`, `H_Curve`, `H_Parametri`.

- **Verticale originale:** `Parametri` raccoglie fessura, altezza, influenza del pavimento e velocità terminale; `CFD_L500/L1000/L1500` contengono i dati; `Curve`, `Riepilogo`, `Catalogo`, `Catalogo_L_libera` e `Lancio_vs_L` li interpolano. Le celle `Parametri!B4:B13` e la tabella `Parametri!A16:F19` sono i principali input e parametri. Questo modello è per un getto verticale verso il basso, non per l’impatto sul soffitto.
- **Orizzontale originale:** `H_Parametri` contiene fessura (`B4`), distanza parete (`B5=6 m`), zona di influenza (`B6`), limite del getto libero (`B7=B5-B6`), velocità terminale (`B8`) e distanza minima significativa (`B9`). `H_CFD_L500/L1000/L1500` sono i dati di ingresso; `H_Curve` calcola profili normalizzati `v/Q`, poi `H_Catalogo` e `H_Lancio_vs_L` interpolano fra portate e lunghezze.
- Il profilo originale `H_CFD_L1500` include il caso **Q=450 m³/h** e 150 punti nelle colonne `Q:R`, da `x=0` a `x=5,772 m`. Il catalogo originale lavora sulla propria griglia, più corta del limite dei dati grezzi.

Le formule e i dati dei 19 fogli originali sono stati preservati. È stato aggiunto `Verifica_15m`, con formule Excel e ricalcolo automatico all’apertura.

## Punti letti dall’immagine

Sono leggibili 11 annotazioni di velocità e coordinate. Il CSV riporta la distanza cumulata tra le coordinate consecutive (lungo la traiettoria disegnata), con origine nel primo punto annotato `(X=0,021 m; Y=2,994 m)`. È una ricostruzione geometrica approssimata della distanza dal diffusore; l’immagine non fornisce una coordinata esplicita del punto di uscita. Le velocità riportate sono quelle visibili nelle etichette e non sono state stimate per interpolazione.

La distanza massima dei punti immagine è **5,521 m**; il profilo CFD nativo del workbook arriva a **5,772 m**. Per il confronto, le distanze cumulate dell’immagine sono confrontate con la coordinata streamwise `x` del profilo nativo, che riproduce bene le velocità annotate. Il confronto non è una validazione indipendente, perché immagine e profilo sembrano riferirsi allo stesso caso CFD.

La scritta nell’immagine indica `SG Volume Flow Rate 3: 450.0000 m³/h`. Quindi l’unità è m³/h, non solo presunta. La portata equivale a `0,125 m³/s`; con area geometrica `0,04 × 1,5 = 0,06 m²`, la velocità media allo slot è `2,083 m/s`. Il valore locale di `5,498 m/s` vicino all’uscita è un picco annotato e non contraddice direttamente la velocità media.

## Formule e coefficienti della nuova scheda

In `Verifica_15m`, gli input editabili sono in `B5:B16`: portata, fessura, lunghezza, parete a 6 m, soffitto a 10 m, coefficienti, lunghezze di nucleo/transizione e fattori di perdita. `B17` calcola la velocità media `V0=Q/[3600·b·L]`; `B18:B19` mostrano i limiti dei dati nativi e dei punti immagine.

Per il tratto orizzontale coperto dai dati, il profilo usa un’interpolazione lineare fra i 150 punti CFD originali in `H_CFD_L1500!Q10:R159`. Per la verifica di forma sul tratto di transizione è stata adattata ai punti nativi con `x ≥ 3,128589 m` la legge:

`Vfit = K·V0·SQRT(b/(x − xtrans + lc))/SQRT(1 + x/L)`

con `K = 2,48552`, `lc = 0,553 m`, `xtrans = 3,128589 m`, `L = 1,5 m` e `V0 = 2,083 m/s`. La correzione `1/SQRT(1+x/L)` considera in modo approssimato la lunghezza finita; nella zona coperta dai dati non viene sovrapposta alla curva CFD, che già include la geometria reale. La regressione sui 69 punti nel tratto indicato ha **RMSE 0,0385 m/s** e **MAE 0,0259 m/s**.

Oltre `x=5,772 m`, la formula orizzontale prosegue la legge a getto piano ancorandola all’ultimo valore CFD, così da evitare discontinuità:

`V(x) = Vmax·SQRT((xmax−xtrans+lc)/(x−xtrans+lc))·SQRT((1+xmax/L)/(1+x/L))`

Da `x=6 m` è applicato il fattore editabile `0,70` per l’impatto sulla parete. Il fattore è una stima, non una misura né una regressione CFD.

Per il verticale verso l’alto, non sono disponibili punti CFD specifici. La scheda usa separatamente `Kv=2,48552` e `lc,v=0,553 m` come **ipotesi iniziali** (uguali ai valori orizzontali, ma editabili), con:

`Vv(x) = Kv·V0·SQRT(b/(x+lc,v))/SQRT(1+x/L)`

Il fattore editabile `0,70` è applicato dal soffitto a 10 m in poi, misurando la distanza complessiva lungo il percorso del getto. Non è una regressione verticale: per validare il tratto a parete servono dati CFD verticali verso l’alto.

La scheda contiene formule per un profilo da 0 a 15 m con passo 0,5 m, il confronto punto per punto con l’immagine e una tabella di controllo della regressione sui dati nativi. Gli scarti fra profilo nativo e annotazioni dell’immagine sono **RMSE 0,0409 m/s** e **MAE 0,0170 m/s** (11 punti, incluso il campione vicino all’uscita). Questi valori misurano l’accordo fra due rappresentazioni del caso CFD, non l’accuratezza del modello fisico oltre il dominio simulato.

## Validità e limiti

- Per l’orizzontale il dato CFD arriva a 5,772 m; da lì in avanti il risultato è estrapolato. L’impatto a 6 m e tutto il tratto successivo fino a 15 m sono sensibili al fattore di perdita e non sono validati.
- Per il verticale richiesto, diretto verso il soffitto a 10 m, non ci sono punti CFD dedicati: l’intero profilo è un’ipotesi. I dati del modello originale verso il basso non sono intercambiabili.
- L’effetto della lunghezza finita è rappresentato da una correzione semplice e modificabile nel modello; non sostituisce una simulazione CFD tridimensionale.
- Isotermia assunta. Galleggiamento, profilo trasversale, condizioni al contorno, ricircoli e geometria reale delle superfici possono cambiare il lancio.
- La tabella fino a 15 m garantisce che il foglio definisca un valore, non che tale valore sia attendibile. Per dichiarare valida la previsione fino a 15 m occorre estendere le simulazioni CFD oltre parete/soffitto o misurare il campo di velocità.
