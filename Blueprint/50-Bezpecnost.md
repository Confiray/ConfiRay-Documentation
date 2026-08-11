# 50 – Bezpečnost

## Princip

Bezpečnost je součástí architektury ConfiRay od začátku.

---

## Uživatelské účty

Každý uživatel má vlastní účet.

Přístup je řízen oprávněními.

---

## Role

Minimální rozdělení:

- Customer
- Operator
- Administrator

Další role mohou být přidány podle potřeby.

---

## Oddělení zákazníků

Data jednotlivých zákazníků musí být logicky oddělena.

Uživatel nesmí získat přístup ke konfiguracím nebo výstupům jiného zákazníka.

---

## Administrace

Administrativní funkce jsou dostupné pouze oprávněným uživatelům.

---

## Creo Worker

Worker nesmí být přímo ovládán veřejným uživatelem.

Veškeré požadavky musí procházet Backendem.

---

## Výstupní soubory

Výsledné soubory musí být přístupné pouze oprávněným uživatelům.

---

## Audit

Důležité operace musí být zaznamenávány.

Například:

- login,
- logout,
- vytvoření konfigurace,
- vytvoření Job,
- dokončení Job,
- chyba,
- změna oprávnění.

---

## Budoucí rozšíření

Produkční instalace musí počítat s:

- šifrovanou komunikací,
- bezpečným ukládáním hesel,
- řízením session,
- zálohováním,
- monitoringem,
- řízením přístupů.
