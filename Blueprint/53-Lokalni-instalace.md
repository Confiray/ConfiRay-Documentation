# 53 – Lokální instalace

## Účel

Lokální instalace umožňuje provozovat ConfiRay přímo v prostředí zákazníka.

---

## Vývojová fáze

První verze ConfiRay Backend může být provozována na vývojovém notebooku.

Notebook v této fázi představuje první ConfiRay Server.

Součástí prostředí může být:

- Web,
- Backend,
- Job Queue,
- Creo,
- Creo Worker,
- Publisher,
- 3D Viewer.

---

## Produkční lokální instalace

Produkční instalace může mít vlastní server.

Například:

```text
ConfiRay Server
│
├── Backend
├── Database
├── Job Queue
└── Storage

Creo Worker
└── Creo + Configurator + Publisher
