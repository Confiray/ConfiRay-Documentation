# 25 – Integration Hub

## Účel

Integration Hub zajišťuje komunikaci ConfiRay s externími systémy.

ConfiRay nesmí být závislý na ručním přepisování dat mezi systémy.

---

## Hlavní integrace

Do budoucna může Integration Hub komunikovat například s:

- ERP,
- CRM,
- PDM,
- PLM,
- CAD,
- databázemi,
- dokumentovými systémy,
- e-mailovými systémy.

---

## Creo

Creo je speciální případ integrace.

Creo není běžný webový klient.

Je provozován jako Creo Worker.

Backend předává Workeru úlohu a parametry.

Worker provede:

- načtení konfigurace,
- regeneraci,
- validaci,
- Publish,
- vytvoření výstupu.

### Současný stav integrace Creo

**Implementováno a ověřeno:** WebCon komunikuje s lokálním backendem, backend ukládá request snapshot do FIFO fronty a Creo Worker si nejstarší požadavek atomicky převezme. Worker ověří aktivní model a `A_CONFIGURATOR=CONVEYOR`, mapuje metadata na Creo parametry, model regeneruje a publikuje request-bound PVZ.

Worker zároveň synchronizuje kompletní aktuální konfiguraci aktivního Creo modelu do backendu. Webový klient Creo přímo neovládá.

---

## Asynchronní komunikace

Operace, které mohou trvat delší dobu, nesmí blokovat webové rozhraní.

Požadavek je proto vytvořen jako Job.

Web sleduje jeho stav.

Možné stavy:

- `queued`
- `processing`
- `ready`
- `failed`

`rendering` je navazující klientský stav WebConu při načítání hotového PVZ do ThingView. `viewed` a `cleaned` jsou interní stavy životního cyklu dočasného výstupu.

Cancellation, retry, heartbeat a obecné orchestrace více typů workerů jsou cílová architektura, nikoli současná implementace.

---

## Oddělení systémů

Integration Hub odděluje:

**Web**

od

**Backendu**

od

**Creo**

od

**ERP/PDM/CRM**.

Jednotlivé systémy proto mohou být v budoucnu nahrazeny nebo rozšířeny bez změny celého systému.
