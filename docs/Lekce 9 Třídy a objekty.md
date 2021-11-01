# Lekce 9: Třídy a objekty

Třídy a objekty jsou základem většiny moderních programovacích jazyků. Snad každý už někdy slyšel o "objektovém programování" či o "objektově orientovaných jazycích". V této kapitole si vysvětlíme a na malé ukázce předvedeme, o co jde. Nepůjde nám přitom o zvládnutí samotného objektového programování, to by přesahovalo časové možnosti tohoto kurzu. Půjde spíš o to pochopit základní strikturu objektů a tříd tak, abyste byli schopni pracovat s objekty a třídami, které již někdo naprogramoval před vámi. Jak uvidíte v další části kurzu, při práci s funkcionalitou ArcGIS pomocí Pythonu jsou objekty všudypřítomné.

Třídu si lze představit jako jakýsi složitý datový typ. Objekt je pak konkrétní realizace (tzv. *instance*) této třídy, podobně jako konkrétní seznam je instancí obecného datového typu `list`. Příkladem může být třída "Pes". V rámci definice této třídy můžeme určit, co bude každý konkrétní objekt této třídy, neboli každý konkrétní "pes", obsahovat. Každá třída může obsahovat:

- **vlastnosti**, neboli proměnné. Stejně jako každá jiná proměnná, i každá vlastnost je určitého datového typu. V případě naší třídy "Pes" to může být: věk (číslo), jméno (text) apod.
- **metody**, neboli funkce. V rámci třídy můžeme definovat funkce, které bude mít každá konkrétní instance této třídy k dispozici. Metody objektu často něco dělají s jeho vlastnostmi. Např. naše třída "Pes" by mohla obsahovat metodu "štěkni", která by po zavolání vypsala na konzoli text "Haf", případně "Haf haf, já jsem ...", kde místo tří teček by bylo jméno daného psa, uložené v té chvíli ve vlastnosti "jméno". Dále by mohla obsahovat metodu "zestárni", která by zvýšila hodnotu vlastnosti "věk" o jedna.

## Definice třídy

Pojďme si třídu pes vytvořit. Definice třídy začíná klíčovým slovem `class`, následuje *tělo třídy* odsazené o jednu úroveň doprava:

```python
class Dog:

    def __init__(self, name):
        self.name = name
        self.age = 0

    def woof(self):
        print("Woof woof, I'm " + self.name)

    def get_older(self):
        self.age = self.age + 1
```

Vidíme, že tělo třídy obsahuje postupně definice třech funkcí. První z nich je tzv. *konstruktor* třídy a budeme se mu věnuvat níže. Druhé dvě funkce, tedy `woof` a `get_older`, jsou metody, které bude mít každá instance této třídy k dispozici. Jediným parametrem obou funkcí je parametr `self`, pomocí kterého se lze uvnitř funkcí odkazovat na vlastnosti daného objektu (`self.name` a `self.age`). Parametr `self` je u všech metod povinný, vedle něj mohou metody obsahovat i další parametry.

Vidíme, že metoda `woof` pouze vytiskne text, ve kterém se použije jméno daného psa, na němž se bude metoda volat (toto jméno bude totiž uloženo ve vlastnosti `self.name`). Metoda `get_older` zvýší hodnotu vlastnosti `self.age` o jedna.

**Konstruktor třídy** se jmenuje vždy stejně, a to `__init__` (před a po slově `init` jsou dvě podtržítka za sebou). Je to funkce, která se automaticky volá při vytváření každé nové instance (tj. objektu) třídy. V této funkci jsou také definovány vlastnosti, které bude každý pes mít k dispozici. Jsou to proměnné `self.name` a `self.age`. Slovo `self` zde vždy odkazuje k budoucí instanci třídy a je povinným (prvním) parametrem každé metody.

Co tedy bude dělat konstruktor třídy `Dog`? Nastaví hodnotu vlastnosti `self.name` na hodnotu parametru `name` a hodnotu vlastnosti `self.age` na hodnotu 0. Parametr `name` konstruktoru bude muset uživatel při vytváření objektu zadat (nebude tedy možné vytvořit bezejmenného psa).

> **Úkol 1.** Napište shora uvedenou definici třídy `Dog` do skriptu a skript spusťte.

Vytvoření konkrétního objektu třídy se provádí voláním funkce, která má stejný název jako daná třída. Ve skutečnosti se tím volá konstruktor třídy. Při volání této funkce je třeba určit hodnoty parametrů v tom pořadí, jak jsou definovány v konstruktoru (ovšem s vynecháním parametru `self`). Funkce vrací objekt dané třídy, který je zpravidla žádoucí uložit do nějaké proměnné.

V našem případě se bude tedy funkce jmenovat `Dog` a jejím jediným parametrem bude `name`:

```python
>>> alik = Dog("Alik")
```

Proměnná `Alik` teď obsahuje objekt (neboli instanci) třídy `Dog` (objekty jsou ve výpisech na konzoli vždy poznatelné podle špičatých závorek):

```python
>>> alik
<__main__.Dog instance at 0x02EBE8F0>
```

K vlastnostem a metodám objektu lze přistupovat pomocí názvu objektu a tečky (metody se od vlastností rozeznají tím, že mají za sebou kulaté závorky: jsou to totiž funkce):

```python
>>> alik.name
'Alik'
>>> alik.woof()
Woof woof, I'm Alik
>>> alik.age
0
>>> alik.get_older()
>>> alik.age
1
```

Objektů určité třídy lze vytvářet prakticky neomezené množství, každý bude mít přitom své vlastní instance všech vlastností a metod, tak jak jsou v dané třídě definovány:

```python
>>> zeryk = Dog("Zeryk")
>>> zeryk.woof()
Woof woof, I'm Zeryk
>>> zeryk.age
0
>>> zeryk.get_older()
>>> zeryk.get_older()
>>> zeryk.age
2
>>> alik.age
1
```

V Pythonu jsou všechno objekty..

```python

```

```python

```

```python

```


## Vlastnosti objektu vs vlastnosti třídy

## Dědičnost

## Úlohy

