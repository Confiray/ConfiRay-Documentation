# 53 – Lokální instalace

## Účel

Lokální instalace umožňuje provozovat ConfiRay přímo ve vývojovém prostředí nebo v infrastruktuře zákazníka.

---

## Současná demo instalace

**Implementováno a ověřeno:** první ConfiRay Server běží na vývojovém počítači.

```text
Veřejný uživatel
└── https://demo.confiray.cz
    └── Cloudflare Tunnel
        └── lokální Web3D backend (127.0.0.1:8001)
            ├── WebCon a ThingView
            ├── sdílená Conveyor metadata a help assets
            ├── cache aktuální Creo konfigurace
            ├── FIFO request JSON
            └── dočasné PVZ a exportní logy

Lokální Creo relace
└── ConfiRay Creo Worker
    ├── guard A_CONFIGURATOR=CONVEYOR
    ├── čtení a zápis parametrů
    ├── regenerace modelu
    └── ProductView export PVZ
```

Po úspěšném načtení PVZ ve WebConu backend naplánuje jeho cleanup. Demo nepoužívá produkční databázi ani trvalé úložiště artefaktů.

### Jednotný runtime startup

**DONE / FUNKČNÍ:** `Start-ConfiRay.ps1` a desktop launcher **ConfiRay START - RESTART** zajišťují první spuštění i bezpečný restart. Startup používá výhradně repo `.venv`, ověří dependency včetně `pyodbc`, existenci ignorovaného Helios configu bez výpisu secrets, bezpečně identifikuje případný starý backend tohoto projektu, spustí backend, čeká na `/api/runtime-status`, připraví Creo/Configurator, čeká na skutečný worker heartbeat a ověří Cloudflare Tunnel. WebCon otevře teprve po readiness; při chybě skončí fail-fast se srozumitelnou diagnostikou.

---

## Produkční lokální instalace

Produkční instalace může oddělit aplikační server a Creo Worker:

```text
ConfiRay Server
├── Web / API
├── Database
├── Job Queue
├── Identity and access control
├── Monitoring
└── Artifact Storage

Creo Worker
└── Creo + ConfiRay Configurator + Publisher
```

Základní worker heartbeat a orphan recovery jsou funkční. Produkční zabezpečení, zálohování, deployment balíčky, service management, více workerů a PDM/PLM konektory jsou plánované.
