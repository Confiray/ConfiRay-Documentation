# 50 – Bezpečnost

## Princip

Bezpečnost je součástí architektury ConfiRay od začátku.

Tato kapitola odděluje funkční bezpečnostní základ od produkční governance. Současné demo má aplikační ochrany, ale není ještě produkčně zabezpečený Customer Portal.

---

## Současný stav dema

**DONE / FUNKČNÍ v současném demo rozsahu:**

- přihlášení pojmenovanou e-mailovou identitou a oddělená organization;
- salted PBKDF2 hash hesla v ignorovaném serverovém user store;
- expirovaná serverová session, HttpOnly/SameSite cookie a CSRF pro změnové browser operace;
- serverová volba organization → Connector bez možnosti klientského override;
- vlastnictví requestů, PVZ, cleanup a nabídek podle user/organization;
- webový klient nevolá Creo přímo, ale používá backend a FIFO requesty;
- Creo Worker kontroluje aktivní model a `A_CONFIGURATOR=CONVEYOR`;
- worker používá `workerId`, heartbeat lease a ownership statusu;
- backend omezuje formát request ID, vazbu requestu na PVZ, cesty metadata/help assets a cleanup pouze na dočasné soubory odpovídající povolenému názvu;
- offer PDF/DOCX endpoint nepovoluje path traversal ani obecné čtení souborů;
- HeliosConnector je read-only, používá parametrizované SELECT dotazy a credentials mimo Git/browser/log;
- veřejné spojení k demu zajišťuje HTTPS přes Cloudflare Tunnel.

**IN DEVELOPMENT / ROZPRACOVÁNO:** lokální identity/session store, jednoduché organization oprávnění a demo runtime. Chybí produkční identity provider, jemné role, tenant isolation, centralizovaný audit, secret manager, rate limiting všech citlivých operací, HA a incident response.

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

Toto oddělení, worker heartbeat a request ownership jsou implementované. Produkční service identity, certifikace workeru a distribuovaná autorizace jsou plánované.

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

---

## Governance & Access Control

**PLANNED / PLÁNOVÁNO:**

- named identities; žádné sdílení owner loginu;
- role a nejmenší potřebná oprávnění;
- vlastní Git branches pro změny;
- protected `main`, Pull Request a owner approval;
- oddělená prostředí DEV / TEST / PROD;
- zákaz production credentials v repozitáři, klientu, logu a test fixtures;
- ERP write pouze přes explicitní allowlistované operace, audit a schválení; default je read-only;
- audit identity, konfigurace, validace, CAD Jobu, nabídky, objednávky a administrativních změn;
- AI guardrails podle role: AI smí provádět jen operace povolené identitě a prostředí, s explicitními hranicemi pro Git, ERP, produkci a destruktivní akce.
