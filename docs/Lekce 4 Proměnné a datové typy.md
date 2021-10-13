# Lekce 4: Proměnné a datové typy

## Proměnné

V Pythonu je proměnná odkazem na nějaký objekt v operační paměti. Tento objekt může být různého typu (neboli třídy): může jít o číslo, o textový řetězec, nebo o cokoli jiného (viz dále). Proměnná má nějaký název (zvolený námi) a nějakou hodnotu. Hodnotu do proměnné přiřazujeme operátorem `=`:

```python
>>> a = 5
```

Datový typ proměnné lze zjistit funkcí *type*:

```python
>>> type(a)
<type 'int'>
>>> b = "Ahoj světe"
>>> type(b)
<type 'str'>
```

> Pojem *funkce* si důkladně rozebereme později. Zatím stačí chápat, že při volání nějaké funkce vždy napíšeme její název a do závorky její *argumenty*, tj. nějaké hodnoty, se kterými daná funkce něco dělá. Funkce pak většinou něco tzv. *vrací*, tj. jejím výsledkem je nějaká hodnota (objekt). Kdykoli tedy vidíte za nějakým názvem jednoduché závorky, máte co do činění s funkcí.

Do proměnné lze "uložit" hodnotu nějaké jiné proměnné:

```python
>>> a = 5
>>> b = a
>>> b
5
```

Je důležité však mít na paměti, že změníme-li hodnotu původní proměnné, hodnota nové proměnné zůstává nezměněna (tato proměnná totiž odkazuje stále na původní objekt - v našem případě číslo 5, kdežto původní proměnná už odkazuje na jiný objekt - např. číslo 100):

```python
>>> a = 100
>>> b
5
```

Proměnné mohou mít téměř libovolné názvy, je však nutné dodržet následující pravidla:

- Název začíná písmenem anglické abecedy nebo podtržítkem
- Následuje libovolná posloupnost číslic, písmen angl. abecedy a podtržítek
- Nesmí jít o klíčové slovo jazyka (např. `and` nebo `if`)
- Malá a velká písmena se **rozlišují**

Python umožňuje i následující podivnost (vytvoření dvou či více proměnných najednou):

```python
>>> a, b = 3, 5
>>> a
3
>>> b
5
```

> **Úkol 1.** Zkuste si vytvořit několik proměnných různých názvů a ověřit jejich výše uvedené chování.

Vytvořenou proměnnou lze také smazat, a to pomocí příkazu `del`:
```python
>>> x = 5
>>> x
5
>>> del x
>>> x
Traceback (most recent call last):
  File "<pyshell#5>", line 1, in <module>
    x
NameError: name 'x' is not defined
```

## Čísla

V Pythonu jsou dva základní datové typy pro čísla. Jsou to celá čísla (anglicky *integer*, zkráceně *int*):

```python
>>> 56
56
>>> -32586
-32586
```

a čísla s desetinnou čárkou (anglicky *floating point number*, zkráceně *float*):

```python
>>> 5.32
5.32
>>> -6e-7
-6e-7
>>> -5.0E-4
-0.0005
```

S čísly lze provádět běžné matematické operace pomocí aritmetických operátorů:

```python
>>> 3 + 2 # Sčítání
5
>>> 3 – 2 # Odčítání
1
>>> 3 * 2 # Násobení
6
>>> 3 / 2 # Celočíselní dělení celých čísel
1
>>> 3.0 / 2 # Dělení
1.5
>>> 3.1 // 2 # Celočíselné dělení
1.0
>>> 3**2 # Mocnina
9
>>> 3 % 2 # Modulo (zbytek po celočíselném dělení)
1
```

Číselný typ výsledku je vždy určen číselným typem vstupních čísel: jsou-li vstupem pouze celá čísla, je výsledkem celé číslo (při dělení se tedy provede celočíselné dělení); je-li alespoň jeden ze vstupů číslo s desetinnou čárkou, je výsledkem rovněž číslo s desetinnou čárkou.

Python disponuje řadou vestavěných matematických funkcí (v ukázkou jsou jen některé):

```python
>>> abs(-5) # Absolutní hodnota
5
>>> max(5,3,-5,1) # Maximum
5
>>> min(5,3,-5,1) # Minimum
-5
>>> pow(2,3) # Mocnina (ekvivalent 2**3)
8
>>> round(5.6) # Zaokrouhlení
6.0
>>> round(5.627,2) # Zaokrouhlení na dvě desetinná místa
5.63
```

Další matematické funkce lze nalézt v modulu `math`:

```python
>>> import math # Načtení modulu math
>>> math.pi # Aproximace čísla pí
3.141592653589793
>>> math.sqrt(2) # Odmocnina
1.4142135623730951
>>> math.log(math.exp(1)) # Logaritmus a exponenciela
1.0
```

Mezi jednotlivými číselnými typy lze konvertovat:

```python
>>> a = 5.6
>>> type(a)
<type 'float'>
>>> b = int(a) # Konverze na celé číslo
>>> b
5
>>> type(b)
<type 'int'>
>>> c = float(b) # Konverze na číslo s desetinnou čárkou
>>> c
5.0
>>> type(c)
<type 'float'>
```

Podobně lze konvertovat i mezi jinými datovými typy:

```python
>>> int("56") # Převod textu na celé číslo
56
>>> float("56") # Převod textu na číslo s desetinnou čárkou
56.0
>>> str(56) # Převod čísla na text
'56'
```

> **Úkol 2.** Vyzkoušejte si všechny výše uvedené operace a příkazy.

## Textové řetězce

Textové řetězce (či krátce jen řetězce, angl. *string*) se zapisují buď v uvozovkách, nebo v apostrofech:

```python
>>> a = "Toto je textovy retezec, ktery muze obsahovat znak ',"
>>> b = 'a toto je retezec, ktery muze obsahovat znak ".'
>>> print(a); print(b)
Toto je textovy retezec, ktery muze obsahovat znak ',
a toto je retezec, ktery muze obsahovat znak ".
```

Třetím způsobem, jak vytvořit řetězec, je použití trojitých uvozovek. Do takového řetězce je pak možné vkládat i více řádků (aniž bychom museli použít speciální znak pro konec řádku):

```python
>>> a = """Toto je textovy retezec,
ktery obsahuje
tri radky"""
>>> print a
Toto je textovy retezec,
ktery obsahuje
tri radky
```

Řetězce lze do jisté míry chápat jako posloupnosti jednotlivých znaků. V Pythonu jsou řetězce indexované, to znamená, že lze např. číst přímo konkrétní (třeba pátý) znak:

```python
>>> a = "Z tohoto retezce budu cist paty znak:"
>>> print a[4]
h
```

Indexování v Pythonu začíná nulou, nikoli jedničkou, proto bylo pro vypsání pátého znaku nutné do hranatých závorek psát index 4.

Vedle přístupu k jednotlivým znakům lze také pomocí hranatých závorek a indexů vytvářet tzv. *řezy*, neboli číst pouze určitou část řetězce:

```python
>>> a = "Z tohoto retezce budu delat rezy:"
>>> print a[2:10]
tohoto r
>>> print a[:20]
Z tohoto retezce bud
>>> print a[7:]
o retezce budu delat rezy:
```

Tvorba řezů se řídí následujícími pravidly:

- Řez začíná znakem na pozici prvního indexu a končí na pozici před posledním indexem (poslední prvek tedy již není součástí řezu). Řez `a[2:10]` tak obsahuje třetí až desátý znak.
- Řez s prázdným prvním indexem obsahuje celý původní řetězec od začátku až do znaku na pozici před posledním indexem.
- Řez s prázdným druhým indexem obsahuje celý původní řetězec počínaje znakem na pozici prvního indexu.

Řetězce (na rozdíl od seznamů) nelze pomocí indexování a řezů přepisovat. Např. pokud bychom chtěli v textu „kost“ změnit první písmeno na „m“, nelze to provést přímo:

```python
>>> a = "kost"
>>> a[0] = "m"
Traceback (most recent call last):
  File "<pyshell#57>", line 1, in <module>
    a[0] = "m"
TypeError: 'str' object does not support item assignment
```

Řetězce lze sčítat a násobit přirozeným číslem pomocí operátorů `+` a `*`:

```python
>>> a = "Ahoj"
>>> b = "Svete"
>>> print a + b
AhojSvete
>>> print a + " " + b + "!"
Ahoj Svete!
>>> print a*5
AhojAhojAhojAhojAhoj
```

Změnit výše zmíněnou „kost“ na „most“ lze tedy např. takto:

```python
>>> a = "kost"
>>> a = "m" + a[1:]
>>> print a
most
```

Řetězec může obsahovat tzv. *escape sekvence*, tj. posloupnosti znaků, nesoucí speciální význam. Escape sekvence je vždy uvozena zpětným lomítkem `\`:

```python
>>> print '\'' # Znak apostrofu v řetězci uvozeném apostrofy
'
>>> print "\"" # Znak uvozovek v řetězci uvozeném uvozovkami
"
>>> print "Zde je\ttabulator" # Znak tabulátoru
Zde je     tabulator
>>> print "Zde je\nnovy radek" # Znak konce řádku
Zde je
novy radek
```

Jelikož escape sekvence jsou uvozeny zpětným lomítkem, zpětné lomítko samotné nelze v textu použít:

```python
>>> print(V tomto textu se snazim pouzit znak \")
SyntaxError: EOL while scanning string literal
```

To lze vyřešit použitím dvojitého zpětného lomítka, což je další z escape sekvencí:

```python
>>> print("V tomto textu znak \\ klidne pouziji")
V tomto textu znak \ klidne pouziji
```

Toto je důležité zejména při psaní adresářových a souborových adres, kde se zpětná lomítka používají:

```python
>>> print "C:\\Users\\Muj_pocitac\\Documents"
C:\Users\Muj_pocitac\Documents
```

Jiným způsobem psaní adres je použití dopředného lomítka:

```python
>>> print "C:/Users/Muj_pocitac/Documents"
C:/Users/Muj_pocitac/Documents
```

Ještě jiný způsob spočívá v uvození řetězce znakem `r` (před uvozovkami). Tím dáváme najevo, že se jedná o tzv. *surový řetězec* (angl. *raw string*), ve kterém se mají všechny znaky číst svým obvyklým způsobem (zpětné lomítko je proto pouze zpětným lomítkem a žádné escape sekvence zde být nemohou):

```python
>>> print r"C:\Users\Muj_pocitac\Documents"
C:\Users\Muj_pocitac\Documents
```

## Seznamy

Seznam je posloupnost libovolných objektů. Definuje se pomocí hranatých závorek a jeho položky se oddělují čárkami:

```python
>>> seznam1 = [2,-4,56,105,0]
>>> seznam2 = ["ahoj","STROM","","guru"]
```

Do jednoho seznamu lze ukládat hodnoty různých datových typů (to v jiných programovacích jazycích nebývá zvykem):

```python
>>> seznam3 = [0, "ahoj",-5.78,2,4]
```

Podobně jako tomu bylo v případě textových řetězců, k jednotlivým položkám seznamu lze přistupovat pomocí indexů:

```python
>>> seznam3[3]
2
```

Indexy začínají nulou (tj. první položka seznamu má index 0, nikoli 1!) a lze používat i záporné indexy. Záporné indexy indexují seznam od konce, tj. poslední prvek seznamu má index -1, předposlední -2 atd. Záporné indexování je zvláště výhodné v tom, že lze rychle přistupovat např. k poslední položce seznamu, aniž bychom znali či se odkazovali na délku seznamu:

```python
>>> seznam3[-1]
4
```

Na rozdíl od textových řetězců, u seznamů lze pomocí indexů měnit jednotlivé položky:

```python
>>> seznam1[2] = "nova hodnota"
>>> seznam1
[2, -4, 'nova hodnota', 105, 0]
```

Řez seznamu je seznam sestávající pouze z některých prvků původního seznamu. Vytváří se pomocí hranatých závorek, dvou indexů (začátku a konce řezu) a dvojtečky mezi nimi:

```python
>>> seznam2[1:4]
['STROM', '', 'guru']
```

Jak vyplývá z ukázky, prvek na pozici druhého indexu již do řezu nepatří. V řezu lze používat i záporné indexy, takže např. řez

```python
>>> seznam2[0:-1]
['ahoj', 'STROM', '']
```

obsahuje všechny prvky od prvního do předposledního.

Pokud necháme některý z indexů prázdný, je příslušný řez proveden od začátku resp. do konce seznamu:

```python
# toto je rez od zacatku seznamu do tretiho prvku:
>>> seznam2[:3]
['ahoj', 'STROM', '']

# toto je rez od druheho prvku do konce seznamu:
>>> seznam2[1:]
['STROM', '', 'guru']

# toto je tzv. uplny rez:
>>> seznam2[:]
['ahoj', 'STROM', '', 'guru']
```

Další možností, jak vytvářet řezy, je použití třetího „indexu“, lépe řečeno hodnoty, která určuje délku kroku při výběru prvků do řezu. Tímto způsobem lze např. vytvořit řez obsahující každé druhé (či třetí) číslo z určitého rozmezí:

```python
>>> a = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18]
>>> a[2:10:2]
[3, 5, 7, 9]
>>> a[1:13:2]
[2, 4, 6, 8, 10, 12]
>>> a[::3]
[1, 4, 7, 10, 13, 16]
```

Podobně jako u textových řetězců, seznamy lze sčítat a násobit přirozeným číslem (včetně nuly):

```python
>>> seznam1 + seznam2
[2, -4, 'nova hodnota', 105, 0, 'ahoj', 'STROM', '', 'guru']
>>> seznam1 * 2
[2, -4, 'nova hodnota', 105, 0, 2, -4, 'nova hodnota', 105, 0]
>>> 0 * seznam2
[]
```

Výsledkem násobení seznamu nulou (a stejně tak i libovolným záporným celým číslem) je prázdný seznam.

Stejně jako lze pomocí indexů přepisovat jednotlivé položky seznamu, lze také přepisovat celé řezy, a to opět libovolným (tj. libovolně dlouhým) seznamem:

```python
>>> a = [1,2,3,4,5]
>>> a[:3] = ["jedna","dva","tri","tri a pul"]
>>> a
['jedna', 'dva', 'tri', 'tri a pul', 4, 5]
```

Tímto způsobem lze také přidávat libovolné položky na konec seznamu (všimněte si, že v takovém případě lze použít index, který již je mimo rozsah původního seznamu):

```python
>>> a[6:] = [6]
>>> a
['jedna', 'dva', 'tri', 'tri a pul', 4, 5, 6]
```

nebo na začátek seznamu:

```python
>>> a[:0] = [-1,0]
>>> a
[-1.0, 'jedna', 'dva', 'tri', 'tri a pul', 4, 5, 6]
```

případně kamkoli doprostřed seznamu:

```python
>>> a[3:3] = [1.5]
>>> a
[-1, 0, 'jedna', 1.5, 'dva', 'tri', 'tri a pul', 4, 5, 6]
```

Stejně lze i vymazat část seznamu vložením prázdného seznamu:

```python
>>> a[4:7] = []
>>> a
[-1, 0, 'jedna', 1.5, 4, 5, 6]
```

V kapitole "Proměnné" jsme si vysvětlili, že proměnná je odkazem na objekt v operační paměti. Proměnná `a = [1,2,3]` je tedy odkazem na příslušný objekt-seznam. To ale znamená, že pokud vytvořím proměnnou `b = a`, bude tato nová proměnná odkazovat na stejný objekt. Pokud nyní nyní tento objekt prostřednictvím jedné z proměnných změním, změna se pochopitelně dotkne i druhé proměnné:

```python
>>> a = [1,2,3]
>>> b = a
>>> a[0] = -50
>>> a
[-50, 2, 3]
>>> b
[-50, 2, 3]
```

Pokud chceme vytvořit nezávislou kopii daného seznamu, můžeme použít úplný řez:

```python
>>> a = [1,2,3]
>>> b = a[:]
>>> a[0] = -50
>>> a
[-50, 2, 3]
>>> b
[1, 2, 3]
```

Vedle výše uvedených možností, jak pracovat se seznamy pomocí matematických operátorů a řezů, existují v Pythonu ještě tzv. *metody seznamů*, což jsou funkce, které se volají pomocí jména seznamu, tečky a jména dané funkce:

```python
>>> x = [1,2,3]

# Přidání nového prvku na konec seznamu metodou append
>>> x.append("ahoj")
>>> x
[1, 2, 3, 'ahoj']

# Vložení prvku na danou pozici seznamu metodou insert
>>> x.insert(1, 1.5) # Vloží hodnotu 1.5 na druhou pozici (tj. s indexem 1)
>>> x
[1, 1.5, 2, 3, 'ahoj']

# Seřazení seznamu čísel nebo textových řetězců metodou sort
>>> x = [2,9,-13,0,50]
>>> x.sort()
>>> x
[-13, 0, 2, 9, 50]
>>> x = ["ahoj","rohlik","kafe","zapalovac","sirky","snidane"]
>>> x.sort()
>>> x
['ahoj', 'kafe', 'rohlik', 'sirky', 'snidane', 'zapalovac']

# Zjištění indexu dané hodnoty metodou index
>>> x.index("sirky")
4

# Zjištění, kolikrát se daná hodnota v seznamu vyskytuje, metodou count
>>> x.count("sirky")
1

# Vymazání prvního výskytu dané hodnoty metodou remove
>>> x.remove("sirky")
>>> x
['ahoj', 'rohlik', 'kafe', 'zapalovac', 'snidane']

# Převrácení pořadí hodnot v seznamu metodou reverse
>>> x.reverse()
>>> x
['snidane', 'zapalovac', 'kafe', 'rohlik', 'ahoj']
```

Vedle metod seznamů jsou v Pythonu k dispozici další vestavěné funkce, pomocí nichž lze se seznamy pracovat (tyto funkce lze stejně dobře použít i pro řetězce):

```python
>>> a = [1,2,3]

# Zjištění délky seznamu funkcí len
>>> len(a)
3

# Zjištění nejmenšího a největšího prvku seznamu funkcí min a max
>>> min(a)
1
>>> max(a)
3

# Umazání části seznamu funkcí del
>>> a = [1,2,3,4,5]
>>> del(a[2])
>>> a
[1, 2, 4, 5]
>>> del a[:2]
>>> a
[4, 5]

```

Do pythonovského seznamu lze vkládat i jiné seznamy (tzv. *vnořené seznamy*) a vytvořit tak seznam seznamů, příp. seznam seznamů seznamů atd.:

```python
>>> a = [1, "ahoj", [1,2,3], ["a","b"]]
>>> a[2]
[1, 2, 3]
>>> a[3]
['a', 'b']
>>> a[3][0]
'a'
```

Hloubka vnoření může být značná (prakticky neomezená):

```python
>>> a = [[[[[1]]]]]
>>> a[0]
[[[[1]]]]
>>> a[0][0]
[[[1]]]
>>> a[0][0][0]
[[1]]
>>> a[0][0][0][0]
[1]
>>> a[0][0][0][0][0]
1
```

## *n*-tice a slovníky

*n*-tice (angl. *tuple*) je struktura ve všem podobná seznamům, až na to, že ji nelze měnit pomocí indexů. Oproti seznamům se zapisuje pomocí *kulatých* závorek:

```python
>>> a = (1,2,3)
>>> a[1]
2
>>> a[1] = 4
Traceback (most recent call last):
  File "<pyshell#43>", line 1, in <module>
    a[1] = 4
TypeError: 'tuple' object does not support item assignment
```

Důvod, proč existují *n*-tice a čím se v praxi liší od seznamů, si vysvětlíme později.

Dalším datovým typem podobným seznamu jsou tzv. *slovníky*. Slovník si lze představit jako seznam s pojmenovanými položkami. Jménům položek se říká *klíče*. Každou položku slovníku tak tvoří dvojice klíč-hodnota. Slovník se uvozuje složenými závorkami, jednotlivé položky jsou oddělené čárkami a klíč od příslušné hodnoty je oddělen dvojtečkou:

```python
>>> person = {"name": "Charles", "age": 60, "childern": 3}
>>> person
{'age': 60, 'name': 'Charles', 'childern': 3}
```

Základní odlišností slovníků od seznamů je, že ve slovníku se k jednotlivým položkám přistupuje nikoli pomocí indexů, ale právě pomocí *klíčů*:

```python
>>> person['name']
'Charles'
>>> person['age']
60
```

Seznam hodnot resp. klíčů daného slovníku lze obdržet pomocí metod `values` a `keys`:

```python
>>> person.keys()
['age', 'name', 'childern']
>>> person.values()
[60, 'Charles', 3]
```

Slovníky tvoří velmi užitečnou součást jazyka Python a možnosti jejich použití se zdaleka nevyčerpávají uvedenou malou ukázkou. V tomto kurzu s nimi však příliš pracovat nebudeme, proto necháváme hlubší seznámení s nimi na čtenáři. 

## Pravdivostní hodnoty

Pravdivostní hodnoty jsou jak známo dvě: *pravda* (anglicky *true*) a *nepravda* (anglicky *false*). Většina programovacích jazyků, Python nevyjímaje, disponuje zvláštním datovým typem pro reprezentaci těchto dvou hodnot. Většinou se takovému datovému typu říká *boolovský* (anglicky *Boolean*), na počest slavného britského matematika 19. století George Boola, který se významně zasloužil o rozvoj matematické logiky a někdy bývá označován i za praotce informatiky.

V pythonu je datový typ `bool` přímo odvozen od datového typu `int`, tedy celých čísel. Hodnota `True` (pravda) je tak vlastně ve skutečnosti celočíselná hodnota `1` (a může být jedničkou i zastoupena), hodnotě `False` zase odpovídá číslo `0` (a rovněž může být nulou zastoupena). Důsledkem toho je např. to, že hodnoty `True` a `False` mohou být součástí aritmetických výrazů s čísly:

```python
>>> True + 1
2
>>> False * 3
0
```

Velkou výhodou Pythonu je, že je schopen interpretovat v podstatě libovolnou hodnotu libovolného datového typu jako hodnotu pravdivostní. Jakou pravdivostní hodnotu má ta která hodnota lze přitom snadno zjistit pomocí konverzní funkce `bool`:

```python
>>> bool(0) 
False
>>> bool(5)
True
>>> bool(0.0) 
False 
>>> bool(-7.0)
True
>>> bool("") 
False 
>>> bool("Ahoj")
True
>>> bool([]) 
False 
>>> bool([1,2,3])
True
```

Pravdivostní hodnota je také výsledkem operací porovnání:

```python
>>> 5 > 3
True
>>> 5 <= 3
False
>>> "a" == "a"
True
```

## Hodnota `None`

V Pythonu, podobně jako ve většině jiných jazyků, existuje zvláštní hodnota reprezentující *nic*, neboli prázdnou hodnotu. Prázdná hodnota existuje v Pythonu jako datový objekt `None`, což si lze představit jako hodnotu, kterou lze přiřadit libovolné proměnné:

```python
>>> a = None
>>> a
>>>
```

Po zadání jména proměnné se nevypíše „None“, ale opravdu se nevypíše nic. Proměnná sice existuje, ale nic neobsahuje. Ve skutečnosti je při spuštění Pythonu vytvořen v operační paměti počítače datový objekt `None`, na který se odkazují všechny proměnné, kterým je hodnota `None` přiřazena (existuje tedy vždy jen jedno nic, bez ohledu na to, kolik proměnných je obsahuje).

Pravdivostní hodnota `None` je `False`:

```python
>>> bool(None)
False
```

## Shrnutí

V této kapitole jste se seznámili s pravidly práce s proměnnými a se základními datovými typy jazyka Python.

## Úlohy