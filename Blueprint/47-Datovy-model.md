# 47 – Datový model

## Účel

Datový model definuje základní objekty platformy ConfiRay.

Objekty níže představují cílový doménový model. Současné technologické demo zatím nepoužívá plnou databázovou implementaci User, Session, Product, Configuration, Worker a Audit Event.

---

## Současný persistovaný model dema

**Implementováno a ověřeno:**

- `CurrentConfiguration` – lokální cache úplné konfigurace aktivního Creo modelu a čas synchronizace;
- `Request` – šestimístné ID, snapshot parametrů, stav, request-bound PVZ, časy vytvoření/aktualizace a případně zobrazení/cleanup;
- dočasný `Artifact` – `confiray_<requestId>.pvz` a nativní exportní log.

Request snapshot se po zařazení do fronty nemění. Data jsou dnes ukládána atomicky do JSON souborů; nejde ještě o produkční databázový ani víceuživatelský model.

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

V současné implementaci odpovídá tomuto objektu `Request`.

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

- STL (plánovaný exportní formát),
- 3D model,
- PDF,
- DXF,
- STEP,
- obrázek.

Současný ověřený Artifact je dočasný barevný PVZ pro ThingView. Trvalé artefakty, vlastnictví, oprávnění, revize a vazby na PDM/PLM jsou plánované.

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
