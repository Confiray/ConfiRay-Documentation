# 22 – Customer Portal

## Účel

Customer Portal je webové prostředí, prostřednictvím kterého zákazník komunikuje s platformou ConfiRay.

Umožňuje zákazníkovi vstoupit do ConfiRay bez nutnosti instalovat CAD software nebo pracovat přímo v prostředí Creo.

Customer Portal je jedním z hlavních rozhraní mezi ConfiRay a zákazníkem.

---

## Veřejná část

Veřejná část webu představuje ConfiRay a umožňuje návštěvníkovi:

- získat základní informace o platformě,
- prohlédnout fotografie a videa,
- pochopit princip konfigurace,
- získat informace o možnostech platformy,
- požádat o přístup k demonstračnímu prostředí.

Hlavním vstupem do testovacího prostředí je možnost:

**Vyzkoušet ConfiRay online**

---

## Registrace

Před vstupem do demonstračního prostředí může být požadována registrace.

Minimální údaje:

- jméno,
- firma,
- e-mail,
- případně telefon,
- souhlas s podmínkami používání.

Registrace vytvoří uživatelský účet nebo požadavek na jeho schválení.

Administrátor ConfiRay musí mít možnost nové registrace zobrazit a spravovat.

---

## Přihlášení

Registrovaný uživatel se přihlašuje do Customer Portal.

Po přihlášení získá přístup pouze k funkcím, které mu byly přiděleny.

V demonstrační fázi může být uživateli zpřístupněn například:

- demo produkt,
- Configurator,
- 3D náhled,
- Generate,
- Publish,
- historie vlastních konfigurací.

---

## Online konfigurace

Customer Portal umožňuje zadávání parametrů produktu prostřednictvím webového rozhraní.

Webový konfigurátor nemusí být kopií desktopového Configuratoru.

Musí však používat stejný datový a pravidlový základ.

Změna parametrů na webu vytvoří požadavek na zpracování.

---

## Generování

Po stisknutí tlačítka Generate:

1. web odešle konfiguraci do backendu,
2. backend vytvoří úlohu,
3. úloha je zařazena do fronty,
4. Creo Worker úlohu převezme,
5. Creo provede konfiguraci,
6. Publisher vytvoří požadovaný výstup,
7. výsledek je uložen,
8. web obdrží informaci o dokončení,
9. 3D výsledek se zobrazí zákazníkovi.

---

## Současní uživatelé

Systém musí být navržen tak, aby více uživatelů mohlo současně požadovat generování.

Jednotlivé požadavky nejsou zpracovávány přímo proti webovému rozhraní.

Požadavky jsou předávány do Job Queue.

Jednotlivý Creo Worker zpracovává jednu úlohu v daném okamžiku.

Více workerů může být přidáno později.

---

## Historie

Uživatel může vidět historii svých konfigurací a generování.

Historie může obsahovat:

- datum,
- čas,
- konfiguraci,
- stav,
- výsledek,
- případnou chybu,
- dobu zpracování.

---

## Princip

Customer Portal odděluje zákazníka od technického prostředí Creo.

Zákazník pracuje pouze s webovým rozhraním.

Creo zůstává výpočetním motorem ConfiRay.

---

## Budoucí rozšíření

Customer Portal může být postupně rozšířen o:

- nabídky,
- ceny,
- objednávky,
- dokumentaci,
- stav zakázky,
- komunikaci,
- ERP data,
- servis,
- zákaznickou historii.
