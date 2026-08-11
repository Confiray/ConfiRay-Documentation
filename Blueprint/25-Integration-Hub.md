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

---

## Asynchronní komunikace

Operace, které mohou trvat delší dobu, nesmí blokovat webové rozhraní.

Požadavek je proto vytvořen jako Job.

Web sleduje jeho stav.

Možné stavy:

- QUEUED
- RUNNING
- COMPLETED
- FAILED
- CANCELLED

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
