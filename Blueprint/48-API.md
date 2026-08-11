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

---

## Heartbeat

Worker pravidelně informuje Backend, že je aktivní.

Backend tak může rozlišit:

- ONLINE,
- BUSY,
- ERROR,
- OFFLINE.

---

## Verze API

API musí být verzované.

Budoucí změny nesmí automaticky rozbít starší klienty nebo Workery.

---

## Bezpečnost

API nesmí být veřejně dostupné bez autentizace a autorizace.

Citlivé operace musí být chráněny.
