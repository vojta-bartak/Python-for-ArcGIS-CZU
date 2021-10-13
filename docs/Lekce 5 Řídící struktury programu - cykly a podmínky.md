# Lekce 5: Řídící struktury programu - cykly a podmínky

Řídícími strukturami programu (též struktury řízení chodu programu) se rozumí syntaktické struktury jazyka, umožňující podmíněné a opakované provádění posloupnosti příkazů.

## Logické výrazy

*Logický výraz* (jinak též *pravdivostní výrok*) je jakýkoli výrok, kterému lze jednoznačně přisoudit pravdivostní hodnotu, tj. určit, zda je pravdivý či nepravdivý. Přisouzení pravdivostní hodnoty danému výrazu se označuje jako jeho *vyhodnocení*. Logické výrazy se v programování používají všude tam, kde je třeba provézt nějaký příkaz (či sérii příkazů) pouze tehdy, je-li splněna nějaká podmínka (takovým příkazům se říká *podmíněné příkazy*). Podmínka pak má podobu logického výrazu, přičemž její splnění/nesplnění je dáno pravdivostní hodnotou tohoto výrazu.

Porovnáním dvou hodnot lze vytvořit jednoduchý logický výraz, jehož pravdivostní hodnota je dána pravdivostí příslušného tvrzení:

```python
>>> 3 < 5
True
>>> 3 > 5
False
```

Porovnávat lze i textové řetězce. Porovnání je pak založeno na abecedním pořadí (tj. na tzv. *lexikografickém* *uspořádání*), přičemž velká písmena mají přednost před malými:

```python
>>> "ahoj" < "svete"
True
>>> "Ahoj" < "AHOJ"
False
```

Rovnost se testuje operátorem `==` (pozor, nezaměňovat s operací `=`, která přiřazuje hodnotu proměnné!):

```python
>>> a = 5
>>> a == 5
True
```

Nerovnost se testuje buď operátorem `!=` nebo `<>`:

```python
>>> a != 5
False
>>> a <> 3
True
```

Poněkud složitější je porovnávací operátor `is`. U čísel a řetězců testuje rovnost stejně jako `==`, ale u složitějších objektů, jako jsou seznamy, testuje totožnost objektů (tj. zda se jedná o tentýž objekt se shodnou adresou v paměti):

```python
>>> a = 3
>>> b = 3
>>> a is b
True
>>> a = [1,2,3]
>>> b = [1,2,3]
>>> a is b
False
>>> a = b
>>> a is b
True
```

Opakem `is` je operátor `is not`.

Operátor `in` testuje, zda je určitý prvek v daném seznamu, případně určitý znak v daném řetězci:

```python
>>> a = [1,2,3]
>>> 2 in a
True
>>> "a" in "ahoj"
True
```

Jeho opakem je `not in`:

```python
>>> a = [1,2,3]
>>> 4 not in a
True
>>> "z" not in "ahoj"
True
```

Složené logické výroky lze z jednoduchých vytvářet pomocí tzv. *logických operátorů* `and`, `or` a `not` (mluvíme pak o tzv. *logických operacích* s logickými výrazy). Význam těchto operátorů je všeobecně známý z logiky, proto výklad omezíme jen na malou ukázku. Např. pokud chceme otestovat, zda je hodnota nějaké číselné proměnné obsažena v určitém seznamu *a zároveň* zda je větší než 3, lze to provést pomocí operátoru `and`:

```python
>>> seznam = [1,2,3,4]
>>> a = 4
>>> a in seznam and a > 3
True
```

Struktura složeného výrazu může být libovolně složitá, obsahující libovolné množství dílčích jednoduchých i složitých výrazů. Pokud jsou dílčí výrazy rovněž složené, je často nutné zajistit požadovaný význam výrazu použitím závorek:

```python
>>> a in seznam and (a > 3 or a < -3)
True
```

Umístění závorek se řídí známou prioritou operací: nejprve se vždy vyhodnocuje `not`, pak `and`, posléze `or`. Pokud bychom v posledně uvedeném příkladu závorky vynechali, význam by se změnil (zodpovězte si: jak?). 

> **Úkol 1.**  Zadejte následující příkaz: `s = [1, 4, -10, 6, 13, -2]`. O následujících výrazech předem rozhodněte, zda jsou pravdivé či nepravdivé. Následně ověřte:
>
> `s[1] < s[3] or 3 in s and len(s) == s[3]`
>
> `s[1] < s[3] or (3 in s and len(s) == s[3])`
>
> `(s[1] < s[3] or 3 in s) and len(s) == s[3]`
>
> `s[1] > s[3] and 3 not in s or len(s) == s[3]`
>
> `s[1] > s[3] and (3 not in s or len(s) == s[3])`
>
> `(s[1] > s[3] and 3 not in s) or len(s) == s[3]`
>
> `s[1] > s[3] and not (3 not in s or len(s) == s[3])`
>
> `not (s[1] > s[3] and 3 not in s) or len(s) == s[3]`
>
> `s`

## Podmíněné příkazy

Příkaz se nazývá *podmíněný*, je-li jeho provedení vázáno splněním nějaké podmínky. Podmínkou přitom může být jakýkoli logický výraz, přičemž splnění podmínky je dáno jeho pravdivostní hodnotou. Syntaxe podmíněných příkazů v Pythonu je značně intuitivní, neboť kopíruje syntaxi běžného jazyka: pokud (anglicky *if*) platí podmínka, proveď následující příkaz(y):

```python
if podmínka: příkaz
```

V případě více příkazů:

```python
if podmínka:
    příkaz 1
    příkaz 2
    příkaz 3
    ...
```

Vidíme zde blokovou strukturu kódu, založenou na povinném odsazování. Příkazy vázané na splnění podmínky jsou vůči řádku s klíčovým slovem `if` odsazené o jednu úroveň (= čtyři mezery) doprava. Jakýkoli příkaz odsazený na úroveň slova `if` již nebude s podmínkou nijak souviset, tj. provede se bez ohledu na její splnění či nesplnění.

Uveďme příklad:

```python
>>> a = 5
>>> b = 6
>>> if a < 6: print a
    
5
>>> if b < 6: print b
    
>>> if a < b:
    print a
    print b
    print a,"je mensi nez",b

5
6
5 je mensi nez 6
```

> Pokud zapisujeme podmíněný příkaz do okna Python Shell, je po ukončení zápisu třeba stisknout klávesu `Enter` hned dvakrát. Druhým stisknutím klávesy `Enter` dáváme interpretu najevo, že jsme již skončili se zápisem a celý blok může být interpretován (tj. proveden). To je důvod, proč za posledním řádkem zápisu je vždy jeden prázdný řádek.

Vedle specifikace, co se má provézt za příkazy v případě splnění nějaké podmínky, lze také programu říct, co má provézt za příkazy, pokud naopak podmínka splněna není. Syntaxe je opět intuitivní: *pokud* (*if*) platí podmínka, proveď následující příkaz(y), *jinak* (anglicky *else*) proveď alternativní příkaz(y):

```python
if podmínka:
    příkaz 1
    příkaz 2
else:
    příkaz 3
    příkaz 4
```

Konkrétní příklad tentokrát zapíšeme pomocí skriptu:

```python
a = 5
b = 6
if a == b:
    print(str(a) + " se rovna " + str(b))
else:
    print(str(a) + " se nerovna " + str(b))
```

Po spuštění skriptu se v okně Python Shell vypíše:

```python
5 se nerovna 6
```

Často také nastává případ, že potřebujeme postupně otestovat více různých podmínek, při jejichž splnění se vždy má provést odpovídající příkazy. Struktura zápisu je: *pokud* platí podmínka 1, proveď příkaz(y) 1, *jinak* *pokud* platí podmínka 2, proveď příkaz(y) 2, *jinak* proveď příkaz(y) 3. Podmínek je přitom možné za sebe zřetězit libovolné množství a celý zápis může, ale nemusí být ukončen závěrečnou částí *else*, za níž následují příkazy provedené v případě, kdy není splněna žádná z předchozích podmínek.

Jednou možností, jak výše popsané schéma realizovat, je následující:

```python
a = 5
b = 6
if a == b:
    print a,"se rovna",b
else:
    if a < b:
        print a,"je mensi nez",b
	else:
        print a,"je vetsi nez",b
```

Jak je vidět, tímto způsobem dochází při zřetězení podmínek k postupnému zanořování dalších a dalších podmíněných příkazů do stále hlubší úrovně, což by při větším množství testovaných podmínek nebylo příliš přehledné. Z toho důvodu existuje v Pythonu alternativní způsob zápisu, spočívající v použití klíčového slova `elif`, které je vlastně zkráceninou `else` `if`. Způsob použití je patrný z ukázky (opět platí, že sekvence `if`-`elif`-`elif`-...-`elif` může a nemusí být ukončena částí `else`):

```python
a = 5
b = 6
if a == b:
    print a,"se rovna",b
elif a < b:
    print a,"je mensi nez",b
else:
    print a,"je vetsi nez",b
```
> **Úkol 2.** Napište skript, který pro zadaná dvě čísla rozhodne, zda je větší z nich dělitelné menším.

## Cyklus `while`

Někdy potřebujeme provádět nějaký příkaz či sérii příkazů opakovaně, dokud nenastane (či naopak neskončí) platnost nějaké podmínky. K tomu slouží cyklus *while*. V Pythonu je jeho syntaxe následující:

```python
while podminka:
    příkaz 1
    příkaz 2
    ...
    příkaz n
```

Dokud podmínka platí, bude se neustále provádět posloupnost příkazů 1, 2, ... , n. Je důležité, aby v rámci těchto příkazů dříve či později došlo k tomu, že podmínka již neplatí. V opačném případě by vznikl tzv. *nekonečný cyklus*. Program bychom pak museli „násilně“ přerušit klávesovou zkratkou `Ctrl`+`C`, příp. v krajním případě pomocí Správce úloh systému Windows.

Příkladem použití cyklu `while` může být výpočet faktoriálu daného přirozeného čísla, třeba 20:

```python
n = 20
fakt = 1
i = 2
while i <= n:
    fakt = fakt * i
	i = i + 1
print(fakt)
```

Podmínkou pro pokračování cyklu je zde platnost výroku `i <= n`. Tento výrok zcela jistě jednou platit přestane, neboť v každém kroku je hodnota proměnné `i` o jednotku zvětšena, takže jednou určitě překročí hodnotu `n`. Jakmile se tak stane, cyklus se již provádět nebude a program bude pokračovat prováděním příkazů (případně) uvedených za cyklem, tj. odsazených na úroveň slova `while`. V našem případě vytiskne výsledek do okna Python Shell:

```python
2432902008176640000
```

Pro různé hodnoty proměnné `n` budeme samozřejmě dostávat různé výsledky faktoriálu. 

Cyklus `while` se dá vlastně považovat za zvláštní druh podmíněného příkazu, neboť určuje, co má program provést v případě splnění nějaké podmínky. Dokonce ho lze zkombinovat s klauzulí `else`, za níž následuje posloupnost příkazů, která se má provést, pakliže již podmínka splněna není (tj. po konci celého cyklu):

```python
while podmínka:
    příkazy 1 až n
else:
    příkazy n+1 až m
```

Lze namítnout, že stejně se bude program chovat, pokud namísto části `else` napíšeme příkazy n+1 až m za cyklus na úroveň slova `while`. To je pravda a použití `else` u cyklu `while` je skutečně velmi řídké. Přesto však není tato možnost zcela bez účelu, jak uvidíme v kapitole o příkazech `break` a `continue`.

> **Úkol 3.** Vytvořte seznam s Fibonacciho posloupností délky 100 pomocí cyklu `while`.

## Cyklus `for`

Cyklus `for` použijeme všude tam, kde chceme procházet nějaký seznam a s každou jeho položkou něco vykonat. Syntax je následující: 

```python
for polozka in seznam:
    příkaz 1 (ve kterém se může použít proměnná položka)
    příkaz 2 (ve kterém se může použít proměnná položka)
    ...
```

Např. můžeme jednotlivé položky seznamu jednoduše vytisknout:

```python
seznam = [0,"jedna",2,"tri",4,"pet"]
for i in seznam: 
    print(i)
```

Výsledkem bude výpis

```python
0
jedna
2
tri
4
pet
```

Stejnou úlohu by šlo vyřešit i pomocí cyklu `while`:

```python
i = 0
while i < len(seznam):
    print seznam[i]
    i = i + 1
```

Cyklus `for` lze využít všude tam, kdy dopředu víme, jak dlouhý náš cyklus bude. V případě, že např. chceme nějakou posloupnost příkazů opakovat 20krát, stačí si vyrobit seznam dané délky a ten cyklem `for` projít. Položky procházeného seznamu přitom vůbec nemusíme v jednotlivých příkazech použít:

```python
for i in [1]*20:
    print("Nebudu rušit při hodině hlasitým krkáním.")
```

Po spuštění se vypíše

```python
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
Nebudu rušit při hodině hlasitým krkáním.
```

Často se hodí procházet seznam čísel od jedné do *n* (třeba do dvaceti). K tomu může dobře posloužit funkce `range`:

```python
>>> range(20) # Vrátí seznam délky 20, začíná se nulou
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19]

>>> range(5, 10) # Vrátí seznam od 5 do 9
[5, 6, 7, 8, 9]

>>> range(5, 50, 3) # Vrátí seznam od 5 do 49 s krokem 3
[5, 8, 11, 14, 17, 20, 23, 26, 29, 32, 35, 38, 41, 44, 47]

>>> range(50, 5, -3) # Vrátí seznam od 50 do 6 s krokem -3
[50, 47, 44, 41, 38, 35, 32, 29, 26, 23, 20, 17, 14, 11, 8]
```

Výše uvedený výpočet faktoriálu by tedy mohl vypadat i takto:

```python
n = 20
fakt = 1
for i in range(n):
    fakt = fakt * i
print(fakt)
```

Cyklus `for` je možné i vnořit do hranatých závorek a vytvořit tak seznam. Následující kód vytváří seznam druhých mocnin čísel od 1 do 10:

```python
>>> a = [x**2 for x in range(1,11)]
>>> a
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

Samozřejmě bychom stejný seznam mohli vytvořit i méně stručně:

```python
>>> a = []
>>> for x in range(1,11): a.append(x**2)
>>> a
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

První způsob je zjevně elegantnější a u zkušenějších programátorů se s ním setkáte častěji.

> **Úkol 4.** Vytvořte seznam s Fibonacciho posloupností délky 100 pomocí cyklu `for`.

## Příkazy `break` a `continue`

Příkazy `break` (anglicky *přeruš*) a `continue` (anglicky *pokračuj*) slouží k přerušení cyklu, nastane-li pro to nějaký důvod. Typicky se proto používají jako součást podmíněného příkazu, jak je vidět z následujícího schematického znázornění syntaxe příkazu `break` (uvádíme verzi s cyklem `while` i verzi s cyklem `for`):

```python
# Verze s cyklem while:
while podmínka1:
    první série příkazů
    if podmínka2:
        druhá série příkazů
        break
	else:
		třetí série příkazů
else:
	čtvrtá série příkazů
pátá série příkazů

# Verze s cyklem for:
for položka in seznam:
    první série příkazů
	if podmínka2:
		druhá série příkazů
		break
	else:
		třetí série příkazů
else:
	čtvrtá série příkazů
pátá série příkazů
```

(První `else` se vztahuje k `if`, zatímco druhé `else` k cyklu `while` resp. `for`.)

Příkaz `break` způsobí okamžité přerušení cyklu, přičemž pokud existuje (což nemusí!) i část `else` vztahující se k cyklu, pokračuje program až za ní. V naší schematické ukázce to znamená, že jakmile v nějakém kroku (iteraci) cyklu platí podmínka u `if`, tj. narazí se na příkaz `break`, program automaticky cyklus přeruší a pokračuje prováděním páté série příkazů.

Ukázkou použití příkazu `break` u cyklu `for` může být např. skript hledající nějakou konkrétní položku (např. číslo 20) v nějakém seznamu:

```python
for cislo in seznam:
    if cislo == 20:
		print("Prave jsem nasel cislo 20!")
		break
else: print("Cislo 20 v seznamu neni!")
```

Příkaz `continue` způsobí, že je přerušen pouze právě probíhající běh cyklu a program pokračuje dalším krokem (iterací) cyklu. Bývá používán méně často než příkaz `break`, neboť většinou lze jeho použití nahradit vhodně zvoleným podmíněným příkazem. Z tohoto důvodu nebudeme uvádět ukázku.

## Shrnutí

## Úlohy

1. Z posloupnosti vzniklé řešením úkolu 3 resp. 4 vytvořte seznam obsahující pouze členy dělitelné třemi a zjistěte jeho délku.
2. Napište skript, který pro zadané dva seznamy délky *m* a *n*, kde *m* > *n*, vytvoří nový seznam, jehož prvních *n* prvků bude tvořeno prvky kratšího z obou seznamů, zbytek seznamu bude doplněn prvky delšího z obou seznamů. Řešení vytvořte tak, aby program fungoval při zadání libovolných dvou seznamů v libovolném pořadí.
3. Napište skript pro porovnání dvou číselných seznamů "položku po položce". Vstupem do programu budou dva libovolné seznamy a typ porovnání ("větší než", "menší než" nebo "rovná se"). Výstupem bude seznam délky kratšího z obou seznamů, který na indexu *i* bude obsahovat výsledek porovnání (`True`/`False`) příslušných položek vstupních seznamů. Např. pro seznamy `a = [1,2,3]` a `b = [1,-2,4,5]` bude výsledkem porovnání "větší než" seznam `[False, True, False]`. 
4. Napište skript, který pro zadaná dvě čísla rozhodne, zda jsou soudělná.