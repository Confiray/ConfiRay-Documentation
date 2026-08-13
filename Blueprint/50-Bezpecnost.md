# 50 – Bezpečnost

## Princip

Bezpečnost je součástí architektury ConfiRay od začátku.

Tato kapitola popisuje především produkční požadavky. Současné technologické demo není produkčně zabezpečený Customer Portal.

---

## Současný stav dema

**Částečně implementováno:**

- webový klient nevolá Creo přímo, ale používá backend a FIFO requesty;
- Creo Worker kontroluje aktivní model a `A_CONFIGURATOR=CONVEYOR`;
- backend omezuje formát request ID, vazbu requestu na PVZ, cesty metadata/help assets a cleanup pouze na dočasné soubory odpovídající povolenému názvu;
- veřejné spojení k demu zajišťuje HTTPS přes Cloudflare Tunnel.

**Dosud neimplementováno:** aplikační přihlášení, autorizace endpointů, tenant isolation, vlastnictví requestů, chráněné stahování výstupů, audit uživatelů, rate limiting a správa tajemství pro produkční provoz.

---

## Uživatelské účty

Každý uživatel má vlastní účet.

Přístup je řízen oprávněními.

---

## Role

Minimální rozdělení:

- Customer
- Operator
- Administrator

Další role mohou být přidány podle potřeby.

---

## Oddělení zákazníků

Data jednotlivých zákazníků musí být logicky oddělena.

Uživatel nesmí získat přístup ke konfiguracím nebo výstupům jiného zákazníka.

---

## Administrace

Administrativní funkce jsou dostupné pouze oprávněným uživatelům.

---

## Creo Worker

Worker nesmí být přímo ovládán veřejným uživatelem.

Veškeré požadavky musí procházet Backendem.

Toto oddělení je v současném demu implementované, ale backend endpointy ještě vyžadují produkční autentizaci a autorizaci.

---

## Výstupní soubory

Výsledné soubory musí být přístupné pouze oprávněným uživatelům.

---

## Audit

Důležité operace musí být zaznamenávány.

Například:

- login,
- logout,
- vytvoření konfigurace,
- vytvoření Job,
- dokončení Job,
- chyba,
- změna oprávnění.

---

## Budoucí rozšíření

Produkční instalace musí počítat s:

- šifrovanou komunikací,
- bezpečným ukládáním hesel,
- řízením session,
- zálohováním,
- monitoringem,
- řízením přístupů.
