# 24 – Administration

## Účel

Administration je interní prostředí pro správu platformy ConfiRay.

Je určeno pouze oprávněným pracovníkům ConfiRay.

---

## Uživatelé

Administrátor může:

- zobrazit uživatele,
- zobrazit nové registrace,
- aktivovat nebo deaktivovat účet,
- měnit oprávnění,
- zobrazit historii aktivity.

---

## Aktivní uživatelé

Administrace zobrazuje:

- počet aktivních uživatelů,
- aktuálně přihlášené uživatele,
- aktivní konfigurace,
- právě probíhající generování.

---

## Job Queue

Administrátor může sledovat:

- počet úloh ve frontě,
- právě zpracovávanou úlohu,
- dokončené úlohy,
- chybné úlohy.

---

## Creo Workers

Každý Creo Worker má stav.

Například:

- ONLINE
- IDLE
- BUSY
- ERROR
- OFFLINE

Administrace umožňuje sledovat aktuální stav workerů.

---

## Server

Administrátor může sledovat:

- CPU,
- RAM,
- disk,
- síť,
- počet běžících procesů,
- stav backendu,
- stav databáze,
- stav Job Queue.

---

## Historie

Administrace uchovává provozní historii.

Historie může obsahovat:

- přihlášení,
- registrace,
- generování,
- chyby,
- změny konfigurace,
- změny oprávnění,
- stav workerů.

---

## Bezpečnost

Administrativní rozhraní nesmí být dostupné běžným zákazníkům.

Přístup musí být řízen oprávněními.
