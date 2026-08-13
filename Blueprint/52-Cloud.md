# 52 – Cloud

## Princip

ConfiRay může být provozován jako cloudová služba nebo jako lokální instalace.

---

## Cloudová varianta

Cloudová instalace může obsahovat:

- Web,
- Backend,
- databázi,
- Job Queue,
- úložiště,
- jeden nebo více Creo Workerů.

---

## Oddělení

Webová a backendová část nemusí běžet na stejném fyzickém počítači jako Creo.

Creo Worker může být samostatný server.

---

## Škálování

Cloudová architektura umožňuje postupné přidávání Workerů podle zatížení.

---

## Data

Konkrétní umístění dat závisí na typu instalace a požadavcích zákazníka.

---

## Vývoj

Cloud není podmínkou první verze.

První vývojová verze může běžet lokálně.

---

## Současný veřejný demo provoz

**Implementováno a ověřeno:** `demo.confiray.cz` je veřejný vstup do WebConu přes Cloudflare Tunnel. Tunnel směruje provoz na lokální ConfiRay backend; Creo Worker, FIFO requesty, PVZ soubory a ThingView assets zůstávají v lokálním prostředí.

To není plná cloudová instalace. Cloudová databáze, objektové úložiště, horizontální škálování, produkční identity, monitoring, vysoká dostupnost a oddělení tenantů jsou plánované.
