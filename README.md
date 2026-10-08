# Diffusore lineare

Il file `Catalogo_diffusore_lineare_verticale_e_orizzontale_v3.xlsx` contiene 19 fogli originali per cataloghi e calcoli CFD, più il nuovo foglio `Verifica_15m`.

## Verifica fino a 15 m

`Verifica_15m` mostra i profili orizzontale e verticale a passi di 0,5 m, confronta il profilo CFD orizzontale con i punti leggibili in `Immagine1.png` e mantiene editabili portata, dimensioni, coefficienti e perdite agli impatti. Il foglio `H_CFD_L1500` contiene un profilo CFD nativo per 450 m³/h e lunghezza 1500 mm: 150 punti fino a 5,772 m. Il profilo immagine ricostruito arriva a 5,521 m lungo la traiettoria annotata. **Oltre 5,772 m i risultati sono estrapolazioni; la parte dopo la parete a 6 m e tutto il verticale verso il soffitto non sono validati da CFD.** La tabella arriva a 15 m, ma ciò non significa che i risultati fino a tale distanza siano attendibili senza ulteriori simulazioni o misure.

La portata mostrata nell’immagine è 450 m³/h (0,125 m³/s). Per una fessura di 40 mm e lunghezza 1500 mm, la velocità media geometrica attesa è 2,083 m/s. Il valore di 5,498 m/s annotato vicino all’uscita è un valore locale e non la velocità media sulla fessura.

I punti estratti, con distanza cumulata tra le coordinate annotate, sono in `punti_cfd_orizzontale.csv`. I coefficienti, le formule, gli scarti e i limiti sono descritti in `NOTE_MODIFICHE.md`.

## Fogli del modello originale

- **Verticale:** `Parametri`, `CFD_L500`, `CFD_L1000`, `CFD_L1500`, `Curve`, `Riepilogo`, `Catalogo`, `Catalogo_L_libera`, `Lancio_vs_L`.
- **Orizzontale:** `H_Parametri`, `H_CFD_L500`, `H_CFD_L1000`, `H_CFD_L1500`, `H_Curve`, `H_Catalogo`, `H_Lancio_vs_L`.
- **Altri:** `Catalogo_L_h`, `Lancio_vs_L_h`, `Note`.

I fogli originali sono conservati. Il catalogo orizzontale interpola i profili CFD in portata e lunghezza; il modello verticale originale riguarda un getto verso il basso e il pavimento. Per il caso verso l’alto, con soffitto a 10 m, usare la stima separata in `Verifica_15m` e considerarla un’ipotesi, non un risultato CFD.
