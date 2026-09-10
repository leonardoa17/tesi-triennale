# Simulazione e analisi della Botnet Mirai

File sorgente LaTeX e note del mio progetto di tesi triennale.

Ho riprodotto il ciclo operativo di Mirai all'interno di un ambiente controllato: scansione e brute force Telnet su credenziali di fabbrica, download del payload e collegamento al server C2. Successivamente ho lanciato un attacco HTTP flood contro un server web Nginx per misurarne il degrado di disponibilità e prestazioni.

### Il Lab
L'ambiente è configurato tramite Docker all'interno di una rete bridge isolata su VM Linux:
* **C2 / Loader:** Su container Ubuntu, gestisce l'infrastruttura, serve i binari e impartisce i comandi di attacco.
* **Scanner:** Script Python per scansionare gli indirizzi locali ed eseguire il brute force Telnet.
* **Bot:** Container Alpine che eseguono il malware ed effettuano heartbeat/keep-alive verso il C2.
* **Target:** Web server Nginx bersaglio dell'attacco.

### Strumenti utilizzati
* **Wireshark e Zeek:** Cattura PCAP, ispezione dei frame Telnet e correlazione strutturata dei flussi (`conn.log`, `http.log`).
* **Glances:** Monitoraggio del carico CPU (user/system) e del load average sul nodo bersaglio.
* **Vegeta:** Benchmarking della latenza HTTP prima e durante la saturazione.

### Risultati principali
Durante un attacco HTTP flood di 240 secondi a 2.500 req/s:
* La latenza p99 è passata da ~61 ms in condizioni ordinarie al superamento del timeout di 10 secondi.
* Oltre 10.000 richieste sono andate in timeout, riducendo la percentuale di successo dal 100% al 98.25%.
* L'utilizzo complessivo della CPU del target ha raggiunto livelli critici (40-60% user/system).

Il PDF completo della tesi è disponibile nella repository (`tesi_finale.pdf`).

<details>
<summary><b>English Version</b></summary>

# Mirai Botnet Simulation & Analysis

LaTeX source files and notes from my B.Sc. thesis project.

I reproduced the complete operational cycle of the Mirai botnet in a controlled lab: Telnet scanning, dictionary brute-forcing on default credentials, payload delivery, and C2 registration. I then conducted an HTTP flood attack against an Nginx web server to measure service degradation and resource saturation.

### Lab Setup
The testbed runs entirely via Docker on an isolated bridge network inside a Linux VM:
* **C2 / Loader:** Ubuntu container managing registrations, serving payloads, and issuing attack directives.
* **Scanner:** Python script automating Telnet brute-force against configured IP ranges.
* **Bots:** Alpine containers running the cross-compiled binary and maintaining C2 keep-alive sessions.
* **Target:** Standard Nginx HTTP server.

### Toolchain
* **Wireshark & Zeek:** PCAP capture, Telnet command-frame inspection, and connection logging (`conn.log`, `http.log`).
* **Glances:** Telemetry tracking for CPU (user/system) and load average on the target container.
* **Vegeta:** HTTP performance benchmarking before and during the flood.

### Key Findings
Under a 240-second HTTP flood attack at 2,500 req/s:
* p99 latency surged from ~61 ms baseline to exceeding the 10-second timeout threshold.
* Over 10,000 requests timed out, dropping server availability from 100% to 98.25%.
* Target CPU load entered critical state with sustained 40-60% user/sys utilization.

The full compiled thesis PDF is included in the root folder (`tesi_finale.pdf`).

</details>
