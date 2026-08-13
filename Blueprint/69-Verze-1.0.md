# 69 – Verze 1.0

## Cíl

Verze 1.0 ověřuje základní princip automatizovaného generování platné konfigurace prostřednictvím webového rozhraní a odděluje tento technologický milník od plné produkční platformy.

Referenčním produktem je Conveyor.

---

## Implementováno a ověřeno

Současný technologický/demo základ umožňuje:

1. načíst WebCon z `demo.confiray.cz` přes Cloudflare Tunnel;
2. vytvořit formulář ze sdílených `PARAMETERS`, `RULES` a `HELP`;
3. načíst konfiguraci aktivního Conveyor modelu z Creo;
4. upravit a validovat parametry;
5. odeslat při Publish neměnný request snapshot;
6. zařadit request do persistentní FIFO fronty a zobrazit `queuePosition`;
7. převzít request jedním Creo Workerem;
8. ověřit správný aktivní konfigurátor;
9. nastavit parametry a automaticky regenerovat Creo model;
10. exportovat request-bound barevný PVZ;
11. zobrazit stavy `queued`, `processing`, klientský `rendering`, `ready` nebo chybu;
12. načíst PVZ v ThingView;
13. ovládat standardní pohledy, ConfiRay izometrii, Fit, Zoom a relativní Orbit;
14. po zobrazení naplánovat cleanup dočasného PVZ a logů.

PVZ nahradil původně uvažovaný STL technologický test, protože zachovává vzhled a je přímo použitelný v současném ThingView workflow.

---

## Částečně implementováno

- Customer Portal dnes existuje jako veřejný WebCon demo vstup, nikoli jako plný portál s účty a historií.
- Backend, API a fronta jsou funkční lokální základ nad JSON soubory, nikoli produkční distribuovaná služba.
- Publish vytváří dočasný webový PVZ, nikoli úplný balík nabídkové a výrobní dokumentace.
- Jeden Creo Worker zpracovává requesty sériově; více workerů a recovery nejsou hotové.

---

## Plánováno pro produkční 1.0 a navazující fáze

- autentizace, autorizace a oddělení zákazníků,
- trvalé konfigurace, historie a artefakty,
- produkční monitoring, heartbeat, retry a obnova po výpadku,
- další produktové konfigurátory,
- automatické end-to-end testy a deployment packaging,
- plný Publisher pro nabídky, výkresy, PDF, STEP, DXF a výrobní data,
- Price/CPQ,
- Deliver a integrace ERP, CRM a PDM/PLM,
- více Creo Workerů podle licencování a zatížení.

Funkční WebCon + Creo + PVZ + ThingView workflow je základ cílové architektury, nikoli důkaz dokončení celé ConfiRay Automation platformy.
