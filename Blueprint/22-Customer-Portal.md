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

### Současný stav

**DONE / FUNKČNÍ:** WebCon generuje responsive pole ze stejných `PARAMETERS`, `RULES` a `HELP` jako Creo Configurator, nabízí kontextovou nápovědu a preview enum variant, validuje změny a odesílá neměnný snapshot. Pojmenovaný uživatel se přihlašuje konkrétním e-mailem; organization serverově volí DEMO JsonConnector nebo ALVARIS HeliosConnector. Pravý panel vyhledá firmu a její kontaktní osoby.

**IN DEVELOPMENT / ROZPRACOVÁNO:** funkční tlačítko Vytvořit nabídku generuje serverově ověřený DOCX/PDF s dynamickým renderem. Chybí trvalý draft, skutečná cena a multi-product nabídka.

**PLANNED / PLÁNOVÁNO:** obecný produktový katalog, persistentní konfigurace, role, zákaznická historie, objednávky a projekty.

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

V současném demu tuto CAD roli plní **VYTVOŘIT MODEL**: backend vytvoří request, FIFO fronta jej předá Creo Workeru, Creo model automaticky regeneruje, vznikne dočasný PVZ a WebCon jej zobrazí v ThingView. Web ukazuje `queuePosition`, zpracování, vykreslování a chybu. **VYTVOŘIT NABÍDKU** následně používá právě ready snapshot/model, zákazníka, kontakt a dynamický render.

---

## Současní uživatelé

Systém musí být navržen tak, aby více uživatelů mohlo současně požadovat generování.

Jednotlivé požadavky nejsou zpracovávány přímo proti webovému rozhraní.

Požadavky jsou předávány do Job Queue.

Jednotlivý Creo Worker zpracovává jednu úlohu v daném okamžiku.

Více workerů může být přidáno později.

**Současný stav:** persistentní lokální FIFO fronta, user/organization ownership, serializované zpracování jedním Creo Workerem, heartbeat lease a orphan recovery jsou implementované. Produkční tenant isolation, retry a distribuce na více workerů jsou plánované.

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

**PLANNED / PLÁNOVÁNO:** po výběru zákazníka se zobrazí jeho nabídky, objednávky, projekty a realizované výrobky. Draft offer lze znovu otevřít, objednat nebo použít jako základ nové varianty. Reload WebConu obnoví relevantní persistentní configuration snapshot, nikoli poslední execution state workeru.

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
