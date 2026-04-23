# Marketing Campaign Analysis
### Segmentazione clienti e ottimizzazione del budget marketing per un'azienda food & beverage

Analisi completa end-to-end su un dataset di 2.240 clienti di un retailer food & beverage premium, con l'obiettivo di rispondere a una domanda di business concreta: **come dovrebbe essere allocato il budget delle prossime campagne marketing, basandosi sul comportamento storico dei clienti?**

Il progetto combina una segmentazione ibrida (RFM classica + clustering K-Means) per isolare tre cluster strategici di clienti e produce una raccomandazione di allocazione budget quantificata, con una gerarchia di valore documentata tra i cluster di **fino a 47×**.

---

## Risultati principali

L'analisi ha prodotto tre risultati chiave, tradotti in raccomandazioni operative:

- **Il 9% dei clienti (Campaign Enthusiasts) giustifica il 45% del budget.** Un piccolo gruppo di 209 clienti iper-reattivi alle campagne, con un Expected Value per Contact (EVC) di 1.435€, assorbe quasi metà dell'allocazione raccomandata. La priorità è proteggerne la fidelizzazione e replicarne il profilo nell'acquisizione di nuovi clienti.

- **Il 32% dei clienti (Affluent Regulars) giustifica il 42% del budget.** Il cluster "mainstream" del business, dove la lente RFM diventa operativamente essenziale: al suo interno i clienti At Risk (168 individui) sono il punto di leva tattico a più alto rendimento marginale dell'intero dataset, candidati prioritari a campagne di riattivazione.

- **Il 58% dei clienti (Budget Families) giustifica solo l'8% del budget.** Un'evidenza controintuitiva ma difendibile: l'EVC di questo cluster è di appena 30€, e un'anomalia sui super-responder suggerisce che le campagne promozionali su questo segmento attraggono cacciatori di deal che cannibalizzano margine anziché generarne.

Tra questi cluster, l'analisi isola inoltre **due sottogruppi ibridi** identificati dall'intersezione RFM × K-Means, non visibili con una segmentazione singola: 41 *At-Risk-Enthusiasts* (priorità massima di riattivazione) e 51 *Lost-non-Budget* (candidati a win-back selettivo).

---

## Struttura del progetto

```
Marketing-Campaign-Analysis/
├── notebooks/
│   ├── 01_data_exploration.ipynb       Prima esplorazione e qualità dati
│   ├── 02_data_cleaning.ipynb          Cleaning e feature engineering
│   ├── 03_eda.ipynb                    EDA univariate, bivariate, correlazioni
│   ├── 04_segmentazione_clienti.ipynb  RFM + K-Means + confronto
│   └── 05_campaign_analysis.ipynb      Efficacia campagne e raccomandazioni
├── data/
│   ├── marketing_campaign.csv          Dataset originale (Kaggle)
│   ├── marketing_campaign_clean.csv    Output del Notebook 02
│   └── marketing_campaign_segmented.csv Output del Notebook 04
├── README.md
└── requirements.txt
```

I notebook vanno letti nell'ordine numerico: ciascuno costruisce sull'output del precedente e documenta passo passo le scelte metodologiche.

---

## Metodologia in sintesi

**Dati.** Il dataset originale — *Customer Personality Analysis* da Kaggle — contiene 2.240 record e 29 variabili sui clienti, tra cui dati socio-demografici (età, reddito, presenza di figli), comportamento d'acquisto per categoria di prodotto, canale di acquisto, e storico delle accettazioni di 6 campagne marketing. Dopo cleaning e feature engineering il dataset di lavoro contiene 2.236 clienti e 34 variabili, incluse 7 feature derivate (`Age`, `MntTotal`, `TotalPurchases`, `TotalCampaignsAccepted`, `Customer_Tenure_Days`, `Family_Size`, `Has_Children`).

**Segmentazione ibrida.** Il progetto adotta due segmentazioni complementari: una **segmentazione RFM** classica (Recency, Frequency, Monetary) che produce 4 segmenti — Champions, Loyal, At Risk, Lost — orientati al momento del ciclo di vita del cliente; e una **segmentazione K-Means** con `RobustScaler` su 5 variabili (`Income`, `MntTotal`, `Has_Children`, `Recency`, `TotalCampaignsAccepted`) che produce 3 cluster orientati al profilo socio-comportamentale — Campaign Enthusiasts, Affluent Regulars, Budget Families. La scelta di K=3 è motivata nel Notebook 04 con criteri di interpretabilità di business. I due metodi non sono ridondanti: sono **ortogonali sui segmenti attivi** e **convergenti sul segmento dormiente**.

**Metrica di efficacia.** L'analisi del Notebook 05 utilizza l'**Expected Value per Contact (EVC)**, definito come *P(responder) × spesa media del responder*, come indicatore sintetico di valore atteso per cluster. Il confronto intra-cluster tra responder e non-responder serve come controllo osservazionale del *selection effect*, per discriminare tra l'ipotesi "la campagna genera valore" e l'ipotesi "la campagna attrae clienti che già spendevano molto".

**Approccio alle raccomandazioni.** Le raccomandazioni finali adottano una logica ibrida **K-Means come spina dorsale strategica** (profilo del cliente) + **RFM come lente tattica** (momento del ciclo di vita), applicata selettivamente dove la combinazione aggiunge informazione.

---

## Come riprodurre l'analisi

### Requisiti
- Python 3.12
- Dipendenze elencate in `requirements.txt`

### Setup
```bash
git clone https://github.com/RobertPaul07/Marketing-Campaign-Analysis.git
cd Marketing-Campaign-Analysis
pip install -r requirements.txt
```

### Esecuzione
Aprire i notebook in Jupyter nell'ordine numerico (`01` → `05`). Ciascun notebook è autonomo nella sua esecuzione ma dipende dagli output su disco dei notebook precedenti (i CSV nella cartella `data/`).

---

## Dataset

*Customer Personality Analysis* — disponibile su Kaggle: https://www.kaggle.com/datasets/imakash3011/customer-personality-analysis

---

## Autore

**Roberto Paul** — [GitHub](https://github.com/RobertPaul07)

Progetto realizzato come parte del portfolio di data analytics. Per domande, feedback o collaborazioni, contattatemi tramite GitHub.
