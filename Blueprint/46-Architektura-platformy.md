# 46 – Architektura platformy

## Stav cílové technické osy

Blueprint používá stavy **DONE / FUNKČNÍ**, **IN DEVELOPMENT / ROZPRACOVÁNO** a **PLANNED / PLÁNOVÁNO**.

**IDENTITY → CUSTOMER → PRODUCT → CONFIGURE → VALIDATE → CAD → BOM → PRICE → OFFER → ORDER → PROJECT**

| Oblast | Stav | Platný rozsah |
|---|---|---|
| Identity | **DONE / FUNKČNÍ** | Konkrétní e-mail, serverová session, CSRF, oddělená organization. |
| Customer | **DONE / FUNKČNÍ** | ALVARIS → read-only HeliosConnector; DEMO → JsonConnector/Virtual ERP; firmy, kontakty a supplier contact. |
| Product | **IN DEVELOPMENT / ROZPRACOVÁNO** | Conveyor je první metadata-driven produkt; obecný katalog čeká. |
| Configure + Validate | **DONE / FUNKČNÍ** | WebCon, HELP, `visibleIf`, RULES a validace WebCon → server → worker → Publisher. |
| CAD | **DONE / FUNKČNÍ** | FIFO, Creo Worker, heartbeat/lease, PVZ a ThingView. |
| BOM + Price | **PLANNED / PLÁNOVÁNO** | Struktura CAD → BOM → ERP ceny a obchodní podmínky. |
| Offer | **IN DEVELOPMENT / ROZPRACOVÁNO** | Funkční pětistránkový DOCX/PDF prototyp s dynamickým renderem, bez skutečné ceny a persistence draftu. |
| Order + Project | **PLANNED / PLÁNOVÁNO** | ERP objednávka, realizace, dokumentace, servis a historie. |

## Product Package a autorita konfigurace

**PLANNED / PLÁNOVÁNO:** každý produkt je verzovaný Product Package: Master CAD model, Product Default/start parameters, configuration schema, rules, UI definition, help, BOM mapping, Price mapping a Publisher mapping. Katalog má zahrnout Dopravník, pracovní stůl, rám, oplocení a další produkty.

Každá nová konfigurace musí vzniknout z Product Default/Masteru konkrétní revize. Worker je jen execution state; jeho poslední Creo stav není autoritativní konfigurace ani default dalšího uživatele.

## Persistence, nabídka a historie zákazníka

**PLANNED / PLÁNOVÁNO:** `Customer → Draft Offers → Offer Items → Configuration Snapshots`.

Offer Item persistuje product ID, configuration ID, revision, úplný snapshot, validation state, vytvořený model, PVZ reference a render; později BOM a price. Draft lze znovu otevřít a existující platný PVZ se při pouhém otevření neregeneruje. Reload WebConu obnoví poslední relevantní konfiguraci uživatele/zákazníka, nikoli stav workeru.

Multi-product nabídka nabídne Přidat do nabídky, Kopírovat, Upravit a Odstranit. Historie zákazníka zpřístupní nabídky, objednávky, projekty, realizované výrobky, objednání nabídky a vytvoření nové varianty ze staršího snapshotu.

## Standardní 3D a AR

**DONE / FUNKČNÍ:** standardní 3D používá request-bound PVZ a ThingView.

**PLANNED / PLÁNOVÁNO:** paralelní `configured CAD → GLB/glTF → AR` podporuje 1:1 scale, umístění/rotaci, QR desktop → mobile a později rozměry, servisní zóny a anotace. Pro více produktů platí `Current Offer → Layout → AR Scene / Assembly Preview`; položka nese model, X/Y/Z, rotation a případné connection points.

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

Requesty jsou dnes atomicky zapisované JSON soubory a nesou vlastníka i organizaci. Konkrétní e-mailové identity, serverové session a CSRF jsou funkční; produkční identity provider, databáze, tenant isolation, robustní retry a distribuovaná fronta jsou cílová architektura.

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

Worker používá unikátní `workerId`, 5s heartbeat, 90s lease, request ownership a orphan recovery do `failed`. Více workerů a plánování podle Product Package jsou plánované.

---

## Stavy a ThingView

Persistovaný stav requestu je:

`queued → processing → ready`

nebo `failed`.

Po `ready` přejde WebCon lokálně do stavu `rendering`, načte strukturu a geometrii PVZ a zobrazí ji v ThingView. Viewer dnes nabízí Front, Top, Left, Right, výchozí ConfiRay izometrii, Fit, Zoom a relativní Orbit.

Po úspěšném zobrazení backend označí request `viewed`, naplánuje smazání PVZ a nakonec jej označí `cleaned`.

---

## Publisher a Deliver

PVZ + ThingView je **DONE / FUNKČNÍ** technologický základ Publish pro webovou vizualizaci.

Serverově ověřený Publisher nabídky je **IN DEVELOPMENT / ROZPRACOVÁNO**: vytváří pětistránkový DOCX/PDF s firmou, vybraným kontaktem, supplier contactem, technickým snapshotem a dynamickým renderem konkrétního modelu. Cena je zatím demo text a nabídka není persistentní multi-product doména.

**PLANNED / PLÁNOVÁNO:** trvalé draft offers a artefakty, BOM, skutečný Price, výkresy, STEP, DXF, názvosloví, revize, výrobní balíčky, PDM/PLM, Order, Project a řízené Deliver workflow.

Creo není server platformy. Creo je worker; backend řídí práci a web komunikuje s uživatelem.
