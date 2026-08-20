# 51 – Výkon

## Základní princip

Webové rozhraní nesmí být blokováno dlouhotrvajícími CAD operacemi.

Generování je proto asynchronní.

---

## Job Queue

Požadavky jsou ukládány do fronty.

Například:

Request 000001 → `queued`
Request 000002 → `queued`
Request 000003 → `queued`

Worker postupně zpracovává jednotlivé Jobs.

**Implementováno a ověřeno:** requesty jsou persistované lokálně, dostávají vzestupné šestimístné ID a worker atomicky přebírá nejnižší queued ID. Tím vzniká FIFO pořadí.

---

## Současní uživatelé

Více uživatelů může požádat o generování současně.

Každý požadavek dostane vlastní Job ID.

Pořadí zpracování je řízeno Queue.

WebCon zobrazuje vlastní `queuePosition`, aniž by klientovi zpřístupňoval obsah ostatních requestů.

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

Současné demo používá jeden Creo Worker a serializované zpracování. Distribuce práce mezi více workerů zatím není implementovaná.

---

## Monitoring

Systém sleduje:

- velikost Queue,
- dobu čekání,
- dobu zpracování,
- počet chyb,
- stav Workerů.

Současný základ ukládá stav a časy requestu, worker posílá 5s heartbeat a backend používá 90s lease s orphan recovery. `/api/runtime-status` poskytuje necitlivou readiness pro startup. Plná telemetrie, dashboard, SLA, kapacitní plánování, persistentní worker registry a automatický retry jsou **PLANNED / PLÁNOVÁNO**.

---

## Optimalizace

Prioritou je:

1. rychlá odezva webu,
2. stabilní zpracování,
3. správnost výsledku,
4. až následně maximální propustnost.
