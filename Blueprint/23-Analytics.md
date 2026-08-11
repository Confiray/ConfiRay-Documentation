# 23 – Analytics

## Účel

Analytics poskytuje ConfiRay přehled o používání platformy, výkonu systému a průběhu zpracování požadavků.

Analytics není pouze statistický nástroj.

Je součástí provozního řízení platformy.

---

## Sledované údaje

Systém může sledovat například:

- počet uživatelů,
- počet přihlášených uživatelů,
- počet aktivních relací,
- počet konfigurací,
- počet generování,
- počet úspěšných generování,
- počet chyb,
- dobu zpracování,
- velikost fronty,
- vytížení workerů,
- vytížení serveru.

---

## Aktivita uživatelů

Administrátor může sledovat:

- kdo je přihlášen,
- kdo právě pracuje,
- kdo právě generuje,
- kdy uživatel naposledy provedl akci.

Citlivá data musí být zobrazována pouze oprávněným uživatelům.

---

## Job Analytics

Každý Job může obsahovat:

- ID,
- uživatele,
- čas vytvoření,
- čas zahájení,
- čas dokončení,
- stav,
- typ požadavku,
- výsledek,
- chybový stav,
- dobu zpracování.

---

## Historie vytížení

Systém může ukládat časové údaje o:

- CPU,
- RAM,
- vytížení workerů,
- velikosti fronty,
- počtu požadavků.

Data lze zobrazovat graficky.

---

## Účel

Analytics umožňuje zjistit:

- zda je systém dostatečně výkonný,
- kdy dochází k přetížení,
- kolik požadavků systém zvládne,
- jak dlouho trvá generování,
- zda je nutné přidat další Worker.

---

## Budoucnost

Analytics může být rozšířeno o:

- obchodní statistiky,
- využití jednotlivých produktů,
- nejčastější konfigurace,
- vytíženost zákazníků,
- SLA,
- náklad na jednotlivé generování.
