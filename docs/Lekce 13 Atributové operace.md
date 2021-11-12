# Lekce 13: Atributové operace

V této lekci probereme operace, které jste většinou zvyklí v grafickém prostředí ArcGIS provádět "ručně", často kliknutím pravým tlačítkem myši na pole atributové tabulky či vrstvu a výběrem nástroje z kontextového menu. Tyto operace zahrnují např.:

- Přidání nového pole do atributové tabulky.
- Smazání existujícího pole atributové tabulky.
- Výpočet hodnot pole atributové tabulky pomocí nástroje *Field calculator*, případně *Calculate geometry*.
- Sumarizace hodnot nějakého pole/polí atributové tabulky.
- Propojení tabulek pomocí *Join* nebo *Relate*.
- Atributový výběr prvků (řádků tabulky) pomocí SQL dotazu.

Ke všem těmto "ručním" operacím ve skutečnosti existují odpovídající geoprocessingové nástroje z ArcToolboxu, které umožňují jejich provádění v rámci modelů z Model Builderu či v rámci skriptů v Pythonu. Většina z nich je umístěná v nástrojovém balíčku *Data Management Tools*.

## Motivační úloha

Jednotlivé operace si ukážeme při řešení následující komplexní úlohy:

> Vypočtěte objem povrchového odtoku pro jednotlivé katastry na Liberecku. Povrchový odtok (v litrech za sekundu) se počítá dle vzorce $Q = S * \Psi * q$, kde $S$ je velikost odvodňované plochy v metrech čtverečních, $\Psi $ je bezrozměrný součinitel odtoku dle Nyplta a Viessmanna (odráží typ, úpravu a drsnost povrchu) a $q$ je roční úhrn srážek v milimetrech.

Nejprve si stáhněte a prohlédněte potřebná [data](https://owncloud.cesnet.cz/index.php/s/R9f1NAoQBbXtMrd). Popis dat...

Nástin řešení...

## Přidání a smazání pole

Nástroje pro práci s poli atributových tabulek, jako je přidání pole, smazání pole, či výpočet hodnot pole, nalezneme v nástrojové sadě *Data Management Tools -> Fields*.

**Přidání pole** se provádí nástrojem *Add Field*. Jeho použití si předvedeme na příkladu přidání textového pole "NEWFIELD" do tabulky vrstvy "obce_LK". Operaci můžeme provést např. v okně *Python* v mapovém dokumentu ArcMap, abychom si mohli okamžitě prohlédnout výsledek. 

```python
arcpy.management.AddField("obce_LK", "NEWFIELD", "TEXT")
```

> **Úkol 1.** Zadejte do příkazového řádku okna *Python* výše uvedený příkaz a prohlédněte si výsledek v atributové tabulce.

Jak je z ukázky patrno, prvním parametrem nástroje je název vstupní tabulky (je možné zadat jak vektorovou třídu prvků - pak se použije její atributová tabulka, nebo samostatnou tabulku ve formátu DBF). Druhým parametrem je název přidávaného pole. Třetím parametrem je datový typ pole (kompletní výčet typů, které je možné použít, naleznete v nápovědě k nástroji). Dalšími nepovinnými parametry bychom mohli nastavit omezení pro daný formát, jako je např. maximální délka textového řetězce (v případě textového pole), počet platných číslic (*scale*) a desetinných míst (*precision*) v případě číselného typu apod. (V ukázce se spokojujeme s výchozím nastavením těchto parametrů.)

Pokus přidat pole stejného názvu ještě jednou... 

Smazání pole

> **Úkol 2.** Napište funkci `AddNewField`, která do zadané tabulky/třídy prvků přidá nové pole zadaného názvu. Funkce však nejprve vyhodnotí, zda pole s tímto názvem již v tabulce není, a pokud ano, toto původní pole smaže a nahradí je polem novým. 

## Propojování tabulek

Propojení tabulek pomocí metody *join* má za následek přidání pole (či polí) z připojované tabulky do cílové tabulky, a to na základě nějakého společného atributu (tzv. *klíče*). Tento typ propojení je možný v případě, kdy jednomu řádku cílové tabulky odpovídá max. jeden řádek připojované tabulky (jinak by totiž nebylo možné rozhodnout, který řádek připojované tabulky se má k danému řádku cílové tabulky připojit). Zároveň platí, že jednomu řádku připojované tabulky může odpovídat jeden či více řádků cílové tabulky. Jeden řádek připojované tabulky se totiž může zkopírovat do libovolného množství řádků cílové tabulky.

Vedle metody *join* je v ArcGIS k dispozici ještě metoda *relate*, která umožňuje definovat i propojení v případě, kdy jednomu řádku cílové tabulky odpovídá více řádků připojované tabulky. Tuto metodu nicméně nelze použít pomocí Pythonu, resp. balíčku ArcPy. Zaměříme se proto pouze na metodu *join*.

Propojení metodou *join* lze vpraxi provést dvěma způsoby:

- jako dočasné propojení, které je definované pouze v rámci vrstev,
- jako permanentní rozšíření cílové tabulky o nové řádky, zkopírované z připojované tabulky.

### Dočasné propojení

Dočasné propojení je právě ten způsob, který jste pravděpodobně zvyklí provádět v prostředí ArcGIS "ručně".

Jelikož je dočasné propojení definováno na úrovni vrstvy, nikoli datové sady, je pro jeho použití pomocí Pythonu třeba nejprve vytvořit ze zdrojové sady vrstvu. I to je v Pythonu možné, vrstva se vutvoří v operační paměti a bude uložena do proměnné, pomocí které s ní bude možné pracovat. Zároveň bude mít svůj uživatelem zvolený název, pod kterým se na ni lze dále v kódu odkazovat.

Vrstva se vytváří nástrojem **Make Feature Layer** z balíčku *Data Management Tools*. Jejím prvním parametrem je cesta k datové sadě, z níž chceme vrstvu vytvořit, druhým parametrem je název vrstvy. Výsledek volání nástroje (objekt třídy *Result*) můžeme uložit do proměnné, která bude následně vrstvu zastupovat v dalších nástrojích.

```python
lyr = arcpy.management.MakeFeatureLayer(r"C:\...\UrbanAtlas_LK.shp", "lyr")
```

Máme-li vytvořenu vrstvu, můžeme v ní definovat propojení s nějakou jinou tabulkou pomocí nástroje **Add Join** z *Data Management Tools -> Joins*. V následující ukázce je k atributové tabulce polygonů krajinného pokryvu z databáze Urban Atlas (viz motivační úloha na začátku lekce) připojena tabulka hodnot součinitelů odtoku pro jednotlivé typy povrchu. Společným sloupcem (klíčem), pomocí kterého se propojení provádí, je atribut "CODE", což je kód třídy krajinného pokryvu. Po připojení je vypsán seznam názvů sloupců vytvořené vrstvy, k níž byla tabulka připojena:

```python
import arcpy

# inputs
land_cover = "UrbanAtlas_LK.shp"
coef_infil = "coef.dbf"

# create a layer from the dataset
lyr = arcpy.management.MakeFeatureLayer(land_cover, "lyr")

# join
arcpy.management.AddJoin(lyr, "CODE", coef_infil, "CODE")

# print fields of the newly created layer
for field in arcpy.ListFields(lyr):
    print(field.name)

```

Výsledek je následující:

```console
UrbanAtlas_LK.FID
UrbanAtlas_LK.Shape
UrbanAtlas_LK.CITIES
UrbanAtlas_LK.LUZ_OR_CIT
UrbanAtlas_LK.CODE
UrbanAtlas_LK.ITEM
UrbanAtlas_LK.PROD_DATE
UrbanAtlas_LK.SHAPE_LEN
UrbanAtlas_LK.SHAPE_AREA
UrbanAtlas_LK.srazky
coef.OID
coef.kod
coef.nazev
coef.cesky
coef.ko_vsak
coef.CODE
```

Jak je vidět, v tabulce vrstvy jsou názvy sloupců doplněny názvem tabulky, z níž sloupce pochází. Díky tomu se v této vrstvě můžeme odkazovat na sloupce z připojené tabulky.

> **Úkol  3.**  Spusťte skript s výše uvedeným kódem. *Pozor*: Skript předpokládá, že pracovní adresář ArcGIS je nastaven na složku, ve které máme data. Pokud tomu tak ve vašem případě není, je třeba přidat do skriptu odpovídající příkaz.

Jak bylo již uvedeno, propojení pomocí *Add Join* je pouze dočasné, neboť se děje ve vrstvě, nikoli ve zdrojové datové sadě. Pokud již propojení tabulek dále nepotřebujeme, je třeba je z vrstvy odstranit nástrojem **Remove Join** (*Data Management Tools -> Joins*). Níže uvádíme jeho použití a následné vypsání polí vrstvy, ze které bylo propojení odstraněno:

```console
>>> arcpy.management.RemoveJoin(lyr)
<Result 'lyr'>
>>> for f in arcpy.ListFields(lyr): print(f.name)

FID
Shape
CITIES
LUZ_OR_CIT
CODE
ITEM
PROD_DATE
SHAPE_LEN
SHAPE_AREA
srazky
```

Je vidět, že návratovou hodnotou nástroje je opět objekt třídy *Result*, reprezentující danou vrstvu. Tuto vrstvu však již máme uloženou v proměnné `lyr`.

Namísto odstranění propojení je možné také vrstvu smazat. Nestačí ovšem použít pythonovský příkaz `del`, je nutné vrstvu smazat nástrojem **Delete** z balíčku ArcPy: 

```console
>>> arcpy.management.Delete(lyr)
<Result 'true'>
```

Smazání vrstvy je důležité proto, že při vytvoření vrstvy z nějaké datové sady se vždy tato datová sada "zamkne", tj. vytvoří se k ní soubor zámku, zamezující jakémukoli programu provádět v datové sadě změny. Pokud bychom např. chtěli v datové sadě přidávat či mazat pole, nebude to možné, pokud je z ní vytvořena vrstva. Po smazání vrstvy se soubor zámku smaže rovněž a datová sada se odemkne pro úpravy.

### Trvalé propojení

Pokud chceme připojit pole trvale, máme dvě možnosti:

- Provedeme nejprve dočasné propojení výše uvedeným způsobem, a pak výslednou vrstvu uložíme do nové datové sady, např. nástrojem **Copy Features** (*Data Management Tools* *->* *General*).
- Připojíme pole přímo do tabulky zdrojové datové sady pomocí nástroje **Join Field** (*Data Management Tools -> Joins*). Zde je možné připojit buď všechna pole, nebo jen vybraná. Připojená pole se stanou trvalou součástí zdrojové tabulky.

> **Úkol 4.** Pomocí nástroje *Join Field* připojte z tabulky součinitelů odtoku pole s hodnotami součinitele ("ko_vsak") jako trvalé nové pole do tabulky krajinného pokryvu. (Syntax nástroje prostudujte v nápovědě.) Prohlédněte si výslednou tabulku. Následně nové pole opět smažte nástrojem *Delete Field*.

## Výpočet hodnot pole

Výpočet hodnot pole tabulky se provádí nástrojem **Calculate Field** z nástrojové sady *Fields* balíčku *Data Management Tools*. Prvním parametrem je cesta ke vstupní datové sadě, druhým název pole, které chceme vypočítat. Následuje výraz výpočtu.

Následující ukázka prezentuje nejjednodušší možný výpočet, kdy je celé pole vyplněno konstantní hodnotou. Nejprve do datové sady "obce_LK.shp" přidáme celočíselné pole s názvem "CONST" a vyplníme ho konstantní hodnotou 100. Dále podobným způsobem přidáme textové pole s názvem "MESSAGE" a vyplníme ho textem "Hello world!":

```python
import arcpy

obce = "obce_LK.shp"

# Add a numeric (integer) field
arcpy.management.AddField(obce, "CONST", "LONG")

# Fill the field with a constant of 100
arcpy.management.CalculateField(obce, "CONST", 100)

# Add a text field
arcpy.management.AddField(obce, "MESSAGE", "TEXT")

# Fill the field with a constant of 100
arcpy.management.CalculateField(obce, "MESSAGE", '"Hello world!"')

```

Všimněte si, že v případě textového atributu bylo třeba výraz "Hello world!" napsat pomocí obou typů uvozovek: `'"Hello world!"'`. To proto, že součástí výrazu musí v případě textu být i uvozovky, přičemž samotný výraz je nástroji předáván také jako text. Vnější, jednoduché uvozovky ohraničují celý text výrazu, kdežto vnitřní, dvojité uvozovky jsou již součástí výrazu samotného a sdělují, že obsah výrazu je text. (Tyto vnější uvozovky odpovídají uvozovkám, které bychom byli nuceni použít v okně *Field Calculator* při "ručním" řešení výpočtu pole.)

> **Úkol 5.** Pomocí uvedeného skriptu přidejte do datové sady obcí uvedená pole a vyplňte je nějakou konstantní hodnotou. Prohlédněte si výsledek v ArcGIS. Následně pole zase smažte (pomocí Pythonu).

Při vyplňování pole je možné použít hodnoty kteréhokoli jiného, již existujícího pole. Na toto pole se lze odkázat jeho názvem, uvedeným mezi vykřičníky (viz dále). Je přitom možné hodnoty různých polí libovolně kombinovat, provádět s nimi výpočty apod. Ve výrazu je možné použít jakoukoli funkci či operaci, která je součástí základní výbavy Pythonu. Jen je třeba čtvrtým, volitelným parametrem nástroje *Calculate Field* specifikovat, jaký jazyk ve výrazu používat (resp. že používáme Python). Volba jazyka má vliv i na to, jakým způsobem se odkazujeme na jiná pole (uvedené vykřičníky se používají pouze v případě jazyka Python). V ukázce níže používáme specifikaci jazyka "PYTHON_9.3", což je jazyk výrazů založený na Python, v podobě, v jaké byl používán od verze ArcGIS 9.3 dále.

Ukážeme si výpočet pole pomocí hodnot jiného pole na příkladu, kdy do pole "NAZEV_2", které nejprve přidáme do datové sady "obce_LK.shp", zkopírujeme text anglického názvu obce, ovšem převedený na velká písmena. V Pythonu je možné libovolný textový řetězec převést na velká resp. malá písmena metodou řetězce `upper` resp. `lower`:

```console
>>> "HelLo".upper()
'HELLO'
>>> "HelLo".lower()
'hello'
```

Nyní zmíněná ukázka vyplnění pole:

```python
import arcpy

obce = "obce_LK.shp"

# add a new field
arcpy.management.AddField(obce, "NAZEV_2", "TEXT")

# fill the field with uppercase names from the field NAZEV_ENG
arcpy.management.CalculateField(obce, "NAZEV_2", '!NAZEV_ENG!.upper()', "PYTHON_9.3")
```

Všimněte si použití vykřičníků pro odkaz na pole "NAZEV_ENG" v rámci výrazu, a na použití metody `upper`.

> **Úkol 6.** Obdobným způsobem, jako v předchozí ukázce, vytvořte pole s názvy obcí psanými malými písmeny.

Velmi častou úlohou je výpočet geometrických vlastností prvků do atributu. Příkladem může být výpočet ploch polygonů do atributu nazvaného např. "AREA_KM". "Ruční" řešení by spočívalo v použití nástroje *Calculate Geometry* dostupného v kontextovém menu příslušného pole. V Pythonu se operace provede opět pomocí nástroje *Calculate Field*, kde se v rámci výrazu odkážeme na speciální pole "Shape" a jeho vlastnost "area" pomocí `!shape.area!` (chceme-li výsledek ve čtverečních kilometrech, je nutné do výrazu zakomponovat i převod jednotek):

 ```python
import arcpy

obce = "obce_LK.shp"

# add a new field for areas
arcpy.management.AddField(obce, "AREA_KM", "DOUBLE")

# fill the field with areas in squared kilometers
arcpy.management.CalculateField(obce, "AREA_KM", '!shape.area!/1000000', "PYTHON_9.3")
 ```

> **Úkol 7.** Do datové sady "UrbanAtlas_LK.shp" přidejte pole "PARATIO" a vypočtěte do něj poměr obvodu a plochy (tzv. "Perimeter-Area Ratio"), což je krajinná metrika popisující míru složitosti tvaru krajinných plošek.

## Sumarizace pole

Dokončení odtoků...

## Atributové výběry

Kde všude se může vyskytnout SQL dotaz..

Select

Jak vytvořit vrstvu v Pythonu

Opakovaný výběr ve vrstvě.. (např. vyplňování pole dle atributu?)

Uložení vrstvy?

## Shrnutí

## Úlohy



