Pipeline di Data Quality e Machine Learning: Taxi Data con DuckDB e Pandas
Obiettivo di Business Questo progetto analizza i flussi di traffico e i ricavi dei Taxi di New York, trasformando milioni di log di viaggio grezzi in insight geolocalizzati. L'obiettivo è classificare i quartieri in base al volume di traffico e alla redditività, fornendo una base dati ottimizzata per la reportistica e simulando un caso d'uso reale di Location Analytics.
Architettura e Stack Tecnologico
•	Data Ingestion & Aggregation: DuckDB (In-process OLAP SQL)
•	Data Manipulation & ML: Pandas, Scikit-Learn (K-Means Clustering)
•	Formato Dati: Parquet (per i Fatti massivi), CSV (per le Dimensioni spaziali)
Benchmark delle Prestazioni: Pandas vs DuckDB Una delle sfide principali è stata l'elaborazione di un dataset massivo (oltre 3.47 milioni di record). Per ottimizzare la pipeline, ho condotto un test di performance mettendo a confronto l'approccio in-memory standard con l'esecuzione vettorializzata su disco:
•	Approccio Pandas: 7.84 secondi (con elevato consumo di RAM per il caricamento del file Parquet).
•	Approccio DuckDB: 0.40 secondi (interrogando direttamente il file Parquet tramite SQL).
Risultato: Sfruttando DuckDB per le operazioni pesanti di JOIN e aggregazione, i tempi di elaborazione sono stati ridotti drasticamente (circa 19.6x più veloce), azzerando i rischi di colli di bottiglia legati alla memoria.
Fasi del Progetto
1.	Data Quality & Estrazione: Utilizzo di DuckDB per interrogare direttamente i file Parquet, unendo i log dei viaggi con le anagrafiche dei quartieri scartando i record anomali.
2.	Ingegnerizzazione: Calcolo aggregato in SQL del Numero di Viaggi e della Corsa Media in Dollari per ogni distretto e quartiere.
3.	Machine Learning: Passaggio diretto del risultato aggregato da DuckDB a Pandas. Applicazione di una normalizzazione Z-score e addestramento di un modello K-Means per clusterizzare i quartieri in 3 livelli strategici (Alto, Medio, Basso traffico/valore).
