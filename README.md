# Semestrální práce ZS 2017/2018

V případě dotazů založte [novou issue v tomto repozitáři](https://github.com/3DprintFIT/B171CW-Assignment/issues/new).

***Alternativní zadání práce:** Po dohodě s Mirem Hrončokem
můžete místo této semestrální práce řešit issues [projektů
ADMesh](https://github.com/admesh/). Kolik jich
musíte vyřešit a jak budete bodově ohodnoceni, závisí na
proběhlé dohodě. V případě zájmu se prosím ozvěte co nejdříve,
je zde jen omezené množství práce a tedy je alternativa k
semestrálce možná jen pro malý počet studentů.*

## 3D vločka

Tématem semestrální práce jsou 3D vločky. Vaším úkolem je v programu
OpenSCAD napsat modul, který umí generovat modely 3D vloček na základě
vstupních dat, vhodně připravit vám přidělenou vločku k tisku,
vytisknout ji a provést případné dodatečné úpravy.

### Generátor 3D vloček

Napište OpenSCAD modul `flake` dodržující předepsané rozhraní, který na
základě vstupních dat zobrazí/vytvoří 3D vločku.

```cpp
module flake(branches=[], circr=30, r=3, ang=45) { ... }
```

Vektor (pole) `branches` obsahuje malá přirozená čísla. Pro každý prvek
tohoto pole bude mít vločka jednu odnož, která se následně rozdělí na N
dalších, kde N je hodnota prvku. Například pro pole `[1, 2, 2, 3, 5]`
bude vločka mít 5 odnoží. Dvě se rozdělí na 2, jedna na 3 a jedna na 5
pod-odnoží. Jedna odnož se dělit nebude.

Číslo `circr` je poloměr opsané koule vločky. Číslo `r` je poloměr
odnoží (i pod-odnoží). Číslo `ang` je úhel ve stupních, pod kterým se
větví pod-odnože (pokud je pod-odnož jen jedna, je rovně (`ang` se
neberu v úvahu)), úhel se počítá tak, že 0 je rovná tyčka, 90 je kolmá
(úhly mimo tento interval nemusíte řešit). Více o anatomii vločky se
dozvíte dále z nákresu.

Odnože musí ze středu vločky vycházet do různých směrů tak, aby byly co
nejstejněji rozdistribuované. Pokud bychom měli 2D vločku, stačilo by
odnože rozmístit tak, aby mezi sebou měli úhel `360/(počet odnoží)`,
protože ale máme vločku 3D, není tento problém úplně triviální. Zajímají
vás algoritmy který umí rovnoměrně rozmístit N bodů na povrchu koule,
které již existují, jen je třeba je adaptovat pro OpenSCAD (ten neumí
inkrementovat hodnoty průchodem cyklu) (tip: `cos()` a `sin()` v
OpenSCADu očekává vstup ve stupních, konstanta `PI` již existuje).
Zajímají vás algoritmy, které dávají dobré výsledky i pro malá čísla,
protože odnoží nebude mnoho. Vzhledem k tomu, že váš algoritmus na
rovnoměrné rozmisťování odnoží může fungovat různými způsoby, je pořadí
prvků ve vektoru `branches` irelevantní - můžete o tomto vektoru
uvažovat jako o multisetu.

**Poznámka:** *Na rozmístění odnoží nemá vliv počet jejich pod-odnoží. Při
řešení rozmístění odnoží uvažujte každou odnož stejnou.*

### Anatomie 3D vločky

Na tomto obrázku je naznačeno, jak jednotlivé odnože vycházejí ze středu
vločky směrem k opsané kouli (ta koule není součástí vločky, pouze
rozměrová informace):

![](assets/male.svg)

A zde vidíte jednotlivé rozměry a tvar jedné odnože. Proměnná `x` je
třeba dopočítat vzhledem k pevně zadaným vstupům:

![](assets/velke.svg)

Jednotlivé pod-odnože jsou vytočené o úhel `ang` a rovnoměrně
rozmístěné:

![](assets/branch.png)

Pokud je pod-odnož jen jedna, je rovně (vytočení o `ang` se nekoná).
Její délka není `7x` z obrázku výše, ale méně (tak aby dosáhla na, ale
nepřesáhla opsanou kouli).

Celkový pohled:

![](assets/flake.gif)

### Vaše vločky

Každému z vás jsou přiděleny tři náhodně vygenerované multisety
`branches`. Je na vás, který z nich si vyberete k tisku. Vločky se
tisknou s ostatními argumenty nastavenými na výchozí hodnoty.

```
an....re [2, 3, 3, 2, 4, 2, 1, 3, 2, 4, 1, 4] [2, 3, 1, 4, 3, 5, 3, 3, 1, 1, 3, 3, 3] [2, 4, 2, 5, 3, 2, 4, 4, 4, 3, 3, 4, 2, 2, 3, 1, 2, 1]
as....ai [5, 4, 1, 4, 3, 3, 2, 4, 3, 3, 3, 3] [3, 4, 2, 2, 1, 2, 2, 2, 1, 5, 2, 4, 4] [2, 2, 2, 3, 4, 2, 5, 5, 3, 1, 2, 4, 4, 3, 1, 2, 4, 3]
ba....ic [2, 2, 3, 5, 3, 3, 5, 2, 3, 2, 2, 2] [4, 3, 3, 5, 3, 4, 1, 3, 3, 3, 2, 2, 5] [3, 2, 2, 3, 4, 3, 3, 2, 2, 3, 5, 3, 3, 5, 3, 4, 2, 1]
ba....37 [3, 4, 4, 2, 3, 3, 4, 3, 3, 2, 2, 3] [2, 5, 2, 3, 3, 3, 1, 2, 3, 2, 2, 3, 5] [3, 5, 3, 3, 1, 1, 2, 1, 2, 3, 3, 2, 2, 2, 5, 4, 2, 2]
be....te [4, 1, 2, 5, 5, 2, 2, 3, 2, 2, 2, 2] [2, 3, 1, 2, 3, 2, 4, 3, 3, 1, 2, 3, 2] [2, 3, 3, 3, 3, 5, 2, 2, 4, 5, 5, 3, 3, 3, 2, 2, 3, 3]
be....an [2, 3, 5, 5, 2, 4, 5, 3, 3, 3, 2, 4] [3, 2, 1, 3, 3, 2, 1, 4, 2, 3, 3, 1, 3] [4, 4, 5, 3, 2, 4, 1, 3, 2, 4, 2, 2, 3, 3, 2, 5, 3, 4]
bl....15 [2, 3, 5, 4, 5, 3, 2, 1, 3, 1, 2, 2] [1, 2, 2, 4, 2, 3, 2, 2, 1, 3, 3, 2, 4] [3, 2, 2, 1, 4, 3, 3, 2, 3, 2, 4, 3, 3, 2, 5, 5, 2, 2]
br....an [3, 3, 3, 2, 3, 3, 4, 1, 2, 1, 2, 1] [4, 1, 4, 3, 3, 3, 3, 3, 5, 5, 2, 3, 3] [2, 2, 5, 4, 3, 1, 3, 3, 2, 5, 4, 2, 3, 2, 4, 3, 2, 2]
ce....o1 [2, 3, 4, 2, 3, 4, 2, 5, 5, 3, 4, 2] [2, 3, 2, 3, 3, 1, 3, 2, 3, 4, 1, 3, 2] [2, 3, 3, 4, 1, 3, 4, 5, 1, 2, 2, 5, 1, 3, 5, 2, 3, 3]
ca....a9 [5, 4, 3, 4, 1, 1, 2, 2, 3, 1, 2, 3] [2, 2, 5, 4, 3, 3, 4, 4, 1, 1, 3, 5, 4] [4, 1, 3, 2, 3, 3, 4, 2, 2, 3, 5, 3, 4, 5, 4, 4, 4, 2]
ci....ch [2, 2, 3, 2, 1, 3, 2, 2, 2, 2, 4, 2] [4, 2, 4, 2, 2, 3, 1, 4, 2, 4, 1, 4, 3] [3, 3, 3, 1, 1, 1, 3, 2, 5, 3, 2, 2, 3, 2, 1, 4, 3, 3]
ct....a2 [1, 3, 4, 3, 2, 3, 1, 2, 2, 2, 1, 2] [4, 2, 2, 2, 2, 2, 2, 1, 3, 5, 2, 4, 4] [2, 2, 2, 5, 3, 4, 5, 2, 2, 1, 1, 3, 2, 3, 2, 3, 4, 2]
da....oj [2, 3, 3, 4, 2, 3, 3, 4, 2, 2, 3, 4] [2, 2, 4, 4, 3, 2, 4, 3, 4, 4, 4, 4, 2] [2, 2, 2, 3, 2, 4, 4, 4, 2, 2, 2, 2, 3, 4, 4, 3, 4, 1]
de....nd [3, 4, 1, 2, 4, 4, 5, 2, 5, 3, 4, 2] [1, 2, 3, 4, 2, 3, 5, 1, 2, 3, 4, 2, 3] [3, 3, 2, 3, 2, 2, 4, 2, 4, 3, 4, 2, 3, 2, 3, 3, 4, 2]
do....12 [3, 5, 3, 3, 3, 3, 2, 1, 4, 3, 2, 2] [3, 1, 2, 4, 1, 3, 3, 2, 3, 4, 3, 3, 3] [5, 4, 2, 1, 5, 3, 5, 3, 3, 3, 2, 4, 4, 5, 3, 3, 3, 2]
du....e2 [3, 2, 2, 3, 4, 2, 2, 4, 1, 4, 2, 3] [3, 2, 3, 3, 1, 3, 5, 3, 2, 3, 3, 2, 1] [4, 1, 2, 3, 2, 3, 3, 4, 2, 2, 5, 1, 5, 3, 4, 4, 1, 4]
gl....la [2, 2, 4, 5, 3, 3, 1, 4, 2, 2, 4, 3] [2, 2, 2, 4, 3, 4, 3, 3, 2, 2, 1, 4, 2] [3, 4, 2, 3, 3, 4, 2, 3, 4, 3, 4, 5, 4, 4, 5, 2, 3, 4]
ha....oj [5, 3, 4, 2, 3, 1, 3, 3, 3, 1, 2, 1] [2, 3, 2, 2, 5, 3, 3, 3, 3, 3, 3, 2, 3] [4, 2, 4, 5, 3, 3, 2, 2, 1, 3, 3, 2, 4, 2, 3, 3, 2, 3]
ha....o5 [3, 5, 2, 2, 2, 2, 3, 3, 3, 3, 2, 2] [4, 4, 3, 1, 2, 5, 4, 3, 5, 2, 3, 5, 5] [5, 3, 2, 2, 3, 2, 1, 3, 3, 4, 3, 1, 2, 4, 4, 2, 5, 5]
hl....i1 [3, 2, 1, 5, 3, 4, 3, 1, 1, 3, 4, 3] [1, 1, 1, 2, 3, 3, 3, 2, 1, 3, 3, 2, 1] [3, 3, 1, 2, 3, 1, 3, 4, 3, 5, 2, 3, 4, 4, 3, 1, 2, 3]
ch....ar [5, 5, 3, 4, 4, 2, 3, 5, 3, 5, 3, 3] [2, 3, 3, 4, 4, 2, 4, 2, 3, 3, 3, 4, 2] [5, 2, 4, 1, 2, 2, 3, 4, 4, 4, 4, 2, 3, 3, 3, 1, 2, 3]
ji....uz [2, 4, 3, 1, 3, 4, 4, 4, 2, 4, 3, 4] [4, 1, 2, 3, 2, 3, 5, 5, 4, 2, 2, 3, 2] [5, 3, 1, 3, 1, 2, 3, 3, 2, 4, 5, 3, 3, 2, 2, 2, 2, 3]
jo....om [2, 5, 2, 3, 3, 3, 5, 3, 2, 2, 2, 2] [4, 3, 3, 3, 4, 4, 3, 2, 1, 3, 4, 3, 3] [5, 3, 3, 3, 3, 3, 3, 2, 4, 2, 1, 5, 4, 2, 4, 3, 3, 2]
kl....au [3, 5, 2, 1, 2, 3, 1, 2, 1, 4, 3, 3] [4, 2, 3, 1, 4, 1, 4, 3, 3, 2, 1, 3, 3] [2, 4, 3, 3, 3, 2, 3, 3, 2, 5, 5, 4, 5, 2, 3, 3, 2, 3]
ko....a9 [3, 2, 3, 2, 3, 3, 3, 4, 5, 2, 1, 5] [2, 2, 4, 3, 4, 3, 4, 2, 2, 2, 2, 2, 2] [3, 4, 4, 3, 3, 2, 4, 5, 4, 3, 3, 3, 3, 3, 2, 3, 4, 2]
ko....ra [2, 3, 4, 3, 5, 5, 4, 5, 1, 3, 3, 3] [3, 3, 2, 3, 2, 2, 4, 3, 1, 1, 4, 2, 4] [4, 2, 4, 3, 3, 1, 1, 5, 3, 4, 4, 3, 3, 3, 2, 3, 5, 4]
ko....a1 [2, 5, 2, 3, 4, 3, 3, 2, 2, 4, 3, 3] [3, 3, 4, 2, 3, 4, 3, 3, 2, 2, 3, 3, 5] [4, 5, 2, 2, 1, 2, 2, 2, 1, 3, 3, 3, 2, 2, 3, 3, 1, 2]
ko....xa [2, 3, 5, 2, 1, 3, 3, 3, 5, 1, 3, 3] [3, 4, 1, 5, 3, 2, 2, 4, 5, 3, 3, 3, 3] [4, 3, 1, 3, 3, 1, 4, 3, 4, 4, 4, 2, 3, 3, 4, 2, 3, 3]
kr....ar [4, 4, 5, 5, 2, 2, 2, 4, 1, 2, 3, 3] [3, 3, 2, 4, 1, 3, 4, 3, 2, 2, 2, 3, 4] [3, 3, 4, 5, 5, 2, 4, 4, 3, 3, 3, 5, 5, 1, 3, 1, 3, 2]
kr....r5 [3, 2, 2, 2, 3, 2, 5, 2, 3, 3, 2, 2] [3, 4, 2, 4, 3, 2, 1, 3, 3, 3, 3, 4, 4] [2, 1, 2, 2, 3, 3, 3, 4, 3, 2, 2, 3, 2, 3, 3, 3, 3, 3]
ku....i2 [2, 3, 2, 2, 2, 1, 2, 2, 3, 5, 5, 4] [2, 4, 4, 3, 3, 3, 3, 3, 4, 4, 1, 3, 2] [2, 3, 2, 2, 4, 2, 4, 4, 4, 2, 2, 3, 2, 3, 1, 5, 3, 2]
ku....a2 [4, 3, 4, 4, 4, 5, 2, 3, 3, 1, 3, 1] [3, 3, 3, 1, 4, 3, 3, 2, 3, 3, 4, 3, 3] [2, 4, 2, 5, 3, 3, 3, 2, 2, 3, 3, 4, 2, 3, 5, 2, 2, 2]
la....dr [5, 3, 4, 2, 2, 3, 4, 3, 2, 3, 2, 4] [3, 5, 2, 3, 2, 3, 4, 3, 3, 3, 3, 3, 2] [2, 4, 1, 2, 2, 3, 2, 3, 4, 4, 2, 2, 3, 5, 3, 3, 3, 3]
lu....ir [4, 5, 3, 3, 3, 2, 2, 5, 2, 4, 4, 3] [2, 3, 1, 3, 4, 3, 2, 1, 2, 1, 4, 3, 3] [3, 2, 2, 4, 5, 2, 3, 5, 1, 3, 4, 3, 2, 3, 3, 2, 3, 5]
ma....45 [3, 3, 3, 2, 2, 3, 3, 3, 5, 2, 1, 2] [1, 2, 4, 4, 2, 2, 3, 5, 1, 3, 3, 4, 3] [4, 3, 2, 2, 4, 4, 5, 4, 3, 3, 4, 1, 2, 4, 1, 3, 2, 2]
ma....nh [3, 3, 2, 1, 3, 3, 5, 4, 4, 2, 1, 3] [2, 1, 2, 1, 2, 3, 4, 5, 3, 3, 3, 3, 4] [2, 4, 3, 2, 3, 3, 2, 2, 1, 2, 4, 2, 3, 5, 1, 2, 3, 2]
ma...ka1 [3, 2, 4, 2, 2, 3, 2, 1, 1, 1, 3, 3] [5, 3, 1, 5, 5, 1, 3, 4, 1, 4, 3, 4, 3] [4, 4, 3, 3, 2, 3, 5, 4, 2, 5, 5, 4, 4, 4, 2, 3, 1, 1]
ma...va1 [3, 5, 4, 2, 5, 3, 3, 1, 2, 3, 4, 4] [4, 5, 3, 3, 4, 2, 2, 4, 5, 5, 5, 2, 3] [2, 5, 1, 2, 5, 1, 5, 2, 2, 1, 2, 2, 3, 3, 4, 2, 3, 3]
ma....24 [1, 1, 3, 3, 2, 2, 2, 2, 3, 3, 4, 3] [3, 2, 4, 3, 3, 2, 3, 4, 5, 4, 2, 1, 3] [4, 2, 3, 3, 2, 5, 2, 3, 3, 3, 4, 2, 3, 3, 2, 3, 3, 4]
ma....ad [5, 2, 4, 4, 1, 3, 3, 4, 2, 3, 5, 3] [3, 1, 4, 3, 2, 3, 2, 2, 3, 3, 2, 3, 3] [2, 3, 2, 4, 3, 3, 2, 2, 5, 4, 5, 2, 3, 3, 3, 4, 3, 4]
ma....22 [3, 3, 1, 3, 3, 4, 3, 3, 3, 3, 3, 1] [2, 2, 4, 3, 2, 5, 3, 4, 3, 3, 3, 1, 3] [3, 2, 5, 4, 1, 4, 4, 1, 2, 3, 4, 4, 4, 2, 4, 2, 3, 2]
mo....my [2, 3, 3, 5, 3, 5, 3, 2, 4, 3, 3, 3] [2, 3, 3, 1, 3, 3, 3, 2, 3, 2, 2, 5, 3] [4, 5, 4, 2, 3, 3, 2, 2, 3, 5, 4, 2, 3, 2, 3, 4, 3, 1]
mr....ic [3, 4, 2, 5, 4, 2, 2, 2, 5, 2, 1, 4] [4, 2, 3, 2, 5, 4, 4, 2, 3, 4, 2, 4, 2] [2, 2, 5, 1, 2, 2, 3, 2, 2, 2, 1, 2, 2, 2, 3, 1, 5, 2]
ng....a3 [2, 3, 2, 1, 3, 3, 2, 4, 3, 5, 4, 4] [3, 2, 1, 4, 4, 2, 3, 4, 4, 3, 5, 3, 4] [3, 2, 2, 2, 3, 3, 2, 3, 4, 2, 3, 2, 5, 2, 3, 1, 3, 3]
no....ry [4, 3, 3, 1, 2, 4, 3, 4, 3, 5, 4, 3] [2, 3, 3, 4, 3, 4, 4, 1, 3, 3, 4, 3, 4] [3, 4, 5, 2, 2, 3, 3, 3, 1, 3, 2, 5, 1, 1, 2, 5, 4, 2]
on....an [3, 2, 3, 2, 3, 1, 3, 3, 4, 3, 2, 2] [3, 3, 2, 2, 1, 5, 1, 3, 4, 2, 2, 2, 2] [2, 3, 4, 5, 4, 4, 2, 3, 5, 3, 3, 2, 3, 2, 2, 3, 3, 2]
pa....j1 [4, 2, 2, 3, 2, 2, 2, 3, 3, 5, 3, 3] [4, 4, 5, 2, 3, 4, 2, 5, 4, 3, 3, 4, 4] [5, 4, 4, 2, 1, 1, 2, 3, 5, 3, 2, 5, 3, 2, 2, 2, 2, 4]
pa....an [2, 3, 4, 5, 1, 3, 3, 3, 2, 2, 5, 3] [1, 3, 2, 4, 1, 2, 3, 4, 5, 4, 4, 3, 3] [5, 5, 3, 5, 2, 4, 4, 3, 3, 3, 5, 4, 2, 5, 4, 3, 3, 3]
pi....ra [3, 1, 3, 5, 2, 3, 3, 3, 3, 2, 3, 2] [4, 1, 3, 3, 5, 2, 2, 1, 2, 1, 5, 1, 3] [4, 5, 3, 1, 3, 3, 2, 3, 3, 3, 3, 3, 3, 3, 4, 5, 3, 3]
pr....nd [3, 3, 2, 2, 2, 3, 3, 4, 2, 2, 3, 1] [2, 3, 3, 1, 4, 2, 2, 3, 2, 4, 4, 2, 3] [4, 3, 4, 5, 3, 4, 2, 3, 3, 4, 4, 1, 3, 3, 4, 4, 4, 3]
re....ar [4, 3, 3, 3, 3, 3, 3, 1, 2, 3, 2, 3] [2, 2, 1, 3, 3, 3, 5, 3, 3, 5, 3, 4, 2] [3, 3, 3, 2, 2, 4, 1, 2, 5, 3, 2, 4, 3, 2, 2, 3, 1, 5]
ry....av [4, 1, 3, 5, 1, 3, 2, 3, 2, 5, 3, 3] [1, 2, 4, 3, 2, 3, 3, 3, 3, 4, 3, 3, 5] [2, 2, 3, 3, 2, 3, 3, 3, 3, 2, 4, 3, 3, 3, 4, 4, 2, 2]
se....it [1, 4, 2, 3, 5, 3, 2, 2, 2, 4, 1, 3] [1, 1, 3, 1, 2, 4, 1, 4, 1, 5, 3, 2, 1] [2, 4, 2, 5, 2, 3, 3, 4, 4, 3, 3, 3, 1, 3, 4, 3, 2, 5]
sh....ri [5, 4, 2, 2, 5, 4, 3, 2, 2, 3, 3, 4] [5, 3, 3, 4, 3, 2, 2, 3, 3, 3, 4, 4, 4] [2, 2, 4, 5, 4, 3, 2, 3, 4, 3, 3, 2, 3, 3, 1, 5, 3, 2]
so....il [3, 4, 2, 2, 1, 3, 3, 4, 4, 2, 2, 3] [3, 4, 3, 3, 3, 1, 2, 4, 2, 4, 3, 3, 3] [5, 3, 3, 3, 1, 2, 4, 5, 4, 3, 4, 1, 3, 2, 1, 2, 4, 4]
st....16 [3, 1, 3, 3, 4, 4, 3, 1, 2, 4, 4, 4] [2, 2, 2, 1, 2, 2, 2, 3, 3, 3, 3, 4, 3] [4, 3, 2, 2, 3, 1, 4, 5, 3, 2, 4, 1, 4, 3, 5, 3, 2, 5]
st....an [3, 4, 2, 1, 3, 2, 3, 1, 3, 4, 3, 2] [2, 3, 1, 3, 2, 5, 1, 4, 2, 2, 5, 3, 4] [5, 3, 2, 2, 2, 4, 2, 2, 2, 2, 3, 3, 2, 2, 2, 3, 3, 3]
sa....o1 [3, 2, 3, 4, 2, 4, 5, 3, 2, 3, 3, 1] [3, 3, 3, 3, 3, 5, 1, 4, 2, 1, 2, 2, 3] [2, 2, 2, 3, 2, 4, 3, 2, 3, 4, 3, 4, 3, 2, 3, 3, 2, 2]
sv....a1 [1, 4, 5, 3, 1, 2, 2, 3, 4, 3, 1, 2] [3, 4, 4, 3, 4, 2, 3, 4, 4, 3, 2, 3, 5] [2, 4, 2, 4, 2, 5, 2, 4, 3, 3, 2, 3, 2, 4, 3, 3, 4, 2]
to....ik [5, 1, 3, 3, 4, 4, 2, 4, 3, 3, 3, 2] [4, 3, 5, 3, 5, 3, 3, 2, 2, 3, 3, 1, 1] [4, 2, 3, 2, 2, 2, 4, 3, 3, 3, 4, 1, 3, 2, 2, 1, 2, 3]
tr....om [3, 2, 2, 2, 3, 3, 2, 2, 4, 1, 3, 3] [3, 2, 3, 5, 1, 5, 4, 5, 5, 1, 2, 5, 3] [3, 4, 4, 4, 3, 2, 4, 4, 3, 3, 3, 2, 5, 3, 2, 4, 5, 3]
tu....ab [3, 3, 5, 4, 2, 1, 2, 3, 3, 3, 5, 1] [2, 4, 2, 1, 3, 2, 3, 4, 2, 2, 1, 4, 2] [5, 3, 2, 3, 3, 2, 3, 4, 2, 3, 4, 2, 3, 3, 5, 1, 3, 2]
tu....te [3, 2, 2, 2, 3, 5, 2, 3, 3, 2, 5, 3] [2, 2, 3, 3, 4, 3, 3, 5, 3, 1, 3, 2, 2] [3, 2, 4, 5, 2, 2, 5, 1, 2, 1, 5, 3, 3, 2, 3, 3, 3, 4]
va....uz [3, 5, 4, 5, 3, 2, 2, 1, 2, 3, 5, 2] [3, 4, 5, 3, 2, 1, 3, 5, 4, 3, 2, 2, 4] [3, 3, 3, 3, 4, 2, 2, 3, 2, 5, 3, 2, 3, 3, 3, 2, 3, 2]
va....an [2, 3, 3, 3, 2, 5, 4, 3, 2, 3, 5, 2] [4, 3, 2, 2, 3, 2, 3, 3, 1, 3, 3, 5, 3] [2, 3, 2, 1, 4, 2, 2, 3, 1, 5, 2, 4, 2, 4, 4, 4, 3, 4]
ve....o1 [2, 5, 3, 3, 2, 2, 2, 4, 4, 2, 5, 1] [3, 3, 5, 3, 3, 4, 4, 3, 3, 3, 1, 3, 1] [4, 4, 4, 5, 3, 4, 3, 3, 3, 3, 2, 2, 3, 3, 3, 3, 1, 2]
vy....o1 [2, 2, 3, 2, 5, 3, 3, 2, 3, 2, 5, 3] [4, 4, 5, 1, 3, 3, 1, 3, 3, 1, 2, 3, 3] [2, 4, 2, 3, 3, 2, 1, 3, 2, 2, 3, 4, 3, 4, 3, 3, 4, 3]
zv....a4 [5, 5, 2, 4, 3, 3, 1, 4, 3, 4, 4, 4] [1, 4, 2, 2, 2, 3, 3, 2, 2, 2, 3, 3, 5] [1, 3, 3, 3, 5, 3, 4, 1, 1, 3, 4, 2, 4, 3, 3, 3, 2, 1]
```

### Předzpracování

Vyberte si libovolný (podle vás nejednodušší nebo nejzajímavější) z
vašich tří multisetů a připravte vločku z něj vygenerovanou pro tisk
(můžete si zvýšit `$fn`, aby byla vaše vločka hezčí). Můžete s ní dělat
prakticky cokoliv (opravovat, otáčet, krájet, přidávat podpůrné
struktury), ale je třeba zachovat při tisku rozměry a tvar vločky dle
zadaných pravidel a dat. Výstupem je jeden nebo více STL souborů
připravených na slicing a velmi stručný popis toho, **co** jste
udělali a **proč** (ne nutně písemně, ale při odevzdávání je třeba
postup vysvětlit a to i několik týdnů po vykonání vašich změn).

**Jak řezat STL soubory?** Jde to jistě i v OpenSCADu, ale to je zbytečně
komplikované. Připravili jsme proto [krátký návod pro program
MeshMixer](https://github.com/3DprintFIT/BI-3DT/blob/master/cs/meshmixer.md) -
doporučují tři ze čtyř cvičících.

**Nejde vám v Meshmixeru dobře alignovat?** Zkuste program
[Cura](https://www.lulzbot.com/cura). Obsahuje funkci *Lay flat*.

### Slicing

Naslicujte libovolným programem vámi připravená tisková STLka s použitím
vhodných nastavení. Pro Slic3r vyjděte z profilů ze cvičení.
Profily pro případné jiné programy pro vás nemáme,
ale smíte si vytvořit svoje. Výstupem je použitý slicovací profil
vyexportovaný z programu a jeden nebo více GCODE souborů. Jednotlivé
části můžete tisknout najednou (pokud se vejdou na tiskovou plochu a
pokud vám to připadá vhodné) nebo postupně, případě kombinaci obojího.

### Tisk a postprocessing

V zápočtových akcích vypsaných v KOSu, probíhajících ve zkouškovém
období, budete v laboratoři z ABS tisknout vločky z vámi připravených
GCODE souborů. Po dotisknutí je třeba výtisk náležitě opracovat -
oddělat podpory, slepit atp. Výsledná vločka by měla vypadat co
nejpodobněji požadovanému modelu. Na jeden termín je celkem maximálně 5
hodin (tisk + postproccessing).

V případě absolutního selhání při tisku je možné tisk opakovat s novým
GCODEm, ale pouze jednou. V případě technického problému na naší straně
se samozřejmě o promarněný pokus nejedná.

## Odevzdání, hodnocení a termíny

Odevzdává se na GitHub, [založte si repozitář tímto
odkazem](https://classroom.github.com/a/CSWCpIQT).
Veškeré slovní popisy uveďte přímo do README (či README.md apod.) v
repozitáři. **Tentokrát nečekejte žádnou automatickou issue.**

V repozitáři odevzdávejte:

- scad soubor s modulem `flake` jeho deklarací/definicí
- scad soubor **volající** modul `flake` s vašimi vybranými daty (bez deklarace/definice modulu `flake`)
- STL soubor s vaší vločkou, tak jak byl vygenerován OpenSCADem
- Tiskové STL soubory
- Tiskové GCODE soubory
- Profil pro slicovací program, který jste použili
- Případné další potřebné soubory

Termín odevzdání na GitHub je 14.1.2018 včetně (případně začátek
vašeho zápočtového termínu, pokud se tento koná dřív), tisknout můžete i
potom. Možnost pozdního odevzdání: Za každý další započatý týden (byť o
vteřinu) je z celkového hodnocení strženo 10 bodů. Pokud je celkový
součet menší než 0, je hodnocení za semestrální práci 0. V době započetí
termínu klasifikovaného zápočtu (tisk v laboratoři), již musí být
odevzdáno na GitHub.

Zkouškové končí 18.2.2018, nemáme nic proti odevzdání a zápočtovým
termínům i po tomto datu, ale je třeba se na tom explicitně domluvit a
přijmout rizika z toho plynoucí.

Hodnocení dle následující tabulky:

| Část | body | poznámka |
|------|------|----------|
| **Moduly pro OpenSCAD:** | **10** | |
| Modul `flake` funguje podle zadání | 7 | povinný v rámci části |
| Zdrojový kód je vhodně členěn a komentován | 3 | |
| **Příprava na tisk:** | **10** | |
| Vhodně připravená tisková STLka | 5 | povinný v rámci části |
| Mesh ve všech tiskových STL je v pořádku | 5 | |
| **Slicing:** | **10** | |
| Podpory (nejsou potřeba (5 b.), vhodné užití* (2.5 b.), zbytečné užití (0 b.)) | 5 | |
| Vhodné nastavení parametrů tisku (perimetry, výplň, výška vrstvy) | 5 | |
| **Tisk:** | **10** | |
| Jedná se o výtisk modelu dle zadání, výtisk je opracovaný (např. bez podpor, slepený atp.) | 4 | povinný v rámci části |
| Výtisk neobsahuje vady zjevně způsobené nevhodnou přípravou modelu | 3 | |
| Výtisk neobsahuje vady zjevně způsobené nevhodnou přípravou tiskárny (příprava tiskové plochy, nevhodné teploty) | 3 | |

\* Pouze za podpory vygenerované při slicování se strhávají body. Protože jsme v části slicing.

*Pro ovládání tiskárny při odevzdávání potřebujete vlastní počítač se
schopností připojit se na WiFi nebo kabelem do lokální sítě. Také
potřebuje znát (umět dohledat) svou MAC adresu.*


Hodnocení je rozděleno na 4 dílčí části. *Povinný v rámci části*
znamená, že bez splnění tohoto úkolu student za danou část nedostane
žádné body. V případě opravného tisku se již neopravují hodnoty bodů v
ostatních dílčích částech. Pokud tedy například nezvládnete slicing,
dostanete z něj nula bodů a (celkem logicky) fatálně selže i tisk,
můžete v náhradním termínu dostat body za tisk, za slicing už ale žádné
body nedostanete.
