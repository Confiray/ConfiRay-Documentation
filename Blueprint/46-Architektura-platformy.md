# 46 – Architektura platformy

## Cílový princip

ConfiRay je vícevrstvá platforma pro workflow:

**Capture → Configure → Price → Publish → Deliver**

```text
Web / Customer Portal / interní klient
↓
Backend a API
↓
Job Queue
↓
Creo Worker a další workery
↓
Publisher
↓
3D / nabídka / dokumentace / výrobní výstupy / integrace
```

ConfiRay Sales a ConfiRay Engineering používají společná produktová metadata, pravidla a konfigurace. Sales pokrývá zejména obchodní workflow; Engineering navazuje CAD automatizací a konstrukčními výstupy.

---

## Současný technologický základ

**Implementováno a ověřeno pro referenční produkt Conveyor:**

```text
WebCon na demo.confiray.cz
↓ HTTP přes Cloudflare Tunnel
lokální Web3D backend
↓ persistentní FIFO requesty
Creo Worker v aktivní Creo relaci
↓ regenerace + ProductView export
dočasný PVZ
↓
ThingView ve WebConu
```

Cloudflare Tunnel zveřejňuje lokální backend, ale fronta, Creo, worker a soubory zůstávají lokální. Jde o demo topologii, nikoli hotovou produkční cloudovou platformu.

---

## Metadata a Configure

Autoritativní definice Conveyor produktu obsahuje:

- `PARAMETERS` – pole, vazby na Creo, typy, rozsahy, jednotky, seskupení a viditelnost,
- `RULES` – neplatné kombinace a validační zprávy,
- `HELP` – kontextovou nápovědu a obrázky.

Stejná metadata používá Creo Configurator i WebCon. WebCon načte aktuální konfiguraci aktivního Creo modelu, zvýrazní změny a Publish odešle jako neměnný snapshot.

---

## Backend a FIFO Queue

Současný lokální backend zajišťuje:

- WebCon a ThingView assets,
- sdílená metadata a help obrázky,
- cache aktuální Creo konfigurace,
- vytvoření a čtení requestů,
- šestimístná ID a request-bound název PVZ,
- FIFO claim nejstaršího queued requestu,
- `queuePosition` bez zpřístupnění cizích requestů,
- ověření hotového PVZ,
- řízený cleanup dočasných PVZ a logů.

Requesty jsou dnes atomicky zapisované JSON soubory. Databáze, uživatelé, tenant isolation, robustní retry a distribuovaná fronta jsou cílová architektura.

---

## Creo Worker

Creo Worker je výpočetní jednotka ConfiRay. Jeden worker zpracovává jednu Creo úlohu v daném okamžiku.

Současný worker:

1. synchronizuje konfiguraci aktivního modelu,
2. převezme nejstarší request,
3. ověří aktivní model a `A_CONFIGURATOR=CONVEYOR`,
4. mapuje snapshot přes metadata na Creo parametry,
5. model automaticky regeneruje,
6. exportuje `confiray_<requestId>.pvz`,
7. oznámí `ready` nebo `failed`.

Více workerů, heartbeat, plánování podle typu produktu a automatické zotavení jsou plánované.

---

## Stavy a ThingView

Persistovaný stav requestu je:

`queued → processing → ready`

nebo `failed`.

Po `ready` přejde WebCon lokálně do stavu `rendering`, načte strukturu a geometrii PVZ a zobrazí ji v ThingView. Viewer dnes nabízí Front, Top, Left, Right, výchozí ConfiRay izometrii, Fit, Zoom a relativní Orbit.

Po úspěšném zobrazení backend označí request `viewed`, naplánuje smazání PVZ a nakonec jej označí `cleaned`.

---

## Publisher a Deliver

PVZ + ThingView je implementovaný technologický základ Publish pro webovou vizualizaci.

**Plánováno / cílová architektura:** trvalé artefakty, výkresy, PDF, STEP, DXF, názvosloví, revize, nabídky, výrobní balíčky, PDM/PLM, ERP a řízené Deliver workflow.

Creo není server platformy. Creo je worker; backend řídí práci a web komunikuje s uživatelem.
