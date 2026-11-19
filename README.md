[https://github.com/odinosuperstar-png/ullarium)
<p align="center">
  <img src="assets/logo.svg" width="112" alt="Logo Ullarium">
</p>

<h1 align="center">Ullarium (ULL)</h1>

<p align="center">
  <b>Una blockchain Layer 1 indipendente che chiunque può minare con la CPU del proprio computer.</b><br>
  Proof of Work · blocchi da 3 minuti · tetto fisso di 4.200.000 ULL · nessun premine
</p>

<p align="center">
  <img alt="Go 1.22+" src="https://img.shields.io/badge/Go-1.22%2B-00ADD8?logo=go&logoColor=white">
  <img alt="Consenso" src="https://img.shields.io/badge/consenso-PoW%20SHA--256d-7c3aed">
  <img alt="Supply" src="https://img.shields.io/badge/supply%20max-4.200.000%20ULL-06b6d4">
  <img alt="Blocchi" src="https://img.shields.io/badge/blocchi-3%20minuti-22c55e">
  <img alt="Premine" src="https://img.shields.io/badge/premine-0-f59e0b">
</p>

---

## In breve

| | |
|---|---|
| **Sito Web** | https://ullarium.xyz
| **Ticker** | `ULL` (8 decimali) |
| **Consenso** | Proof of Work SHA-256d, vince la catena con più lavoro cumulato |
| **Tempo di blocco** | 3 minuti (difficoltà LWMA su 60 blocchi: si adegua da sola all'hashrate) |
| **Ricompensa** | 3 ULL a blocco, più le commissioni |
| **Slow start** | nelle prime 24 ore (480 blocchi) la ricompensa sale da 0,3 a 3 ULL |
| **Halving** | ogni 700.000 blocchi (circa 4 anni) |
| **Supply massima** | **4.200.000 ULL**, nessun premine |
| **Indirizzi** | iniziano con `ULL1…` |
| **Porte** | `26333` P2P · `26332` RPC (solo locale) |
| **Genesis hash** | `4abf8e9698ab1d6f9a6f3d4024ded7fa9bd5b25f9cd7cf35f033dabe8a0b58c9` |

Il tuo nodo è sulla rete giusta solo se il suo genesis hash coincide con quello qui sopra: `./ullarium genesis` lo stampa.

---

## ⛏️ Inizia a minare in 5 minuti

### 1. Procurati il programma

- **Scarica** il file per il tuo sistema dalla pagina [Releases](../../releases) (`windows-amd64`, `linux-amd64`, `linux-arm64`, `darwin-amd64` per Mac Intel, `darwin-arm64` per Mac Apple Silicon) e controlla l'impronta con `SHA256SUMS`.
- **Oppure compila** dai sorgenti (serve [Go 1.22+](https://go.dev/dl/)):

```bash
go mod tidy
go build -o ullarium ./cmd/ullarium        # su Windows: -o ullarium.exe
```

### 2. Crea il tuo wallet

```bash
./ullarium wallet new
```

Scegli una passphrase di almeno 8 caratteri. Il comando stampa il tuo indirizzo `ULL1…` e salva il wallet cifrato in `~/.ullarium/wallet.json` (su Windows `C:\Users\<tu>\.ullarium\wallet.json`).

> **Fai subito una copia di `wallet.json` e annota la passphrase.** Se li perdi, i tuoi ULL sono persi per sempre: nessuno può recuperarli.

### 3. Avvia nodo e miner

**Linux / macOS**

```bash
./ullarium node \
  -peers seed1.ullarium.example:26333,seed2.ullarium.example:26333 \
  -mine -miner-wallet ~/.ullarium/wallet.json -threads 4
```

**Windows (PowerShell)**

```powershell
.\ullarium.exe node `
  -peers seed1.ullarium.example:26333,seed2.ullarium.example:26333 `
  -mine -miner-wallet "$HOME\.ullarium\wallet.json" -threads 4
```

- `-threads` è il numero di thread CPU da usare: lascia libero almeno un core per il resto del computer. Senza questa opzione usa tutti i core.
- Al primo avvio il nodo scarica la catena (vedi le righe `[sync]`), poi comincia a minare. Lascialo acceso; `Ctrl+C` lo ferma in modo pulito.
- Il nodo legge dal wallet solo l'**indirizzo**: la passphrase non serve per minare.

### 4. Controlla che funzioni

In un secondo terminale:

```bash
./ullarium info              # altezza, peer connessi, difficoltà, "mining": true
./ullarium mine status       # il tuo hashrate
./ullarium balance ULL1…     # il tuo saldo
```

Quando trovi un blocco compare nel log: `[miner] blocco 1234 minato …`. La ricompensa è accreditata subito all'indirizzo del wallet.

### 5. Guarda il saldo dal browser (facoltativo)

Installa l'[estensione per Chrome, Edge e Brave](#-estensione-per-il-browser): mostra saldo e stato del nodo direttamente sull'icona.

---

## 💰 Ricompense

### Slow start: le prime 24 ore

Per evitare che chi arriva per primo con molta potenza si prenda quasi tutte le monete ("instamine"), nei primi 480 blocchi la ricompensa cresce in modo lineare. Dopo il blocco 480 vale la ricompensa piena.

| Blocco | Tempo dal lancio | Ricompensa |
|---:|:---:|---:|
| 1 | 0 h | **0,30 ULL** |
| 60 | ~3 h | 0,63 ULL |
| 120 | ~6 h | 0,97 ULL |
| 240 | ~12 h | 1,65 ULL |
| 360 | ~18 h | 2,32 ULL |
| 480 | ~24 h | **3,00 ULL** |

### Halving e tetto massimo

Ogni 700.000 blocchi (circa 4 anni) la ricompensa si dimezza. La somma di tutte le ricompense non può superare **4.200.000 ULL**: il tetto è scritto nelle regole di consenso e ogni nodo lo verifica.

| Ciclo | Blocchi | Ricompensa | ULL emessi a fine ciclo | Anno |
|:---:|---|---:|---:|:---:|
| 1 | 1 – 700.000 | 3 ULL | 2.099.352 | ~4 |
| 2 | 700.001 – 1.400.000 | 1,5 ULL | 3.149.352 | ~8 |
| 3 | 1.400.001 – 2.100.000 | 0,75 ULL | 3.674.352 | ~12 |
| 4 | 2.100.001 – 2.800.000 | 0,375 ULL | 3.936.852 | ~16 |
| 5 | 2.800.001 – 3.500.000 | 0,1875 ULL | 4.068.102 | ~20 |
| 6 | 3.500.001 – 4.200.000 | 0,09375 ULL | 4.133.727 | ~24 |

Dopo circa 4 anni è già emesso il 50 % del totale, dopo 8 anni il 75 %. Lo slow start paga 648 ULL in meno rispetto alla ricompensa piena, quindi l'emissione totale converge a circa **4.199.352 ULL**, sempre sotto il tetto. In media si producono 480 blocchi al giorno, cioè 1.440 ULL al giorno nel primo ciclo, divisi tra tutti i miner in proporzione al lavoro svolto.

---

## ⏱️ Quanto tempo serve per un blocco

La rete produce in media un blocco ogni 3 minuti: se arrivano più miner la difficoltà sale, se se ne vanno scende. All'avvio della rete la difficoltà è tarata sul **singolo thread di una CPU normale**: con un solo thread, finché la rete è piccola, il tempo medio per blocco è questo.

| Hashrate di 1 thread | Tempo medio per un blocco |
|---:|---:|
| 1 MH/s (CPU datata) | ~6 min 20 s |
| 2,1 MH/s | 3 min |
| **3,6 MH/s** (riferimento) | **1 min 45 s** |
| 5 MH/s | 1 min 16 s |
| 6,3 MH/s | 1 min |
| 8 MH/s | 47 s |

Il tempo è una media: i blocchi arrivano a caso e uno può richiedere pochi secondi, un altro molto più della media. Per misurare l'hashrate del tuo computer (servono Go e i sorgenti):

```bash
go test -run '^$' -bench PoWHash -benchtime 5s ./internal/core
```

Il risultato mostra i `MH/s` per thread e i secondi attesi per blocco alla difficoltà iniziale.

---

## 🌐 Collegarsi alla rete

- **Peer iniziali:** indica con `-peers` uno o più nodi noti. Da quel momento il tuo nodo ne scopre altri da solo. Per aggiungerne uno mentre il nodo gira: `./ullarium addnode IP:26333`.
- **Porta 26333 (TCP):** aprila in ingresso sul router o sul firewall se vuoi che altri nodi si colleghino a te. Per minare non è obbligatorio, ma aiuta la rete.
- **Porta 26332 (RPC):** resta chiusa a Internet. Il nodo la espone solo su `127.0.0.1`; se la apri a indirizzi non locali pretende `-rpcuser` e `-rpcpass`.
- **Orologio:** deve essere sincronizzato (NTP). I blocchi con data oltre 2 minuti nel futuro vengono rifiutati.
- **Altra rete:** un nodo si collega solo a nodi con lo stesso genesis hash. Se vedi `rete o versione incompatibile` nel log, controlla di avere la versione giusta.

---

## 🧩 Estensione per il browser

Nella cartella [`extension/`](extension) trovi un'estensione per **Chrome, Edge, Brave, Opera e Vivaldi** (qualsiasi browser Chromium). Mostra il saldo sull'icona, l'elenco dei tuoi indirizzi, l'altezza della catena, i peer e ti avvisa quando il saldo aumenta. È di **sola lettura**: non gestisce chiavi private e non può spostare fondi.

1. Apri `chrome://extensions` (in Edge `edge://extensions`) e attiva **Modalità sviluppatore**.
2. Premi **Carica estensione non pacchettizzata** e scegli la cartella `extension`.
3. Clicca l'icona, incolla il tuo indirizzo `ULL1…` e premi **Aggiungi**. Per usare un nodo diverso da quello locale apri l'ingranaggio ⚙.

Dettagli in [`extension/README.md`](extension/README.md).

---

## Comandi utili

| Comando | Cosa fa |
|---|---|
| `ullarium node` | avvia il nodo (con `-mine` anche il miner) |
| `ullarium wallet new` | crea un wallet cifrato |
| `ullarium wallet address` | mostra l'indirizzo del wallet |
| `ullarium balance <indirizzo>` | saldo e nonce |
| `ullarium send -to <indirizzo> -amount 1.5` | firma in locale e invia ULL (commissione predefinita 0,0001 ULL) |
| `ullarium info` | stato della catena e del nodo |
| `ullarium block -height N` | dettagli di un blocco |
| `ullarium tx <txid>` | stato di una transazione |
| `ullarium peers` · `addnode` | peer connessi · aggiungi un peer |
| `ullarium mine start\|stop\|status` | controlla il miner di un nodo già acceso |
| `ullarium genesis` | parametri di consenso e genesis hash |

Ogni comando accetta `-h`. I comandi di interrogazione parlano con il nodo locale (`127.0.0.1:26332`); con `-network devnet` usano la rete di sviluppo (porta 36332).

---

## Compilare e collaudare

```bash
go mod tidy
go vet ./... && go test ./...                     # consenso, emissione, difficoltà, wallet
node --test extension/test/lib.test.js            # libreria dell'estensione (Node 20+)
make release                                      # binari per Linux, macOS e Windows in dist/
```

Per provare tutto in locale senza rischi esiste la **devnet** (blocchi da 10 s, difficoltà minima): `./ullarium node -network devnet -datadir ./dev1 -mine -miner-wallet wallet.json`.

---

## 🔒 Sicurezza

- **Backup:** il file `wallet.json` e la passphrase sono le uniche cose che servono per recuperare i tuoi fondi. Conservali in due posti separati e non condividerli con nessuno.
- **Software giovane:** Ullarium non è stato sottoposto a un audit indipendente. Non metterci più di quanto sei disposto a perdere.
- **RPC privato:** non esporre mai la porta 26332 a Internet.
- **CPU:** il mining usa i core al 100 %: controlla temperature e consumi, soprattutto sui portatili.

## Domande frequenti

**Ho minato un blocco ma il saldo è 0.** Controlla che l'indirizzo con cui hai avviato il miner sia quello giusto (`./ullarium wallet address`) e che il nodo sia sincronizzato (`./ullarium info`). Se un altro miner trova un blocco alla stessa altezza un attimo prima, il tuo viene scartato: succede nelle reti piccole.

**Posso minare con la scheda video?** Il miner incluso usa solo la CPU.

**Quanto pesa la catena?** Poco: la rete produce 480 blocchi al giorno e ognuno pesa pochi KB; la sincronizzazione iniziale è rapida.

**Perché la ricompensa iniziale è così bassa?** È lo slow start: serve a rendere poco conveniente l'instamine nelle prime ore.

## Altra documentazione

- [`docs/TECHNICAL.md`](docs/TECHNICAL.md): regole di consenso, protocollo P2P, API JSON-RPC e limiti noti.
- [`extension/README.md`](extension/README.md): installazione e uso dell'estensione.

## Struttura del repository

```
ullarium/
├── cmd/ullarium/        programma da riga di comando (nodo + wallet + strumenti)
├── internal/            parametri, chiavi, wallet, storage, consenso, P2P, miner, RPC
├── extension/           estensione per Chrome e altri browser Chromium
├── docs/                documentazione tecnica
├── deploy/              esempio di servizio systemd per server Linux
├── assets/              logo
├── Makefile             build, test e release
└── go.mod
```
