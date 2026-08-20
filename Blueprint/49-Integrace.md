# 49 – Integrace

## Princip

ConfiRay je integrační platforma.

Jednotlivé systémy komunikují přes definovaná rozhraní.

---

## CAD

Creo je hlavním CAD výpočetním prostředím pro automatizované generování.

Komunikace probíhá prostřednictvím Creo Workeru.

**Implementováno a ověřeno:** Worker běží v Creo relaci, ověřuje správný Conveyor konfigurátor, čte a zapisuje parametry podle sdílených metadat, automaticky regeneruje aktivní model a publikuje request-bound PVZ. Aktuální konfiguraci synchronizuje do lokálního backendu; WebCon Creo přímo neovládá.

---

## ERP

ERP poskytuje zejména:

- ceny,
- zákaznická data,
- materiálová data,
- skladová data,
- výrobní data,
- zakázky.

Konkrétní rozsah závisí na implementaci.

**DONE / FUNKČNÍ v read-only Customer rozsahu:** organizace ALVARIS používá serverový HeliosConnector nad cvičnou databází Helios002. Parametrizované SELECT dotazy načítají firmy, kontaktní osoby, místa určení, obchodní údaje a supplier contact přihlášeného uživatele. Organizace DEMO používá JsonConnector/Virtual ERP. Connector volí výhradně server podle organization; credentials jsou mimo Git a browser.

**PLANNED / PLÁNOVÁNO:** BOM, materiály, ERP ceny, objednávky, výrobní data a jakýkoli řízený zápis. Současný HeliosConnector je striktně read-only a neposkytuje obecné arbitrary SQL API.

---

## CRM

CRM může poskytovat:

- zákazníky,
- kontakty,
- obchodní případy,
- nabídky,
- historii komunikace.

CRM integrace je plánovaná.

---

## PDM / PLM

PDM/PLM může sloužit jako úložiště:

- CAD dat,
- dokumentace,
- revizí,
- výrobních dat.

Plný Publisher a PDM/PLM workflow zatím nejsou implementované. Současný PVZ je request-bound dočasný webový výstup a Publisher nabídky vytváří DOCX/PDF prototyp; ani jeden ještě není řízený PDM artefakt s revizí a dlouhodobým lifecycle.

---

## Princip oddělení

ConfiRay nesmí být pevně svázán s jedním konkrétním ERP, CRM nebo PDM.

Integrace musí být modulární.

Společný Connector kontrakt odděluje WebCon a Publisher od konkrétního ERP. Cílový cenový tok je **CAD → BOM → ERP → Price → Offer** a nesmí přebírat cenu autoritativně z browseru.
