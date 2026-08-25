# CAN Bus Intrusion Detection

Sistema di rilevamento anomalie su rete CAN veicolare basato su Machine 
Learning

## Obiettivo

Rilevare messaggi CAN iniettati da un attaccante (attacchi DoS, Fuzzy, 
spoofing gear/RPM) distinguendoli dal traffico legittimo, usando feature 
comportamentali/temporali invece di regole statiche.

## Dataset

[Car Hacking Dataset](https://ocslab.hksecurity.net/Datasets/car-hacking-dataset) 
(HCRL, Korea University) — traffico CAN reale da un Hyundai YF Sonata, 
con 4 tipologie di attacco etichettate.

## Approccio

- **Feature engineering**: 
  - `iat` — tempo tra messaggi consecutivi con lo stesso CAN ID
  - `global_iat` — tempo tra messaggi consecutivi sull'intero bus
- **Modello**: Random Forest (scikit-learn)
- **Nota metodologica**: una prima versione del modello includeva anche 
  l'ID del messaggio come feature, ottenendo un F1-score di 1.00 — un 
  segnale di overfitting, dato che il modello aveva semplicemente 
  memorizzato l'ID specifico usato nell'attacco DoS di questo dataset, 
  senza generalizzare. Le feature sono state quindi riviste per basarsi 
  solo su comportamento temporale, generalizzabile a diversi tipi di 
  attacco.

## Risultati

| Attacco | F1-score (classe attacco) | Recall |
|---|---|---|
| DoS | 0.98 | 0.99 |
| Fuzzy | 0.96 | 0.98 |
| Gear spoofing | 0.95 | 0.99 |
| RPM spoofing | 0.95 | 0.99 |

