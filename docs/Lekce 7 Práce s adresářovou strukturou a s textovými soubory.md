# Lekce 7: Práce s adresářovou strukturou a s textovými soubory

V této lekci si probereme, jak se v Pythonu dá zacházet s adresářovou strukturou počítače (nastavování pracovního adresáře, tvorba adresářových cest, tvorba a mazání adresářů apod.) a jak lze číst a zapisovat textové soubory. Přitom si zároveň rozšíříme možnosti práce s textovými řetězci.

## Zápis adresářových cest

V Pythonu je výchozím způsobem, jak zapsat adresářovou cestu, klasický styl se zpětnými lomítky, např. `C:\Moje_slozka\muj_soubor.txt`. Problém je, že zpětné lomítko, jak víme, má v textových řetězcích speciální význam (uvozuje *escape* sekvenci). To lze obejít třemi způsoby:

1. V zápisu cest použijeme dopředná lomítka: `"C:/Moje_slozka/muj_soubor.txt"`. S tím si Python poradí. 
2. Použijeme escape sekvenci `\\`, tj. dvojitá zpětná lomítka: `"C:\\Moje_slozka\\muj_soubor.txt"`. Oba popsané způsoby jsou jistě výhodné tehdy, chceme-li cesty vypisovat ručně. Často ale využíváme např. toho, že si cestu zkopírujeme z nějakého prohlížeče a následně vložíme do kódu. Takové cesty mívají zpětná lomítka a jejich následná editace (záměna za dopředná či dvojitá zpětná lomítka) je zdlouhavá a nepraktická. Z takovém případě se hodí třetí způsob:
3. Před textový řetězec napíšeme `r`, čímž z něj vytvoříme "surový" (angl. "raw") řetězec, jehož všechny znaky se interpretují tak, jak jsou (tj. řetězec neobsahuje žádné escape sekvence): `r"C:\Moje_slozka\muj_soubor.txt"`.

Přehled všech tří metod:

```python
# Dopředná lomítka
path = "C:/Moje_slozka/muj_soubor.txt"

# Dvojitá zpětná lomítka
path = "C:\\Moje_slozka\\muj_soubor.txt"

# Surový řetězec
path = r"C:\Moje_slozka\muj_soubor.txt"
```

## Modul `os`

Jméno modulu je zkratkou slov operační systém (anglicky "operating system"). Modul je určen pro správu adresářů a procesů. Z mnoha funkcí modulu budou pro nás užitečné zvláště některé pro správu adresářů. Parametr `path` (anglicky "cesta") má ve všech ukázkách význam textového řetězce s adresou složky nebo souboru.

Modul načteme standardním způsobem:

```python
import os
```

Při operacích s adresářovou strukturou je výhodné mít správně nastaven tzv. **pracovní adresář**  (angl. "workspace direcory"). Je-li nastaven, nemusíme např. při zadávání vstupních souborů do nějakého výpočtu zadávat celou cestu k souboru - pokud je vstupní soubor v pracovním adresáři, interpret Pythonu jej bude hledat tam. Rovněž jakékoli výstupní soubory, pokud neurčíme celou cestu, kam mají být uloženy, se budou ukládat do pracovního adresáře. Výchozí nastavení pracovního adresáře závisí na tom, jak jsme program IDLE spustili:

- Při otevření programu IDLE standardním způsobem z nabídky programů je pracovní adresář nastaven na výchozí pozici, typicky třeba `'C:\\WINDOWS\\system32'`. (Pokud budeme nyní chtít např. otevřít nějaký skript stisknutím kláves `Ctrl`+`O`, vyhledávací okno se otevře právě v tomto umístění.)

- Při otevření programu IDLE "ze skriptu", tj. tím, že skript otevřeme pro editaci v programu IDLE, je pracovní adresář automaticky nastaven na složku s tímto skriptem. 

Pro zjištění aktuálního pracovního adresáře slouží funkce `getcwd()` (z angl. "current workspace directory"):

```python
>>> os.getcwd()
'C:\\Documents and Settings\\Muj_pocitac'
```

Pokud chceme naopak pracovní adresář změnit, použijeme funkci `chdir(path)` (z angl. "change directory"). Pozor: příslušné umístění `path` musí existovat!

```python
>>> os.chdir("C:/Moje_slozka")
>>> os.getcwd()
'C:\\Moje_slozka'
```

Často chceme nějaký výpočet provést v cyklu pro všechny soubory v nějaké složce, případně pro všechny složky v nějaké nadřazené složce. Abychom získali seznam položek uvnitř nějaké složky, použijeme funkci `listdir(path)`. Výstupní seznam je seznamem názvů (tj. řetězců) položek, včetně koncovek:

```python
>>> os.listdir("C:/Moje_slozka")
['Dalsi_soubor.xls', 'Druhy_soubor.txt', 'Prvni_soubor.doc']
```

Často se potřebujeme v podobných funkcích odkázat na aktuální pracovní adresář (např. vypsat jeho obsah). To je možné pomocí již zmíněné funkce `getcwd`:

```python
>>> os.listdir(getcwd())
['Dalsi_soubor.xls', 'Druhy_soubor.txt', 'Prvni_soubor.doc']
```

Existuje však i jednodušší způsob. V Pythonu se totiž lze odkazovat na pracovní adresář zkratkou `"."`:

```python
>>> os.listdir(".")
['Dalsi_soubor.xls', 'Druhy_soubor.txt', 'Prvni_soubor.doc']
```

> **Úkol 1.** Změňte svůj pracovní adresář na nějakou složku, ve které máte větší množství souborů různých typů. Následně vypište její obsah na konzoli.


Další dvě funkce, které si ukážeme, slouží k vytváření nových adresářů. První, funkce `mkdir(path)`, vytvoří na dané adrese novou složku. Podmínkou je, že celá cesta kromě koncové vytvářené složky existuje. TUto podmínku obchází druhá funkce, `makedirs(path)`, která vytvoří na dané adrese novou složku, včetně všech případných neexistujících nadřazených složek.

```python
>>> os.mkdir("C:/nova_slozka")
>>> os.makedirs("C:/nejaka_nadslozka/nova_slozka")
```

Pokud naopak chceme nějakou složku či soubor smazat, máme k dispozici následující funkce:

```python
# Smazání souboru
>>> os.remove("C:/Moje_slozka/muj_soubor.doc")

# Smazání (prázdné!) složky
>>> os.rmdir("C:/nova_slozka")

# Smazání (prázdné!) složky včetně všech (prázdných!) nadsložek
>>> os.removedirs("C:/ nejaka_nadslozka/nova_slozka")
```

Užitečné jsou rovněž funkce `rename(old, new)` a `renames(old_path, new_path)`, pomocí kterých lze přejmenovávat cesty k souborů, a tím nejen měnit jejich název, ale i soubory přesouvat na nová umístění. První z nich, `rename`, přejmenuje cílový soubor ze staré cesty na novou, složka nového umístění souboru však již musí existovat. Druhá funkce, `renames`, toto nepožaduje a potřebné neexistující složky v nové cestě nejprve vytvoří (a zároveň odstraní již nepotřebné složky původní cesty).

```python
# Přejmenování souboru
os.listdir("C:/nova_slozka")
['Dalsi_soubor.xls', 'Druhy_soubor.txt', 'Prvni_soubor.doc']
os.rename("C:/nova_slozka/Prvni_soubor.doc", "C:/nova_slozka/Treti_soubor.doc")
os.listdir("C:/nova_slozka")
['Dalsi_soubor.xls', 'Druhy_soubor.txt', 'Treti_soubor.doc']

# Přesunutí souboru
os.renames("C:/nova_slozka/Prvni_soubor.doc", "C:/uplne/jina/cesta/Treti_soubor.doc")
```

> **Úkol 2.** Uložte všechny textové soubory (nebo soubory jiného určitého typu) z vašeho pracovního adresáře do nově vytvořené podsložky "`textove_soubory`". Použijte cyklus a podmínku `if`.

## Modul `os.path`

Modul `os.path` je podmodul modulu `os`, obsahující funkce pro práci se samotnými adresářovými cestami. Po načtení modulu `os` jsou funkce podmodulu přístupné přes `os.path.nejaka_funkce()`, podmodul lze však také načíst samostatně (pak ovšem ostatní funkce z modulu `os` přístupné nebudou):

```python
import os.path
```

případně:

```python
from os import path
```

Ve druhém případě budou funkce podmodulu dostupné přes `path.nejaka_funkce()`.

Z obsahu podmodulu `os.path` vybíráme pouze některé, pro nás potenciálně užitečné funkce.

Funkce `basename(path)` vrací poslední položku názvu cesty `path`, tj. v případě, že jde o adresu souboru, vrací jméno souboru, v případě, že jde o složku, vrací jméno složky:

```python
>>> os.path.basename("C:/Moje_slozka/Dalsi_soubor.xls")
'Dalsi_soubor.xls'
>>> os.path.basename("C:/Moje_slozka")
'Moje_slozka'
```

Funkce `dirname(path)` vrací nadřazenou složku poslední položky cesty:

```python
>>> os.path.dirname("C:/Moje_slozka/Dalsi_soubor.xls")
'C:/Moje_slozka'
>>> os.path.dirname("C:/Moje_slozka")
'C:/'
```

Často potřebuju spojit adresu nějakého umístění z jednotlivých částí, např. `C:\hlavni_slozka`, `dalsi_slozka` a `nazev_souboru.xls`, které máme uložené samostatně v nějakých proměnných. To můžeme udělat buď ručně:

```python
a = "C:/hlavni_slozka"
b = "dalsi_slozka"
c = "nazev_souboru.xls"
path = a + "/" + b + "/" + c
path
"C:/hlavni_slozka/dalsi_slozka/nazev_souboru.xls"
```

nebo trochu stručnějším a bezpečnějším způsobem pomocí funkce `join`:

```python
path = os.path.join(a, b, c)
path
"C:\\hlavni_slozka\\dalsi_slozka\\nazev_souboru.xls"
```

Chceme-li ověřit, že dané umístění (soubor či složka) existuje, použijeme funkci `exists`:

```python
>>> os.path.exists("C:/Moje_slozka/Dalsi_soubor.xls")
True
>>> os.path.exists("C:/Moje_slozka/Dalsi_soubor.txt")
False
```

Někdy se také hodí zjistit, zda je dané existující umístění adresou složky či souboru:

```python
# Ověří, zda je daná cesta cestou k souboru
>>> os.path.isfile("C:/Moje_slozka/Dalsi_soubor.xls")
True
>>> os.path.isfile("C:/Moje_slozka")
False

# Ověří, zda je daná cesta cestou ke složce
>>> os.path.isdir("C:/Moje_slozka/Dalsi_soubor.xls")
False
>>> os.path.isdir("C:/Moje_slozka")
True
```

> **Úkol 3.** Modifikujte řešení úkolu 2 tak, abyste použili funkci `os.path.join`.

## Rozdělení řetězce znakem

Pro další část této lekce bude výhodné seznámit se s jednou technikou práce s textovými řetězci, která nebyla probrána v lekci o datových typech. Je jí rozdělení řetězce daným znakem či sérií znaků metodou `split`, což je funkce, která se volá přímo na daném řetězci či na proměnné obsahující řetězec:

```python
>>> "anakonda".split("a")
['', 'n', 'kond', '']

>>> a = "okolo potoka"
>>> b = a.split("o")
>>> b
['', 'k', 'l', ' p', 't', 'ka']
```

Vidíme, že funkce `split` vrací seznam jednotlivých částí původního řetězce, rozděleného zadaným znakem. Pokud je tento znak na začátku či na konci řetězce, obsahuje výsledný seznam na začátku resp. na konci prázdný řetězec.

Pokud použijeme funkci `split` bez argumentu, tj. bez zadání řetězce, kterým se má vstupní řetězec rodělit, rozdělí se všemi tzn. prázdnými znaky, tj. mezerou, tabulátorem (znak `\t`) či znakem nového řádku (znak `\n`). V případě, že text obsahuje posloupnost jakýchkoli prázdných znaků za sebou, interpretuje se tato posloupnost, jako by šlo o jeden prázdný znak (tj. řetězec se v tomto místě rozdělí jen jednou):

```python
>>> zprava = "Ahoj lasko\tje to    jiz dlouho\nco jsme\t\t\tse nevideli"
>>> print(zprava)
Ahoj lasko	je to    jiz dlouho
co jsme			se nevideli

>>> zprava.split()
['Ahoj', 'lasko', 'je', 'to', 'jiz', 'dlouho', 'co', 'jsme', 'se', 'nevideli']
```

Tímto způsobem lze snadno nějaký text rozdělit na jednotlivá slova.

## Čtení textových souborů

Pro ilustraci si představme textový soubor obsahující následující text:

```
Davno, davno jiz tomu, co jsem posledne se divala do te mile mirne tvare, co jsem zulibala to blede lice, plne vrasku, nahlizela do modreho oka, v nemz se jevilo tolik dobroty a lasky; davno tomu, co mne posledne zehnaly stare jeji ruce! - Neni vice dobre starenky! Davno jiz odpociva v chladne zemi!
Mne ale neumrela! - Obraz jeji odtisknut v dusi me s veskerou svoji barvitosti, a dokud zdrava zustane, dotud bude zit v ni! - Kdybych stetcem mistrne vladnout znala, oslavila bych te, mila babicko, jinak; ale nastin tento, perem kresleny - nevim, nevim, jak se komu zalibi!
Ty jsi ale vzdy rikala: "Neni na svete clovek ten, aby se zachoval lidem vsem." Dost na tom, kdyz se najde jen nekolik ctenaru, kteri o tobe s takovou oblibou cisti budou, s jakou ja o tobe pisu.
```

Dejme tomu, že je soubor s tímto textem uložen na adrese `C:\Documents and Settings\user\Dokumenty\babicka.txt.`

K otevření souboru pro čtení slouží vestavěná funkce `open`:
```python
>>> soubor = open(r"C:\Documents and Settings\user\Dokumenty\babicka.txt", "r")
```
Hodnota druhého parametru `"r“` je odvozena z anglického „read“ a udává, že bude soubor otevřen pro čtení.

Pokud chceme ze souboru přečíst naráz vše, co obsahuje, použijeme funkci `read()`. Výsledkem je jediný textový řetězec obsahující celý text:
```python
>>> a = soubor.read()
>>> print(a)
Davno, davno jiz tomu, co jsem posledne se divala do te mile mirne tvare, co jsem zulibala to blede lice, plne vrasku, nahlizela do modreho oka, v nemz se jevilo tolik dobroty a lasky; davno tomu, co mne posledne zehnaly stare jeji ruce! - Neni vice dobre starenky! Davno jiz odpociva v chladne zemi!
Mne ale neumrela! - Obraz jeji odtisknut v dusi me s veskerou svoji barvitosti, a dokud zdrava zustane, dotud bude zit v ni! - Kdybych stetcem mistrne vladnout znala, oslavila bych te, mila babicko, jinak; ale nastin tento, perem kresleny - nevim, nevim, jak se komu zalibi!
Ty jsi ale vzdy rikala: "Neni na svete clovek ten, aby se zachoval lidem vsem." Dost na tom, kdyz se najde jen nekolik ctenaru, kteri o tobe s takovou oblibou cisti budou, s jakou ja o tobe pisu.
```
Důležité je po každé ukončené akci se souborem daný soubor uzavřít:
```python
>>> soubor.close()
```
Jinou možností je číst pouze jeden řádek pomocí funkce `readline()` (pozor: v našem textu je celý odstavec jediným řádkem!). Opakované volání této funkce čte postupně jednotlivé řádky:
```python
>>> soubor = open(r"C:\Documents and Settings\user\Dokumenty\babicka.txt","r")
>>> a = soubor.readline()
>>> b = soubor.readline()
>>> print(a)
Davno, davno jiz tomu, co jsem posledne se divala do te mile mirne tvare, co jsem zulibala to blede lice, plne vrasku, nahlizela do modreho oka, v nemz se jevilo tolik dobroty a lasky; davno tomu, co mne posledne zehnaly stare jeji ruce! - Neni vice dobre starenky! Davno jiz odpociva v chladne zemi!
>>> print(b)
Mne ale neumrela! - Obraz jeji odtisknut v dusi me s veskerou svoji barvitosti, a dokud zdrava zustane, dotud bude zit v ni! - Kdybych stetcem mistrne vladnout znala, oslavila bych te, mila babicko, jinak; ale nastin tento, perem kresleny - nevim, nevim, jak se komu zalibi!
>>> soubor.close()
```
Konečně poslední možností je číst celý text po řádcích pomocí funkce `readlines()`. V takovém případě je výsledkem seznam textových řetězců obsahujících jednotlivé řádky (všimněte si znaků `\n` na konci každého řádku):
```python
>>> soubor = open(r"C:\Documents and Settings\user\Dokumenty\babicka.txt","r")
>>> a = soubor.readlines()
>>> print(a)
['Davno, davno jiz tomu, co jsem posledne se divala do te mile mirne tvare, co jsem zulibala to blede lice, plne vrasku, nahlizela do modreho oka, v nemz se jevilo tolik dobroty a lasky; davno tomu, co mne posledne zehnaly stare jeji ruce! - Neni vice dobre starenky! Davno jiz odpociva v chladne zemi!\n', 'Mne ale neumrela! - Obraz jeji odtisknut v dusi me s veskerou svoji barvitosti, a dokud zdrava zustane, dotud bude zit v ni! Kdybych stetcem mistrne vladnout znala, oslavila bych te, mila babicko, jinak; ale nastin tento, perem kresleny - nevim, nevim, jak se komu zalibi!\n', 'Ty jsi ale vzdy rikala: "Neni na svete clovek ten, aby se zachoval lidem vsem." Dost na tom, kdyz se najde jen nekolik ctenaru, kteri o tobe s takovou oblibou cisti budou, s jakou ja o tobe pisu. \n']
>>> soubor.close()
```
> **Úkol 4.** Uložte si ("manuálně", např. pomocí Poznámkového bloku) uvedený text do textového souboru. Následně soubor v Pythonu otevřte a zjistěte počet řádků, počet slov a počet znaků v textu.

V aplikacích často chceme procházet text přečtený z nějakého souboru "řádek po řádku". Např. se může jednat o datovou tabulku ve formátu `.csv` ("comma-separated values", hodnoty oddělené čárkou), což je jednoduchý textový soubor s tabulkovými daty, který lze nicméně přímo otevírat např. v aplikaci MS Excel. Procházení "řádek po řádku" pak znamená procházení jednotlivých řádků tabulky. Lze k němu využít již zmíněné funkce `readlines`, která vrací seznam řádků - ten lze samozřejmě následně procházet cyklem:

```python
f = open("tabulka.csv", "r")
lines = f.readlines()
f.close()
for l in lines:
    # Tady bude nějaká akce s proměnnou l
```

Objekt, který vrací funkce `open`, je nicméně v Pythonu udělán tak, že lze v cyklu procházet přímo jej, chová se tedy sám jako seznam řádků:

```python
f = open("tabulka.csv", "r")
for l in f:
    # Tady bude nějaká akce s proměnnou l
f.close()
```

V tomto případě se ovšem procházení děje pomocí tzv. **kurzoru**, což se mimo jiné projeví tím, že projdeme-li celý soubor až do konce, je kurzor na konci souboru a další procházení již nic nevrací:

```python
>>> f = open("text.txt", "r")
>>> for l in f: print(l)

prvni radek
druhy radek
...
posledni radek
>>> for l in f: print(l)

>>> f.close()
```

Abychom soubor mohli procházet znovu, museli bychom soubor znovu načíst.

Způsob pomocí načtení seznamu řádků metodou `readlines` má navíc tu výhodu, že lze procházet jen vybrané řádky, např. při čtení tabulky vynechat první řádek, jímž bývá hlavička tabulky:

```python
f = open("tabulka.csv", "r")
lines = f.readlines()
f.close()
for l in lines[1:]:
    # Tady bude nějaká akce s proměnnou l
```

Závěrem si ještě ukážeme způsob, jak soubory otevírat, aniž byste museli myslet na jejich následné zavření. Je to možné pomocí klauzule `with`, která vytváří odsazený blok kódu. Jakmile blok kódu skončí tím, že odsadíme na původní úroveň, otevřený soubor se sám automaticky zavře. Syntax je následující:

```python
with open("soubor.txt") as f:
    f.readlines()
    ...
# Tady pokračuje kód a soubor již je uzavřen
```

> **Úkol 5.** Otevřte si tabulku [precipitation_2014.csv](https://owncloud.cesnet.cz/index.php/s/SMwOA7a4evWXRhC) v aplikaci MS Excel a prozkoumejte, co obsahuje. Pak ji zavřete a otevřete ji v programu Poznámkový blok: čím jsou odděleny jednotlivé sloupce? Napište program, který tabulku otevřte a spočítá průměrný srážkový úhrn v srpnu daného roku. (Poznámka: data pocházejí z ČHMÚ.)

## Zápis textových souborů

Chceme-li naopak do textového souboru zapisovat, je nutné jej otevřít s parametrem `"w"` (z anglického „write“):

```python
soubor = open(r"C:\Documents and Settings\user\Dokumenty\dedecek.txt","w")
```

Soubor, který tímto způsobem otvíráme, nemusí ve skutečnosti vůbec existovat a je tímto příkazem vytvořen (musí však existovat příslušná složka). Pokud soubor již existuje, je uvedeným způsobem otevření veškerý jeho obsah vymazán a při následném zapisování nahrazen obsahem novým.

Zápis se provádí pomocí funkce `write(text)`. Pokud chceme zapsat text na více řádků, je třeba použít znak pro konec řádku `\n`. Opět je třeba nezapomenout na konečné uzavření souboru.

```python
>>> soubor.write("Ahoj dedo.\n")
>>> soubor.write("Nevis,\nco je s babickou?")
>>> soubor.close()
```

Přečteme-li výsledný soubor, obdržíme:

```python
>>> soubor = open(r"C:\Documents and Settings\user\Dokumenty\dedecek.txt","r")
>>> print(soubor.read()
Ahoj dedo.
Nevis,
co je s babickou?
>>> soubor.close()
```

Zbývá vyřešit, jak zapisovat do existujícího souboru, aniž bychom jej při otvírání celý přepsali. To lze provést jeho otevřením s parametrem `"a"` (z anglického „append“). Následný zápis funkcí `write(text)` pak způsobí zapsání daného textu na konec za již existující text:

```python
>>> soubor = open(r"C:\Documents and Settings\user\Dokumenty\dedecek.txt","a")
>>> soubor.write("\nJo, hochu, to vazne nevim.")
>>> soubor.close()
```

Výsledek vypadá následovně:

```python
>>> soubor = open(r"C:\Documents and Settings\user\Dokumenty\dedecek.txt","r")
>>> print(soubor.read())
Ahoj dedo.
Nevis,
co je s babickou?
Jo, hochu, to vazne nevim.
>>> soubor.close()
```

> **Úkol 6.** Napiště program, který v tabulce [precipitation_2014.csv](https://owncloud.cesnet.cz/index.php/s/SMwOA7a4evWXRhC) změní desetinné tečky na desetinné čárky.

## Shrnutí

## Úlohy

1. Napište program, který vyrobí obrácenou kopii daného textového souboru (tj. text ve výstupním souboru bude zrcadlově obrácen).
2. Napište program, který spočítá četnost výskytu daného písmene v zadaném vstupním textovém souboru. Řešte nejdříve s použitím metody `count` seznamů, následně bez této metody.
3. Ve složce [data_teplota](https://owncloud.cesnet.cz/index.php/s/C3cV43wNi3ngFiJ) najdete textové soubory, v nichž jsou údaje o měření teploty (v desetinách stupně Celsia) na jednotlivých evropských stanicích (každé stanici odpovídá jeden soubor). Napište program, který z těchto textových souborů vyextrahuje hodnoty teplot pro květen 1998, spočítá pro každou stanici z těchto hodnot průměr a výsledek uloží do nové textové tabulky ve formátu `.csv` (oddělovačem sloupců bude středník). Výstupní tabulka bude obsahovat dva sloupce: "STAID" s identifikátorem stanice a "T" s průměrnou teplotou v květnu 1998.

