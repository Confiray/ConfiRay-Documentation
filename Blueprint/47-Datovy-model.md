# 47 – Datový model

## Účel

Datový model definuje základní objekty platformy ConfiRay.

---

## User

Uživatel platformy.

Základní údaje:

- ID,
- jméno,
- firma,
- e-mail,
- stav,
- oprávnění,
- datum registrace,
- poslední aktivita.

---

## Session

Aktivní připojení uživatele.

Obsahuje například:

- ID session,
- User,
- čas přihlášení,
- poslední aktivitu,
- stav.

---

## Product

Konfigurovatelný produkt.

Obsahuje:

- ID,
- název,
- verzi,
- konfigurátor,
- pravidla,
- CAD data.

---

## Configuration

Konkrétní konfigurace produktu.

Obsahuje:

- ID,
- User,
- Product,
- parametry,
- stav validace,
- čas vytvoření,
- verzi.

---

## Job

Požadavek na zpracování.

Obsahuje:

- ID,
- User,
- Configuration,
- typ,
- stav,
- čas vytvoření,
- čas spuštění,
- čas dokončení,
- Worker,
- výsledek,
- chybu.

---

## Worker

Výpočetní jednotka.

Obsahuje:

- ID,
- typ,
- stav,
- verzi,
- poslední heartbeat,
- aktuální Job.

---

## Artifact

Výstup vytvořený Jobem.

Například:

- STL,
- 3D model,
- PDF,
- DXF,
- STEP,
- obrázek.

---

## Audit Event

Událost systému.

Obsahuje:

- čas,
- uživatele,
- typ události,
- objekt,
- výsledek.

---

## Vztah

Zjednodušeně:

User
→ Configuration
→ Job
→ Worker
→ Artifact

Jeden User může mít mnoho Configuration.

Jedna Configuration může mít více Job.

Jeden Job může vytvořit více Artifact.
