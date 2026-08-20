# 47 – Datový model

## Účel

Datový model definuje základní objekty platformy ConfiRay.

Objekty níže představují cílový doménový model. Současný základ již používá lokální User, serverovou Session, Customer kontrakt, Request, Worker lease a Offer artefakty, ale dosud nemá produkční databázovou implementaci celé domény.

---

## Současný persistovaný model dema

**Implementováno a ověřeno:**

- `CurrentConfiguration` – lokální cache úplné konfigurace aktivního Creo modelu a čas synchronizace;
- `Request` – šestimístné ID, snapshot parametrů, stav, request-bound PVZ, časy vytvoření/aktualizace a případně zobrazení/cleanup;
- dočasný `Artifact` – `confiray_<requestId>.pvz` a nativní exportní log.
- `User` – pojmenovaná e-mailová identita, organization a volitelné serverové mapování na externí username;
- `Session` – expirovaná serverová session, CSRF a neveřejná supplier identity;
- `Customer` a `Contact` – jednotný Connector kontrakt nad JSON nebo read-only Helios002;
- `Offer` – číslo, vlastník/organization, customer/contact IDs, supplier contact ID, configuration snapshot, vazba na model request, render hash a DOCX/PDF artefakty.

Request snapshot se po zařazení do fronty nemění. Data jsou dnes ukládána atomicky do JSON souborů; nejde ještě o produkční databázový ani víceuživatelský model.

---

## Cílová persistence konfigurací a nabídek

**PLANNED / PLÁNOVÁNO:**

```text
Customer
└── Draft Offers
    └── Offer Items
        └── Configuration Snapshots
```

Každý Offer Item persistuje minimálně:

- product ID;
- configuration ID;
- revision;
- úplný configuration snapshot;
- validation state;
- vytvořený model;
- PVZ reference;
- render;
- později BOM a price.

Rozpracovanou nabídku lze znovu otevřít. Existující platný PVZ se při pouhém otevření znovu negeneruje. Reload WebConu obnovuje relevantní persistentní konfiguraci uživatele a zákazníka. Worker ani `CurrentConfiguration` nejsou autoritativní doménovou persistencí.

Multi-product Offer obsahuje více Offer Items a podporuje přidání, kopii, úpravu a odstranění položky. Každá položka zachovává vlastní product/revision/snapshot/model/artifacts.

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

Product odkazuje na verzovaný Product Package: Master CAD, Product Default, schema, rules, UI/help a mapování BOM/Price/Publisher. Nová Configuration vzniká z tohoto defaultu, nikoli z posledního stavu Creo workeru.

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

- PVZ a 3D render (**DONE / FUNKČNÍ** v dočasném request-bound workflow),
- DOCX/PDF nabídka (**IN DEVELOPMENT / ROZPRACOVÁNO** jako lokální artefakt),
- STEP, DXF a STL (**PLANNED / PLÁNOVÁNO**),
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

Cílový obchodní vztah rozšiřuje tento základ na:

`Identity → Organization → Customer → Draft Offer → Offer Item → Configuration Snapshot → Job → Artifact → Order → Project`.
