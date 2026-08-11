# 51 – Výkon

## Základní princip

Webové rozhraní nesmí být blokováno dlouhotrvajícími CAD operacemi.

Generování je proto asynchronní.

---

## Job Queue

Požadavky jsou ukládány do fronty.

Například:

JOB 001 → QUEUED
JOB 002 → QUEUED
JOB 003 → QUEUED

Worker postupně zpracovává jednotlivé Jobs.

---

## Současní uživatelé

Více uživatelů může požádat o generování současně.

Každý požadavek dostane vlastní Job ID.

Pořadí zpracování je řízeno Queue.

---

## Worker

Jeden Creo Worker zpracovává jednu Creo úlohu v daném okamžiku.

To odpovídá principu jedné aktivní Creo licence / instance.

---

## Škálování

Výkon lze zvýšit přidáním dalších Workerů.

Například:

1 Worker
→ 1 současně zpracovávaný Job

3 Workery
→ až 3 současně zpracovávané Jobs

Konkrétní možnosti závisí na licencování a technickém prostředí.

---

## Monitoring

Systém sleduje:

- velikost Queue,
- dobu čekání,
- dobu zpracování,
- počet chyb,
- stav Workerů.

---

## Optimalizace

Prioritou je:

1. rychlá odezva webu,
2. stabilní zpracování,
3. správnost výsledku,
4. až následně maximální propustnost.
