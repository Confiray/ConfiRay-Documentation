# 69 – Verze 1.0

## Cíl

První verze ConfiRay ověřuje základní princip automatizovaného generování konfigurace prostřednictvím webového rozhraní.

---

## Referenční produkt

Prvním demonstračním produktem je konfigurátor dopravníku.

---

## Minimální funkční celek

Verze 1.0 musí umožnit:

1. uživatel otevře web,
2. přihlásí se,
3. zobrazí konfigurátor,
4. změní parametry,
5. stiskne Generate,
6. Backend vytvoří Job,
7. Job je zařazen do Queue,
8. Creo Worker Job převezme,
9. Creo provede konfiguraci,
10. Publisher vytvoří výstup,
11. výstup je uložen,
12. Web zobrazí nový 3D model.

---

## První výstup

První výstupní formát je STL.

STL je pouze technologický test.

Cílový 3D formát bude řešen následně s ohledem na:

- barvy,
- vzhled,
- velikost souboru,
- výkon webového Vieweru,
- možnost zobrazovat technické informace.

---

## Současní uživatelé

Verze 1.0 musí být navržena s Job Queue.

To umožní současné požadavky více uživatelů.

První testovací prostředí může používat pouze jeden Creo Worker.

---

## Vývojové prostředí

První verze může běžet na vývojovém notebooku.

Notebook slouží jako první ConfiRay Server.

---

## Další fáze

Po ověření základního řetězce budou následovat:

- registrace,
- administrace,
- Analytics,
- historie,
- barevný 3D formát,
- další produkty,
- ERP,
- nabídky,
- více Workerů.
