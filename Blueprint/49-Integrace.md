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

ERP integrace zatím není součástí funkčního dema.

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

Plný Publisher a PDM/PLM workflow zatím nejsou implementované. Současný PVZ je dočasný webový výstup, nikoli řízený PDM artefakt.

---

## Princip oddělení

ConfiRay nesmí být pevně svázán s jedním konkrétním ERP, CRM nebo PDM.

Integrace musí být modulární.
