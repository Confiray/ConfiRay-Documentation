# 48 – API

## Účel

API je komunikační rozhraní mezi jednotlivými částmi platformy ConfiRay.

---

## Web API

Customer Portal komunikuje s Backendem prostřednictvím API.

API zajišťuje například:

- přihlášení,
- registraci,
- načtení produktu,
- načtení parametrů,
- odeslání konfigurace,
- vytvoření Job,
- zjištění stavu Job,
- načtení výsledku.

---

## Job API

Základní operace:

- vytvořit Job,
- získat stav Job,
- získat výsledek,
- případně Job zrušit.

### Současné API technologického dema

**Implementováno a ověřeno:**

- session login/logout a načtení přihlášené identity;
- `GET /api/runtime-status` – necitlivá readiness backendu, worker heartbeat a dostupnost HeliosConnectoru;
- `GET /api/customers?q=...`, `GET /api/customers/{id}` a kontakty firmy – session-bound Customer API přes serverově zvolený Connector;
- `GET /api/configuration/current` – aktuální konfigurace synchronizovaná z Creo;
- `PUT /api/configuration/current` – úplná synchronizace z workeru;
- `POST /api/requests` – vytvoření request snapshotu ve stavu `queued`;
- `GET /api/requests/{requestId}` – stav, `queuePosition` a po `ready` název PVZ;
- `POST /api/worker/claim` – atomické FIFO převzetí nejstaršího requestu a změna na `processing`;
- `POST /api/worker/heartbeat` – lease a recovery workeru;
- `POST /api/requests/{requestId}/status` – worker oznámí `ready` nebo `failed`;
- `POST /api/cleanup` – po zobrazení naplánuje cleanup dočasného výstupu;
- `POST /api/offers` – serverově ověřený customer/contact/configuration/model/render → DOCX/PDF;
- `GET /api/offers/{offerId}/pdf|docx` – bezpečné stažení pouze povoleného offer artefaktu.

Metadata a help assets jsou backendem poskytovány z autoritativní Conveyor definice.

---

## Worker API

Backend komunikuje s Creo Workery.

Worker:

1. získá Job,
2. potvrdí převzetí,
3. zpracuje Job,
4. průběžně aktualizuje stav,
5. odešle výsledek,
6. oznámí dokončení.

Současný worker neodesílá průběžné procentuální progress události. Persistované stavy jsou `queued`, `processing`, `ready` a `failed`; `rendering` je klientský stav WebConu. Cancellation, retry a obecné result API jsou plánované.

---

## Heartbeat

Worker pravidelně informuje Backend, že je aktivní.

Backend tak může rozlišit:

- ONLINE,
- BUSY,
- ERROR,
- OFFLINE.

**DONE / FUNKČNÍ:** Creo Worker posílá heartbeat každých 5 sekund. Backend udržuje 90sekundový lease, kontroluje ownership requestu a zotavuje orphaned `processing` requesty do `failed`. Produkční persistentní registr workerů, telemetrie a multi-worker scheduler jsou **PLANNED / PLÁNOVÁNO**.

---

## Verze API

API musí být verzované.

Budoucí změny nesmí automaticky rozbít starší klienty nebo Workery.

Současné lokální demo endpointy zatím nejsou verzované.

---

## Bezpečnost

API nesmí být veřejně dostupné bez autentizace a autorizace.

Citlivé operace musí být chráněny.

**DONE / FUNKČNÍ v současném rozsahu:** browser API vyžaduje serverovou session, změnové operace CSRF a request/offer/PVZ endpointy kontrolují vlastníka a organizaci. Worker API přijímá pouze lokální worker provoz a používá heartbeat/ownership. Produkční federovaná identity, role, tenant isolation, audit a secret manager zůstávají **PLANNED / PLÁNOVÁNO**.
