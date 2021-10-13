# Lekce 6: Interaktivní vstupy do programu a náhodná čísla

V této lekci si ukážeme, jak lze v pythonu interaktivně zadávat vstupy do nějakého programu, který spouštíme na konzoli (např. v prostředí IDLE). Vstupy se zadávají pomocí funkce `input`. Protože její vlastnosti jsou odlišné v Pythonu řady 2.x a řady 3.x, probereme obě varianty zvlášť. Dále si ukážeme, jak v Pythonu generovat náhodná čísla.

## Funkce `input` a `raw_input` v Pythonu 2.x 

Ve verzích Pythonu 2.x existují dvě funkce, `input` a `raw_input`.

Funkce `input` vypíše na konzoli zadaný text a následně čeká, až uživatel zadá na konzoli vstup:

```python
>>> input("Milý uživateli, zadej číslo:")
Milý uživateli, zadej číslo:
```

Po zadání a potvrzení klávesou `Enter` funkce vrátí hodnotu zadanou uživatelem, s tím, že automaticky vyhodnotí, o jaký datový typ se jedná:

```python
>>> input("Milý uživateli, zadej číslo:")
Milý uživateli, zadej číslo: 5
5
```

V tomto případě funkce automaticky poznala, že se jedná o číslo, neboť bylo zadáno číslo bez uvozovek. V případě, že bychom chtěli, aby funkce vstup přečetla jako textový řetězec, museli bychom vstup zadat v uvozovkách:

```python
>>> input("Milý uživateli, zadej text:")
Milý uživateli, zadej text: "5"
'5'
```

 Podobně lze zadávat i ostatní datové typy:

```python
>>> input("Milý uživateli, zadej seznam:")
Milý uživateli, zadej seznam: [1,2,3,4]
[1, 2, 3, 4]
```

nebo např. volat funkce:

```python
>>> input("Milý uživateli, zadej seznam od 0 do 5:")
Milý uživateli, zadej seznam od 0 do 5: range(6)
[0, 1, 2, 3, 4, 5]
```

Funkce `raw_input` oproti tomu vrací jakýkoli vstup v podobě textového řetězce:

```python
>>> raw_input("Milý uživateli, zadej číslo:")
Milý uživateli, zadej číslo:5
'5'

>>> raw_input("Milý uživateli, zadej seznam:")
Milý uživateli, zadej seznam:[1,2,3,4]
'[1,2,3,4]'

>>> raw_input("Milý uživateli, zadej seznam od 0 do 5:")
Milý uživateli, zadej seznam od 0 do 5:range(6)
'range(6)'
```

## Použití vstupu do dalšího výpočtu

Ve výše uvedených ukázkách jsme výsledek volání funkce `input` pouze nechávali vypsat na konzoli. Zpravidla s ním však chceme provádět nějaké další výpočty či operace. To zajistíme tím, že výsledek volání funkce `input` resp. `raw_input` uložíme do proměnné:

```python
>>> a = input("Milý uživateli, zadej číslo:")
Milý uživateli, zadej číslo:5
>>> a + 2
7
```

V případě funkce `raw_input` nesmíme však zapomenout na převedení textového vstupu na požadovaný datový typ:

```python
>>> a = raw_input("Milý uživateli, zadej číslo:")
Milý uživateli, zadej číslo:5
>>> int(a) + 2
7
```

Je také možné využít funkci `eval` (z angl. "evaluate" čili "vyhodnoť"), která vezme textový vstup a pokusí se jej interpretovat jako kód v Pythonu a vrátit požadovaný výstup:

```python
>>> seznam = raw_input("Milý uživateli, zadej seznam od 0 do 5:")
Milý uživateli, zadej seznam od 0 do 5:range(6)
>>> seznam
'range(6)'
>>> eval(seznam)
[0, 1, 2, 3, 4, 5]
```

Chování funkce `input()` lze tedy věrně imitovat použitím `eval(raw_input())`:

```python
>>> seznam = eval(raw_input("Milý uživateli, zadej seznam od 0 do 5:"))
Milý uživateli, zadej seznam od 0 do 5:range(6)
>>> seznam
[0, 1, 2, 3, 4, 5]
```
> **Úkol 1.** Napište program, který uživatele vyzve k zadání přirozeného čísla a následně vrátí jeho faktoriál.

> **Úkol 2.** Napište program, který uživatele postupně vyzve k zadání dvou čísel a následně sdělí, zda je větší z nich dělitelné menším.

## Funkce `input` v Pythonu 3.x

Ve verzích Pythonu 3.x došlo k následujícím změnám:

- funkce `raw_input` byla zrušena,
- chování funkce `input` bylo změněno na původní chování funkce `raw_input`.

To znamená, že k interaktivnímu vstupu přes konzoli již slouží pouze funkce `input` a vstup je vždy předáván jako textový řetězec. Pokud s ním dále chcete pracovat jako s číslem, seznamem apod., je třeba použít přetypování (konverzní funkce `int`, `float`, `list` apod.), případně funkci `eval`.

## Generování náhodných čísel

Ke generování náhodných čísel slouží v Pythonu modul `random`, který je součástí základní výbavy jazyka. Modul je třeba nejprve načíst příkazem `import`:

```python
>>> import random
```

K jednotlivým funkcím modulu se pak dostaneme, jak je v Pythonu obvyklé, přes jméno modulu, tečku a jméno funkce. Např. funkce `random()` bez argumentů generuje náhodné číslo z intervalu (0, 1) (z rovnoměrného rozdělení):

```python
>>> import random
>>> random.random()
0.6744697203253585
>>> random.random()
0.7169711430089877
>>> random.random()
0.4213975602144999
```

Modul lze načíst a dále používat i pod zvoleným alternativním (např. kratším) názvem:

```python
>>> import random as rn
>>> a = rn.random()
>>> a
0.8591162717665027
```

Funkce `randint(a, b)` vrací náhodné celé číslo z intervalu <`a`, `b`> (včetně obou mezí):

```python
>>> rn.randint(-100, 100)
70
>>> rn.randint(-100, 100)
-89
```

Funkce `choice(seq)` vrací náhodně vybraný prvek z posloupnosti (seznamu, *n*-tice apod.) `seq`:

```python
>>> seznam = [1,"dva",3,"ctyri",5]
>>> rn.choice(seznam)
5
>>> rn.choice(seznam)
'dva'
```

Funkce `sample(population, k)` vrací náhodný výběr velikosti `k` z posloupnosti `population`:

```python
>>> rn.sample(range(100), 10)
[10, 67, 93, 45, 56, 17, 71, 8, 91, 9]
```

Funkce `uniform(a, b)` vrací náhodné číslo generované z rovnoměrného rozdělení na intervalu <`a`,`b`>. (Poznámka: stejné jako `a + random() * abs(a – b)`.)

```python
>>> rn.uniform(2,4)
2.8693870960257666
```

Funkce `gauss(mu, sigma)` vrací náhodné číslo generované z normálního rozdělení se střední hodnotou `mu` a směrodatnou odchylkou `sigma`:

```python
>>> rn.gauss(0, 1)
-0.072104281974643761
```

Generátor opravdu náhodných čísel pomocí počítače neexistuje (a v principu existovat nemůže). Čísla, která označujeme za "náhodná" jsou tzv. *pseudonáhodná*, tj. taková, která se jako náhodná pouze tváří. Ve skutečnosti je každé další číslo v posloupnosti "náhodných" čísel složitým algoritmem přesně (a vždy stejně) vypočteno z čísla předchozího. V čem tedy spočívá jejich "náhodnost"? Spočívá v tom, že vygenerujeme-li libovolně dlouhou posloupnost, bude se vždy ze statistického hlediska "tvářit" jako náhodná. Žádný statistický test neodhalí souvislost mezi po sobě následujícími čísly.

Z uvedeného plyne, že pokud bychom daným generátorem postupně generovali dvě řady náhodných čísel, přičemž bychom začali od stejného čísla, obě řady by byly identické (ačkoli by se každá z nich sama o sobě tvářila jako náhodná). Tento problém se v praxi řeší tím, že se pokaždé začne jiným číslem, tzv. *semínkem* (anglicky *seed*). Defaultně se semínko v danou chvíli odvodí z aktuálního času v počítači.

Semínko je ale možné i nastavit pevně, což se hodí, chceme-li např. opakovaně testovat nějaký algoritmus, pracující s náhodnými čísly, přičemž chceme, aby do každého testu vstupovaly stejné hodnoty. K nastavení semínka slouží funkce `seed`:

```python
>>> rn.seed(123)
>>> rn.random()
0.052363598850944326
>>> rn.random()
0.08718667752263232
>>> rn.random()
0.4072417636703983
>>> rn.seed(123)
>>> rn.random()
0.052363598850944326
>>> rn.random()
0.08718667752263232
>>> rn.random()
0.4072417636703983
```

Jak je z ukázky vidět, po nastavení semínka na stejnou hodnotu (v našem případě 123) tvoří následující sekvenci stejná čísla.

> **Úkol 3.** Vygenerujte 1000 čísel z rovnoměrného rozdělení na intervalu (0, 1) a zjistěte, kolik procent z nich je větších než 0,7.

## Shrnutí

## Úlohy

1. Vytvořte program, který vás vyzkouší z malé násobilky. Program náhodně vygeneruje příklad a vyzve vás, abyste zadali výsledek. V případě špatného výsledku vám vynadá a sdělí vám správný, v případě správného výsledku vám pogratuluje.
2. Vytvořte program, který bude zkoušet z malé násobilky tak, v případě špatného výsledku vás bude nutit zadat výsledek ještě jednou, a to tak dlouho, dokud nezadáte správný výsledek.
3. Modifikujte program z úlohy 2 tak, aby se vás po ukončení příkladu zeptal, zda si přejete pokračovat dalším příkladem. Pokud zvolíte "ano" (například tak, že zadáte "Y" jako "YES"), dá vám další příklad. Pokud zvolíte "ne" (např. "N" jako "NO"), ukončí se.
4. Vygenerujte 10 000 čísel z normálního rozdělení se střední hodnotou 13 a směrodatnou odchylkou 2,3. Spočítejte jejich průměr a (výběrovou) směrodatnou odchylku. Vypočítejte stejné hodnoty z náhodného výběru 100 čísel z vygenerovaného souboru 10 000 čísel. Které hodnoty jsou blíže střední hodnotě a směrodatné odchylce původního normálního rozdělení? 