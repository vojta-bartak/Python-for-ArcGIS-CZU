# Lekce 15: Procházení tabulek pomocí kurzorů

V této lekci si ukážeme, jak lze procházet atributové tabulky (či jiné tabulky ve formátu `.dbf`) řádek po řádku a část a zapisovat hodnoty z jednotlivých polí (sloupců). Jednotlivé procedury si budeme zkoušet na [datech](https://owncloud.cesnet.cz/index.php/s/87mECJH9RhNIdVz) krajů (polygony) a velkých měst (body) ČR. 

> **Úkol 1.** Obě vrstvy si nahrajte do mapového dokumentu v ArcMap a prohlédněte si je, včetně atributových tabulek.

Procházení tabulky se děje pomocí tzv. *kurzoru*, což je objekt podobný tomu, jaký vytváříme při otevírání textových souborů funkcí `open`: zprostředkovává spojení s daným souborem (tabulkou), takže po celou dobu, kdy tabulku procházíme, je uzamčena pro úpravy jinými aplikacemi. Z toho důvodu je po ukončení práce s tabulkou nutné vytvořený kurzor smazat (příkazem `del`), podobně jako po ukončení práce s textovým souborem jej uzavřeme metodou `close`.

Kurzory v `arcpy` jsou trojího druhu:

- **Search Cursor** - určený pro pouhé čtení hodnot z tabulky,
- **Update Cursor** - určený pro čtení a zápis do existujících řádků tabulky,
- **Insert Cursor** - určený pro vkládání nových řádků do tabulky a mazání existujících řádků.

## Čtení řádků pomocí *Search* kurzoru

Syntaxe použití kurzoru je následující:

```python
tab = arcpy.da.SearchCursor(in_table, field_names, {where_clause}, ...)
```

V uvedeném kódu ukládáme kurzor do proměnné `tab`. Část `arcpy.da.` odkazuje na skutečnost, že kurzory jsou v balíčku `arcpy` implementovány v samostatném modulu `da` (zkratka pro "Data Access"). Kurzor se vytváří stejnojmenným konstruktorem dané třídy (funkce `SearchCursor` v ukázce). Nejdůležitějšími parametry funkce jsou: 

- **in_table** - tabulka, která se má otevřít (celá cesta jako řetězec, případně relativní cesta vzhledem k pracovnímu adresáři) - může se jednat o shapefile, `.dbf` tabulku, diskrétní rastr (který má tabulku), vrstvu (pokud jsme ji vytvořili či pokud pracujeme v Python Window v ArcMap) apod.,
- **field_names** - seznam polí (sloupců), která chceme otevřít (pythonovský seznam řetězců),
- **where_clause** - SQL dotaz (nepovinně), kterým můžeme omezit otevíranou tabulku pouze na vybrané řádky.

Kurzor obsahuje ještě další parametry (znázorněné v ukázce třemi tečkami), ty však zde ponecháme stranou.

Kurzor je možné přímo procházet `for` cyklem. Iterační proměnná přitom prochází jednotlivé řádky a nabývá vždy hodnoty *n*-tice stejné délky, jako je délka řádku (tj. počet otevřených polí). Ukažme si to na příkladu otevření atributů "KATEGORIE" a "NAZEV" vrstvy sídel:

```python
import arcpy
arcpy.env.workspace = r"C:\my_path\..."
tab = arcpy.da.SearchCursor("Sidla_body.shp", ["NAZEV", "KATEGORIE"])
for r in tab:
    print(r)
del(tab)
```

Po spuštění skriptu obdržíme na konzoli následující výpis:

```
(u'Praha', 1)
(u'Brno', 2)
(u'Plze\u0148', 2)
(u'Ostrava', 2)
(u'Karlovy Vary', 3)
(u'Zl\xedn', 3)
(u'Chomutov', 3)
(u'Most', 3)
(u'\xdast\xed nad Labem', 3)
(u'Kladno', 3)
(u'Hradec Kr\xe1lov\xe9', 3)
(u'Karvin\xe1', 3)
(u'Pardubice', 3)
(u'Fr\xfddek-M\xedstek', 3)
(u'Hav\xed\u0159ov', 3)
(u'Liberec', 3)
(u'\u010cesk\xe9 Bud\u011bjovice', 3)
(u'Olomouc', 3)
```

Jelikož je řádek jednoduchou *n*-ticí, jemožné k jednotlivým položkám (sloupcům) řádku přistupovat pomocí indexů:

```python
# -*- coding: cp1250 -*-

import arcpy
arcpy.env.workspace = r"C:\my_path\..."
tab = arcpy.da.SearchCursor("Sidla_body.shp", ["NAZEV", "KATEGORIE"])
for r in tab:
    print(u"Město kategorie " + str(r[1]) + " se jmenuje " + r[0] + ".")
del(tab)
```

Výpis vypadá následovně:

```python
Město kategorie 1 se jmenuje Praha.
Město kategorie 2 se jmenuje Brno.
Město kategorie 2 se jmenuje Plzeň.
Město kategorie 2 se jmenuje Ostrava.
Město kategorie 3 se jmenuje Karlovy Vary.
Město kategorie 3 se jmenuje Zlín.
Město kategorie 3 se jmenuje Chomutov.
Město kategorie 3 se jmenuje Most.
Město kategorie 3 se jmenuje Ústí nad Labem.
Město kategorie 3 se jmenuje Kladno.
Město kategorie 3 se jmenuje Hradec Králové.
Město kategorie 3 se jmenuje Karviná.
Město kategorie 3 se jmenuje Pardubice.
Město kategorie 3 se jmenuje Frýdek-Místek.
Město kategorie 3 se jmenuje Havířov.
Město kategorie 3 se jmenuje Liberec.
Město kategorie 3 se jmenuje České Budějovice.
Město kategorie 3 se jmenuje Olomouc.
```

Všimněme si, že jsme při otevírání tabulky změnili pořadí sloupců oproti tomu, jak se tabulka zobrazuje v ArcMap. Při otevírání tabulky pomocí kurzoru můžeme zvolit libovolné sloupce v libovolném pořadí, dokonce můžeme jeden sloupec otevřít v několika kopiích (což má zřídka nějaký dobrý důvod). Při čtení hodnot z řádku je přitom pořadí položek přesně takové, v jakém pořadí jsme sloupce seřadili při vytváření kurzoru.

V předešlé ukázce jsme nevyužili volitelný parametr *where_clause*, kterým můžeme omezit otvírané řádky. Pokud bychom např. chtěli otevřít pouze záznamy o městech první a druhé kategorie, vypadal by kód takto:

```python
import arcpy
arcpy.env.workspace = r"C:\my_path\..."
tab = arcpy.da.SearchCursor("Sidla_body.shp", ["NAZEV"], '"KATEGORIE" < 3')
for r in tab:
    print(r[0])
del(tab)
```

Výsledek:

```
Praha
Brno
Plzeň
Ostrava
```

Procházení pomocí `for` cyklu není jediný způsob, jak kurzorem procházet tabulku. Chceme-li se v tabulce posunout o jeden řádek, můžeme použít metodu kurzoru `next`, která vrací aktuální řádek a zároveň posouvá polohu kurzoru na následující řádek. Tímto způsobem je možné např. číst obsah osmnáctého řádku:

```python
import arcpy
arcpy.env.workspace = r"C:\my_path\CR_kraje_sidla_toky"
tab = arcpy.da.SearchCursor("Sidla_body.shp", ["NAZEV", "KATEGORIE"])
for i in range(18):
    r = tab.next()
print(r)
del(tab)
```

```
(u'Olomouc', 3)
```

Při procházení tabulky je možné kurzor posouvat pouze dopředu. Pokud se dostaneme na konec tabulky (ať již `for` cyklem nebo metodou `next`), tabulku již znovu procházet nelze. K tomu účelu je nutné kurzor vytvořit znovu.

Na závěr pojednání o *Search* kurzoru si ukážeme, jak se lze vyhnout nutnosti kurzor po použití mazat, a to díky klauzuli `with`, kterou znáte (v podobném kontextu) z lekce o textových souborech:

```python
import arcpy
arcpy.env.workspace = r"C:\my_path\..."
with arcpy.da.SearchCursor("Sidla_body.shp", ["NAZEV"], '"KATEGORIE" < 3') as tab:
    for r in tab:
        print(r[0])
# Zde následují další příkazy, přičemž kurzor již je zavřený
```

> **Úkol 2.** Otevřete tabulku krajů a vypište jejich názvy a počty obyvatel.

> **Úkol 3.** Pomocí kurzoru najděte kraj s největší početní převahou žen nad muži.

> **Úkol 4.** Pomocí kurzoru vypočítejte index tvaru (obvod dělený plochou) jednotlivých polygonů krajů. (Nápověda: nejprve do tabulky přidejte pole "Plocha" a "Obvod" a nástrojem *Calculate Field* vypočítejte jejich hodnoty. Následně kurzorem projděte tabulku a tyto hodnoty podělte.)

## Přepsání hodnot v řádku pomocí *Update* kurzoru

Pokud chceme obsah tabulky nejen číst, ale i měnit, použijeme *Update* kurzor. Syntaxe je prakticky stejná jako u *Search* kurzoru:

```python
tab = arcpy.da.UpdateCursor("Sidla_body.shp", ["NAZEV", "KATEGORIE"])
```

Rozdíl je v tom, jaké metody má příslušný kurzor v nabídce (v předchozí části jsme používali např. metodu `next`, která je dostupná i v *Update* kurzoru). U *Update* kurzoru jde především o metodu *updateRow*, pomocí které lze daný řádek, na kterém zrovna kurzor je, nahradit jiným řádkem. Ten je třeba metodě předat v podobě *n*-tice nebo seznamu odpovídající délky, přičemž jednotlivé položky musejí svým datovým typem odpovídat otevřeným sloupcům.

V následující ukázce změníme kategorii města Brno z 2 na 3 (tím o tomto městu nenaznačujeme nic špatného!):

```python
import arcpy
arcpy.env.workspace = r"C:\my_path\CR_kraje_sidla_toky"
tab = arcpy.da.UpdateCursor("Sidla_body.shp", ["NAZEV", "KATEGORIE"], 
                            '"NAZEV" = \'Brno\'')
tab.next()
tab.updateRow(("Brno", 3))
del(tab)
```

Všimněte si, že jsme otevřeli rovnou pouze řádek s Brnem. Přesto jsme museli použít metodu `next`, abychom se na tento první řádek kurzorem posunuli.

> **Úkol 5.** Do tabulky krajů přidejte textové pole "STAV" a vyplňte do něj hodnotu "OK" pro ty kraje, kde je zemřelých méně či stejně jako narozených, a "KO" pro ostatní.

## Smazání řádku

Dalším použitím *Update* kurzoru je smazání řádku. Toho nyní využijeme a pokusíme se vymazat město Brno z mapy světa nadobro. Jelikož však následně budeme chtít svůj strašný čin odčinit a pomocí *Insert* kurzoru Brno opět na mapu světa vrátit, bude vhodné si příslušný záznam někam dopředu uložit, např. takto:

```python
with arcpy.da.SearchCursor("Sidla_body.shp", ["SHAPE@", "NAZEV", "KATEGORIE"],
                          '"NAZEV" = \'Brno\'') as tab:
    brno = tab.next()
```

Důležité je zde uchovat i obsah sloupce *Shape*, což se děje pomocí názvu `"SHAPE@"` (o tomto poli a jeho použití se důkladně zmíníme v další lekci). V tomto poli je totiž uložena geometrie prvku (v našem případě souřadnice města Brna), které budeme při jeho obnovování potřebovat.

Nyní již samotná poprava:

```python
with arcpy.da.UpdateCursor("Sidla_body.shp", ["SHAPE@", "NAZEV", "KATEGORIE"],
                          '"NAZEV" = \'Brno\'') as tab:
    tab.next()
    tab.deleteRow()
```

O výsledku se v tabulce měst přesvědčte sami...

## Přidání nového řádku pomocí *Insert* kurzoru

Nyní, jak jsme avizovali, jsme si uvědomili dosah našeho brutálního činu. Naštěstí je zde *Insert* kurzor, umožňující do tabulky vložit nový řádek. Má jednodušší syntax, neboť neumožňuje filtrování otvíraných řádků pomocí SQL dotazu. Jediné, co můžeme ovlivnit, je, které sloupce otevřeme. Nový řádek se přitom vždy přidává na konec tabulky (i v případě tak významného města, jakým je Brno).

Pokud pracujeme s atributovou tabulkou vektorové vrstvy, je nutné mít na paměti, že kdykoli přidáváme nový záznam, je třeba v daném řádku mít i geometrii přidávaného prvku, kterou je třeba vložit do pole *Shape*. Jinak by danému řádku neodpovídal žádný geometrický prvek a celá vrstva by byla poškozena nepoužitelná. My jsme si naštěstí v případě Brna geometrii uložili, takže není problém ji do tabulky opět vrátit:

```python
with arcpy.da.InsertCursor("Sidla_body.shp", ["SHAPE@", "NAZEV", "KATEGORIE"]) as tab:
    tab.insertRow(brno)
```

Zbývá než doufat, že jste celou proceduru smazání a opětovné záchrany Brna dělali v rámci jedné session, tj. že jste mezitím nerestartovali konzoli. Proměnná `brno` by tak byla smazána a město nadobro ztraceno... 

## Staré typy kurzorů

## Shrnutí

## Úlohy

1. Napište skript, který pro polygonovou vrstvu krajinného pokryvu vypočítá průměrný index tvaru pro jednotlivé třídy krajinného pokryvu. Řešte bez kurzorů. 
2. Řešte předchozí úlohu s pomocí kurzorů, výsledek uložte do tabulky ve formátu `.csv`.