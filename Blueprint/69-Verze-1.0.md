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
15. přihlásit pojmenovaného uživatele konkrétním e-mailem a oddělit identity od organization;
16. serverově zvolit DEMO JsonConnector nebo ALVARIS read-only HeliosConnector;
17. načíst firmu, kontaktní osobu a serverově ověřený supplier contact;
18. provozovat worker s `workerId`, 5s heartbeat, 90s lease, ownership a orphan recovery;
19. validovat konstrukční pravidla na WebCon/server/worker/Publisher hranici;
20. vytvořit pětistránkový DOCX/PDF prototyp nabídky s dynamickým renderem;
21. spustit nebo restartovat runtime jednotným START/RESTART launcherem s readiness kontrolou.

PVZ nahradil původně uvažovaný STL technologický test, protože zachovává vzhled a je přímo použitelný v současném ThingView workflow.

---

## Částečně implementováno

- Customer Portal dnes existuje jako autentizovaný WebCon demo vstup s organization Connector mappingem, nikoli jako plný portál s rolemi, drafty a historií.
- Backend, API a fronta jsou funkční lokální základ nad JSON soubory, nikoli produkční distribuovaná služba.
- Publish vytváří dočasný webový PVZ a funkční DOCX/PDF nabídku, nikoli úplný revizně řízený nabídkový a výrobní balík.
- Jeden Creo Worker zpracovává requesty sériově; heartbeat a recovery jsou funkční, více workerů není hotových.

---

## Plánováno pro produkční 1.0 a navazující fáze

- produkční identity provider, role, tenant isolation a audit,
- trvalé konfigurace, historie a artefakty,
- produkční monitoring, retry, HA a service management,
- další produktové konfigurátory,
- automatické end-to-end testy a deployment packaging,
- persistentní multi-product Draft Offers a plný Publisher pro výkresy, STEP, DXF a výrobní data,
- Price/CPQ,
- Deliver a integrace ERP, CRM a PDM/PLM,
- více Creo Workerů podle licencování a zatížení.

Funkční WebCon + Creo + PVZ + ThingView workflow je základ cílové architektury, nikoli důkaz dokončení celé ConfiRay Automation platformy.
