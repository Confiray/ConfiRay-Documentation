# 46 – Architektura platformy

## Základní princip

ConfiRay je vícevrstvá platforma.

Základní architektura:

Web / Customer Portal
↓
Backend
↓
Job Queue
↓
Creo Worker
↓
Publisher
↓
3D / dokumentace / další výstupy

---

## Web

Webová část zajišťuje:

- prezentaci,
- přihlášení,
- konfiguraci,
- zobrazení stavu,
- 3D vizualizaci,
- komunikaci se zákazníkem.

Web nesmí být přímo závislý na interních funkcích Creo.

---

## Backend

Backend je centrální řídicí vrstva.

Zajišťuje:

- uživatele,
- autentizaci,
- konfigurace,
- Jobs,
- frontu,
- historii,
- komunikaci s Workery,
- administraci,
- API.

---

## Job Queue

Job Queue odděluje požadavek uživatele od jeho skutečného zpracování.

To umožňuje:

- více současných uživatelů,
- dávkové zpracování,
- opakování úloh,
- sledování stavu,
- pozdější přidání workerů.

---

## Creo Worker

Creo Worker je výpočetní jednotka ConfiRay.

Obsahuje:

- Creo,
- ConfiRay Configurator,
- Publisher,
- potřebná data,
- komunikaci s Backendem.

Jeden Worker zpracovává v daném okamžiku jednu Creo úlohu.

---

## Škálování

Základní instalace může obsahovat:

- jeden Backend,
- jednu Job Queue,
- jeden Creo Worker.

Větší instalace může obsahovat:

- jeden Backend,
- jednu Job Queue,
- více Creo Workerů.

---

## Výstup

Výsledkem Workeru může být:

- STL,
- barevný 3D formát,
- 3D náhled,
- výkres,
- PDF,
- další výrobní dokumentace.

Formát STL je používán v první fázi jako testovací výstup.

---

## Základní princip

Creo není server platformy.

Creo je Worker.

Backend řídí práci.

Web komunikuje se zákazníkem.

Job Queue řídí pořadí požadavků.
