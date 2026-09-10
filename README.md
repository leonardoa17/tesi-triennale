#IT
# Simulazione e analisi della Botnet Mirai

File sorgente LaTeX e note del mio progetto di tesi triennale.

Ho riprodotto il ciclo operativo di Mirai all'interno di un ambiente controllato: scansione e brute force Telnet su credenziali di fabbrica, download del payload e collegamento al server C2[span_0](start_span)[span_0](end_span). Successivamente ho lanciato un attacco HTTP flood contro un server web Nginx per misurarne il degrado di disponibilità e prestazioni[span_1](start_span)[span_1](end_span).

### Il Lab
L'ambiente è configurato tramite Docker all'interno di una rete bridge isolata su VM Linux[span_2](start_span)[span_2](end_span):
* **C2 / Loader:** Su container Ubuntu, gestisce l'infrastruttura, serve i binari e impartisce i comandi di attacco[span_3](start_span)[span_3](end_span).
* **Scanner:** Script Python per scansionare gli indirizzi locali ed eseguire il brute force Telnet[span_4](start_span)[span_4](end_span).
* **Bot:** Container Alpine che eseguono il malware ed effettuano heartbeat/keep-alive verso il C2[span_5](start_span)[span_5](end_span).
* **Target:** Web server Nginx bersaglio dell'attacco[span_6](start_span)[span_6](end_span).

### Strumenti utilizzati
* **Wireshark e Zeek:** Cattura PCAP, ispezione dei frame Telnet e correlazione strutturata dei flussi (`conn.log`, `http.log`)[span_7](start_span)[span_7](end_span).
* **Glances:** Monitoraggio del carico CPU (user/system) e del load average sul nodo bersaglio[span_8](start_span)[span_8](end_span).
* **Vegeta:** Benchmarking della latenza HTTP prima e durante la saturazione[span_9](start_span)[span_9](end_span).

### Risultati principali
Durante un attacco HTTP flood di 240 secondi a 2.500 req/s[span_10](start_span)[span_10](end_span):
* La latenza p99 è passata da ~61 ms in condizioni ordinarie al superamento del timeout di 10 secondi[span_11](start_span)[span_11](end_span).
* Oltre 10.000 richieste sono andate in timeout, riducendo la percentuale di successo dal 100% al 98.25%[span_12](start_span)[span_12](end_span).
* L'utilizzo complessivo della CPU del target ha raggiunto livelli critici (40-60% user/system)[span_13](start_span)[span_13](end_span).

Il PDF completo della tesi è disponibile nella repository (`tesi_finale.pdf`)[span_14](start_span)[span_14](end_span).


#ENG
# Mirai Botnet Simulation & Analysis

LaTeX source files and notes from my B.Sc. thesis project.

I reproduced the complete operational cycle of the Mirai botnet in a controlled lab: Telnet scanning, dictionary brute-forcing on default credentials, payload delivery, and C2 registration[span_15](start_span)[span_15](end_span). I then conducted an HTTP flood attack against an Nginx web server to measure service degradation and resource saturation[span_16](start_span)[span_16](end_span).

### Lab Setup
The testbed runs entirely via Docker on an isolated bridge network inside a Linux VM[span_17](start_span)[span_17](end_span):
* **C2 / Loader:** Ubuntu container managing registrations, serving payloads, and issuing attack directives[span_18](start_span)[span_18](end_span).
* **Scanner:** Python script automating Telnet brute-force against configured IP ranges[span_19](start_span)[span_19](end_span).
* **Bots:** Alpine containers running the cross-compiled binary and maintaining C2 keep-alive sessions[span_20](start_span)[span_20](end_span).
* **Target:** Standard Nginx HTTP server[span_21](start_span)[span_21](end_span).

### Toolchain
* **Wireshark & Zeek:** PCAP capture, Telnet command-frame inspection, and connection logging (`conn.log`, `http.log`)[span_22](start_span)[span_22](end_span).
* **Glances:** Telemetry tracking for CPU (user/system) and load average on the target container[span_23](start_span)[span_23](end_span).
* **Vegeta:** HTTP performance benchmarking before and during the flood[span_24](start_span)[span_24](end_span).

### Key Findings
Under a 240-second HTTP flood attack at 2,500 req/s[span_25](start_span)[span_25](end_span):
* p99 latency surged from ~61 ms baseline to exceeding the 10-second timeout threshold[span_26](start_span)[span_26](end_span).
* Over 10,000 requests timed out, dropping server availability from 100% to 98.25%[span_27](start_span)[span_27](end_span).
* Target CPU load entered critical state with sustained 40-60% user/sys utilization[span_28](start_span)[span_28](end_span).

The full compiled thesis PDF is included in the root folder (`tesi_finale.pdf`)[span_29](start_span)[span_29](end_span).

