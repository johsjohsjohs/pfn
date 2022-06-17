# pfn
python faq for norsk dette er tatt fra https://www.diskusjon.no/topic/592738-vi-lager-en-python-faq/.

Jeg tror det er på tide at vi begynner arbeidet med å lage en Python FAQ. Jeg tror vi kan ta det litt hulter til bulter i første omgang, og heller systematisere/kategorisere senere og få gjort den sticky, så den alltid ligger øverst i emnelista. Håper så mange som mulig bidrar med rettelser/forbedringer/omskrivninger, flere spørsmål og ev. svar under.

Det kuleste ville ha vært om vi kunne bruke SPOILER-taggen i BB Code, men det ser ikke ut til at det går an å endre klikketeksten. Noen som vet?

Alternativt kan vi hoste FAQ'en et annet sted, kanskje som wiki og linke til den i forumet. Si i fra hvis noen kan ordne dette. Python.no ville ha vært ønskedrømmen. Domenet er registrert, men ikke aktivt:

[http://www.norid.no/domenenavnbaser/whois/?query=python.no](http://www.norid.no/domenenavnbaser/whois/?query=python.no)

Norsk Python FAQ (en begynnelse)

**Q**

#### **Hvordan unngå "DeprecationWarning: Non-ASCII character" når jeg bruker norske tegn i scriptet mitt?**

**A**

For å unngå dette, legg inn


    # -*- coding: UTF-8 -*-




eller andre encodings i en av de første to linjene i scriptet.

Oversikt over alle encodings:

[http://docs.python.org/lib/standard-encodings.html](http://docs.python.org/lib/standard-encodings.html)

---------

**Q**

#### **Hvordan slippe at kommando-vinduet vises når jeg dobbelt-klikker på scriptet i Windows?**

**A**

Kall filen .pyw i stedet for .py

---------

**Q**

#### **Hva er** **_List Comprehensions (listcomps)_****?**

**A**

List comprehensions (LC) er en måte å jobbe med og lage lister i Python. Konstruksjonen er hentet fra språket Haskell. LC gir ofte korte, kraftige kodesekvenser som er enkle å lese og gir ryddig kode.

LC erstatter gjerne vanlige for-løkker. Eksempel:

# klassisk for-løkke: Ny liste med store forbokstaver
navne_liste = ['kjersti klar', 'rupert rar', 'selmer svanger', 'lise salat']
kapiteler = []

```python
for navn in navne_liste:
   kapiteler.append(navn.title())

print kapiteler
>>> ['Kjersti Klar', 'Rupert Rar', 'Selmer Svanger', 'Lise Salat']
```

# Som over, men med LC
```python
navne_liste = ['kjersti klar', 'rupert rar', 'selmer svanger', 'lise salat']
kapiteler = [s.title() for s in navne_liste]
print kapiteler
>>> ['Kjersti Klar', 'Rupert Rar', 'Selmer Svanger', 'Lise Salat']
```


LC egner seg ikke hvis løkkene er kompliserte med mange vilkår. De blir vanskelige å skrive, og vanskelige å lese. Men de kan godt være mer omfattende enn eksempelet over. Et lite eksempel til:

# ta en liste med tall og lag en ny med tall som er ikke er "bad"
# tallene som strenger, ikke integere
```python
bad = (4, 8, 12, 13, 20, 27, 29) # noen "bad numbers"
numbers = range(35)
ok_numbers = [str(n) for n in numbers if n not in bad]
```


(Flere treffende eksempler ønskes)

I neste versjon av Python, 2.5 som kommer høsten 2006, er det bygget inn en versjon av ternary-konstruksjonen som finnes f.eks i C og PHP.

Den åpner for korte, konsise uttrykk:

# vanlig måte
```python
if condition:
   x = true_value
else:
   x = false_value
```
# ny mulig måte:
```python
x = true_value if condition else false_value
```


Dette vil kunne brukes i LC og gjøre filtreringsmulighetene ennå kraftigere!

---------

**Q**

#### **Hva er en** **_iterator_****?**

**A**

I engelske tekster om Python møter man tidsnok på ordene _iterator_ og _iterable_. Dette er kjernebegreper i Python - viktige å forstå, og viktige å vite forskjellen på.

Først _iterable_. En _iterable_ i Python er alle datatyper/datastrukturer som det kan itereres, dvs. 'loopes' over.

Noen eksempler er:

-   En streng: "abcdef"
-   En liste: [1,2,3,4,5]
-   En tuple: (44, 55, 66)
-   En dictionary: {1 : 'en', 2 : 'to', 3 : 'tre'}
-   En fil
-   Et objekt som spesifiserer "den magiske metoden" __iter__()
-   ... og helt konkret: en _iterator_

Typisk looper man over en _iterable_ i en for-løkke. Mange funksjoner krever en _iterable_ som argument, f.eks map().

En _iterator_ er et spesialobjekt som har en metode, nemlig next(). Den er spesialinnrettet for å iterere over en bestemt datastruktur, f.eks en liste, eller en fil.

Du kan lage en iterator ved å bruke den innebygde funksjonen _iter()_:
```python
en_liste = range(10)
en_iterator = iter(er_liste)
```
![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "Click and drag to move")

I nyere Python brukes de ofte, og Python 3.0 (aka Python 3000), vil mange funksjoner returnere iteratorer i stedet for lister, eller tupler som i dag.

Iteratorer brukes som regel indirekte:
```python
tekstfil = open('poengsum.txt', 'r')
for linje in tekstfil:
   print "Poeng:", linje
```
![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "Click and drag to move")

Jeg sier "indirekte", for her er det ingen kall til next()... Det skjer "under panseret", og det er en viktig del av iteratorprotokollen. Vi looper over dem uten egentlig å oppleve at det ikke er datastrukturen vi jobber direkte på.

Det samme kan gjøres eksplisitt:
```python
tekstfil = open('poengsum.txt', 'r')
try:
   while True:
       print "Poeng:", tekstfil.next()
except StopIteration:
   # Iterator er tom, dvs. siste linje nådd
   pass
```
![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "Click and drag to move")

#### **Tre poenger om iteratorer**:

1. En iterator er "ikke-destruktiv", dvs. man kan ikke endre datastrukturen som den looper over via iteratoren selv. De kan derfor brukes til å gjøre data "read only". En annen måte å si dette på er: "En iterator er "enveis", data kan hentes ut, men ikke sendes inn.

2. En iterator har bevissthet - dvs, den vet og husker hvor i sekvensen den sist var. Du behøver ikke å holde rede på en indeks.

# åpne tekstfil med 4 linjer, med tall 0,1,2,3
```python
tekstfil = open('poengsum.txt', 'r')

for line in range(2):
   print tekstfil.next()

>>> '0\n'
>>> '1\n'
```
# masse annen kode
```python
for line in range(2):
   print tekstfil.next()

>>> '2\n'
>>> '3\n'
```
![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "Click and drag to move")

3. En iterator er ofte minne-effektiv. Et typisk eksempel her er svære filer. La oss si du har en log-fil på 1GB. Kode a la:
```python
log_data = open('log.txt', 'r').read()
```
![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "Click and drag to move")

vil spise opp minnet. Ved å loope over filobjektet via den innebygde iteratoren vil du aldri bruke mer minne enn hver enkelt linje.

---------

**Q**

#### **Hva er dette if __name__ == '__main__' greiene?**

**A**

I script du finner på nettet osv., vil du ofte finne kode som dette nederst i scriptet:
```python
def main():
   # diverse

if __name__ == '__main__':
   main()
   # og ofte: sys.exit(0)
```
![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "Click and drag to move")

Dette er en uoffisiell standard i Python-miljøet. Hensikten er enkel: Dersom scriptet kjøres alene, _utfør main()_. Hvis det importeres som en modul i et annet script: _Ikke utfør main_.

___name___ er en built-in variabel. Den rommer enten strengen "__main__", eller filnavnet på modulen som importeres (uten fil-ekstensjon). Alt avhengig av hvor den kalles fra.

Mer konkret:

Vi lager en fil som heter _parse_html.py_. I den har vi kun følgende kode:
```python
print __name__
```

![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "Click and drag to move")

Kjører vi scriptet fra prompt, blir utdata:
```python
>>> __main__
```

Lager vi en annet script som heter _parser.py_

og legger inn denne koden:
```python
import parse_html
```
![](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw== "Click and drag to move")

så vil
```python
>>> parse_html
```
bli printet ut. ___name___ inneholder nå navnet på modulen, ikke "__main__"

Det er ingen ulemper med å bruke denne metoden, så det anbefales. Da kan du importere funksjoner eller klasser fra et script uten at kode eksekveres.

Det er også vanlig å legge til sys.exit(0) som siste statement. Det betyr at returverdien fra scriptet er 0, dvs "alt vel". Det kan være nyttig i andre sammenhenger og forøvrig en standard, spesielt i *UX-sammenheng.

---------

**Q**

#### **Hvorfor går det så tregt å jobbe med store tekstblokker?**

**A**

Python er ikke spesielt tregt, men alle verktøy virker dårlig om de brukes feil.

Datatypen _string_ i Python er _immutable_, dvs. den kan ikke endres. Dette får konsekvenser for det som kalles "string concatenation", det å sette samme flere strenger til en.

Jobber man med små mengder tekst, er det helt greit å skrive f.eks:

# Setter sammen liten HTML tabell
```python
navneliste = [('Petter', 'Moss'), ('Anna', 'Grimstad')] # + ti navn til
html = '<table border="1">'
for navn, bosted in navneliste:
   table_row = '<tr><td>' + navn + '</td><td>' + bosted + '</td></tr>\n'
   html = html + table_row

html = html + '</table>\n'
```

Men er det store tekstmengder, kan gir dette ytelsesproblemer. Det er bedre å bruke å bruke lister og streng-interpolasjon (%), og heller sette sammen teksten til slutt med streng-metoden join()

# Setter sammen liten HTML tabell
```python
navneliste = [('Petter', 'Moss'), ('Anna', 'Grimstad')] # + ti navn til
html = ['<table border="1">']  # liste for å mellomlagre tekstblokkene

for navn, bosted in navneliste:
   html.append('<tr><td>%s</td><td>%s</td></tr>\n' % (navn, bosted))

html.append('</table>')
html_streng = "\n".join(html)  # sett sammen listelemente, skill med linjeskift
```


Med en liste med 100.000 navn/bosted par gir disse eksemplene en ytelse på hhv. 4.85 og 0.15 sekunder på min maskin. Spesielt hvis tekstblokkene er store kan konkatinering med + gi svært ineffektiv kode. Ytelsesforebdringer på over 3000% er verdt å få med seg ;-)

---------

**Q**

#### **Hvordan fjerne like elementer i en liste?**

**A**

Svaret på dette avhenger av om det er viktig å bevare rekkefølgen på elementene.

Hvis rekkefølgen er likegyldig, er det to _idiomer_ som er typiske:
```python
liste = [1,1,2,3,3,3,4,3,1,2,5,3,4,5,1]
```
# klassisk metode, gjør om til dictionary, siden en dict key må være unik:
```python
tmp_dict = {}
for el in liste:
  tmp_dict[el] = 1  # verdien spiller inge rolle

unik_liste = tmp_dict.keys()
```
# i nyere Python (2.3 >)m bruk set()
```python
unik_liste = list(set(liste))
```


Dict/set-metoden krever at elementet i listen er _hashable_. Strenger, tall, tupler er det, men ikke lister. Så metoden vil ikke virke dersom elementene er lister. En mulig work-around er å konvertere listene til tupler med tuple() funksjonen:
```python
tmp_dict = {}
for el in liste:
  tmp_dict[tuple(el)] = 1  # verdien spiller inge rolle

unik_liste = tmp_dict.keys()
```
# med set()
```python
unik_liste = list(set([tuple(el) for el in liste]))
```


Bruk hash(objekt) for å sjekke om et bestemt objekt er hashbart.

For eksempler og diskusjon om dette, se Python Cookbook med bidrag fra Python-kanoner som Martelli, Peters og Hettinger:

[http://aspn.activestate.com/ASPN/Cookbook/...on/Recipe/52560](http://aspn.activestate.com/ASPN/Cookbook/Python/Recipe/52560)

---------
**Q**

#### **Hvordan få pythons interaktive tolk til å starte ved å skrive python i cmd?**

**A**

Python har en interaktiv tolk som er kjekk å ha når man vil teste små kode snutter eller

vil leke litt med python og finne ut hvordan ting funker.

For å få starte tolken så åpner man cmd (windows) og skriver python, den skal da starte, men hvis man

bruker windows så skjer ikke det automatisk når man installerer python. For å kunne starte tolken

ved å bare skrive python, så må man gjøre noen små endringer.

Først så høyreklikker man på Min Datamaskin og velger egenskaper. Eller man kan gå inn på kontrollpanelet,

Velge "Ytelse og vedlikehold" og klikke på "System"

Det vil da komme opp et vindu hvor det står en del info om datamaskinen. Trykk på fanen som heter "Avansert".

Nederst på den sida er det en knapp som heter "Miljøvariabler", trykk på den.

Det vil da komme opp et vindu med to lister, en for brukervariabler og en for systemvariabler.

Hvis man vil at endringen skal gjelde for hele systemet, velger man systemvariabler, eller hvis

man vil at det kun skal gjelde for en bruker velger man brukervariabler.

Finn variabelen som heter "path" eller "PATH", trykk på den og velg rediger.

I vinduet som kommer opp, der det står "variabelverdi", skal man legge til mappa man installerte python i.

F.eks, hvis du installerte python i C:\python24, så legger man til dette i variabelverdien: ;C:\python24

Jeg installerte python i C:\Programfiler\python24 så derfor legger jeg til ;C:\Programfiler\python24

Trykk ok, og trykk ok på de andre vinduene også.

Du kan nå prøve å starte cmd: start>kjør>cmd

Prøv å skriv, python

Tolken skal da starte og det skal vise noe lignende som dette:

```python

Python 2.4.2 (#67, Sep 28 2005, 12:41:11) [MSC v.1310 32 bit (Intel)] on win32

Type "help", "copyright", "credits" or "license" for more information.

>>>

```
-----------
**Q**

#### **Hva er et** **_Python egg_** **og hvordan bruker jeg det?**

**A**

Et _Python egg_ tilsvarer Javas JAR og Rubys Gems. Dvs, en måte å distribuere/installere/kjøre tilleggspakker. Python eggs er ganske nytt, men brukes stadig mer. Konseptet minner mye om RPM og DEB på Linux.

Bruk _Easy Install_ for å laste ned/installere/oppdatere egg.

Se:

[http://peak.telecommunity.com/DevCenter/PythonEggs](http://peak.telecommunity.com/DevCenter/PythonEggs)

[http://peak.telecommunity.com/DevCenter/EasyInstall](http://peak.telecommunity.com/DevCenter/EasyInstall)

---------

**Q**

#### **Kan jeg lage en EXE-fil av scriptet mitt?**

**A**

Ja, ved hjelp av Py2Exe.

Se: [http://www.py2exe.org/](http://www.py2exe.org/)

Freeze gir tilsvarende funksjonalitet på *UX

---------
**Q**

#### **Hvorfor blir 2/3 0?**

**A**

Python har arvet denne oppførselen fra C, den kalles "klassisk divisjon". To dividerte heltall (integers) gir et heltall som resultat. Men det finnes utveier, her en noen eksempler fra interaktiv modus:
```python
>>> 2 / 3
0
>>> 2 / 3.0
0.66666666666666663
>>> 2 // 3.0
0.0
>>> from __future__ import division
>>> 2 / 3
0.66666666666666663
```
Som vist i eksemplet, er en måte å eksplitt dividere med et flyttall. Bruk _float(heltall)_ for å typecaste fra int til float. Dersom du ikke ønsker alle desimalene, kan du bruke floor-operatoren //. Alternativt kan du bruke math.floor for å redusere antall desimaler.

I klasser kan du definere metodene ___truediv___ ___floordiv___ for å overloade / og //.

I Python 3.0 vil denne oppførselen bli endret. Da vil 2 / 3 være lik 0.66666666666666663. Ved å importere division fra fra __future__, skriver du Python 3.0-kompatibel kode.
--------------------------------
**Q**

#### **Hva er Python 3000 /Py3K?**

**A**

Python 3000 er en fremtidig versjon av Python, 3.x. Den er spesiell fordi den vil innebære et brudd med prinsippet om bakoverkompatibilitet. Builin-ins vil bli fjernet eller flyttet til biblioteker, returverdier vil få andre typer og enkelte andre grunnleggende ting i språket vil endres. Python 3000 er en lenge planlagt revisjon av språket. Diskusjonen har foregått i minst seks år og releasen ligger fortsatt flere år frem i tid. Det jobbes for at overgangen skal bli minst mulig.

Se mer om dette her:

[http://www.python.org/dev/peps/pep-3000/](http://www.python.org/dev/peps/pep-3000/)

[http://en.wikipedia.org/wiki/Python_3000](http://en.wikipedia.org/wiki/Python_3000)

**Q**

#### **Hva er Parrot?**

**A**

Parrot var i utgangspunktet en aprilspøk fra skaperne av Perl (Larry Wall) og Python (Guido van Rossum) i 2001. Men spøken er i ferd med å bli virkelighet: Parrot er navnet på interpreteren (den virtuelle maskinen) i v6.0 av Perl. Den skal være i stand til å kjøre kode fra flere dynamiske språk, bl.a. Python, Ruby, Tcl, v5 av Perl, Scheme og PHP, enten direkte eller via Parrot assembly language/Parrot Intermediate Representation. Parrot skal være ekstremt rask.

Mange har store forhåpninger til at Parrot skal forene alle Open Source-språkene under en felles platform. "One bytecode to rule them all". Arbeidet har pågått siden 2001 og det er fremdeles langt igjen.

Se

[http://www.python.org/parrot.html](http://www.python.org/parrot.html) (1.april)

[http://www.parrotcode.org/](http://www.parrotcode.org/)

[http://en.wikipedia.org/wiki/Parrot_virtual_machine](http://en.wikipedia.org/wiki/Parrot_virtual_machine)

**Q**

#### **Hva er PyPy?**

**A**

PyPy er et EU-støttet prosjekt som bl.a tar sikte på å lage en implementasjon av Python _skrevet i Python_. I teamet finnes størrelser som Christian Tismer og Armin Rigo, navnene bak _Stackless Python_ og _Psyco_.

En av de mange effektene av PyPy er at Python kode skal kunne oversettes til andre språk, til og med så forskjellige som C og Jacascript. Tismer annonserte nylig (mars 2006) at han har lykkes med å bruke PyPy til å kompilere Python kode til en binær Python modul(!).

PyPy virker veldig lovende og gjør stadige fremskritt.

Se

[http://codespeak.net/pypy/](http://codespeak.net/pypy/) (teknisk heavy)

[http://codespeak.net/pipermail/pypy-dev/2006q1/002911.html](http://codespeak.net/pipermail/pypy-dev/2006q1/002911.html) (Tismers jubelmelding)
---------------------------
**Q**

####  **Hvordan lage en array i Python?**

**A**

Dette kan man gjøre ved hjelp av lister.

Lister er nesten det samme som arrays i C og Pascal, men lister kan inneholde forskjellige typer.

Du kan bruke array modulen til å lage arrays på andre måter, men det går saktere.

Eksempel på liste:
```python
["dette", 6, "er", "en", "array", "eller", "liste"]
```
------------------
**Q**

#### **Finnes det et** **_switch-statement_** **i Python?**

**A**

Mange språk har kontrollstrukturen _switch_ (aka _case_). I PHP f.eks kan man gjøre noe slikt:

```php
<?php
// Eksempel fra php.net
switch ($i) {
   case "apple":
      echo "i is apple";
      break;
   case "bar":
      echo "i is bar";
      break;
   case "cake":
      echo "i is cake";
      break;
   default:
      echo "i is something else";
}
?>
```

Python har ikke denne konstruksjonen. Det er to måter å oppnå tilsvarende funksjonalitet:

# 1 Bruk if-elif-else  (legg til break hvis nødvendig)
```python
if i == 'apple':
   print 'i is apple'
elif i == 'bar':
   print 'i is bar'
elif i == 'cake':
   print 'i is bar"'
else:
   print 'i is something else'
```

# 2 Bruk en dictionary!
```python
cases = {'apple' : 'i is apple', 'bar' : 'i is bar',
          'cake' : 'i is cake', 'default' : 'i is something else'}

if i in cases:
   print cases[i]
else:
   print cases['default']
```

Man vil kanskje tenke "ok, men hva hvis jeg ikke skal printe noe, men sende _i_ til en funksjon..." Siden funksjoner i Python er _first-class objects_ kan de være verdier i en dictionary:

# et sett av funksjoner som håndterer ulike case
```python
def handle_apple_case(i):
   # process(i)
   pass

def handle_bar_case(i):
   # process(i)
   pass

def handle_cake_case(i):
   # process(i)
   pass

def handle_default_case(i):
   # process(i)
   pass
```
# funksjonsnavn som value i dictionary'en
```python
case_to_dict = {'apple' : handle_apple_case,
               'bar' : handle_bar_case,
               'cake' : handle_cake_case,
               'default' : handle_default_case
               }
```

# selve case sjekken
```python
if i in case_to_dict:
   case_to_dict[i](i)
else:
   case_to_dict['default'](i)
```

Vi legger altså funksjonen i en dictionary og gjør et vanlig dictionary-oppslag med caset som argument. Det finnes varianter av denne måten. Ved bruk av _lambda-konstruksjoner_ som verdi i dictionary'en kan den gjøres kortfattet og kraftig.

Som oftest gjør if/elif/else jobben.
------------------------------------
Q

#### **Støtter Python rekursiv programmering?**



A

Ja. Maksimalt antall nivåer er 1000. Dette kan endres med sys.setrecursionlimit(limit)



Rekursiv programmering er en teknikk der man skriver en funksjon som kaller seg selv inntil den når et avslutningsvilkår. Dette gir ofte svært kompakt kode, men gjør det også nødvendig å holde tunga rett i munnen for å unngå uendelige løkker og/eller minneslukere. Rekursjon er ofte effektiv ifm. lister og sekvenser (som det jo vrimler av i Python)



Her er et enkelt eksempel hentet fra Kap. 4. i How to Think Like a Computer Scientist:
```python
def countdown(n):
   if n == 0:
       print "Blastoff!"
   else:
       print n
       countdown(n-1)  # <-
```




Her er et annet eksempel som skriver ut alle mulige kombinasjoner av en bokstavrekke:
```python
def print_out(combination):
   print ''.join(combination)

def    make_combinations(str_list, length, wrk_list=[]):
   if not length:
       return print_out(wrk_list)
   for i in range(len(str_list)):
       wrk_list.append(str_list.pop(i))
       make_combinations(str_list, (length - 1), wrk_list)
       str_list.insert(i, wrk_list.pop())

a_string = 'abcd'
make_combinations(list(a_string), len(a_string))
```


Det er sjelden nødvendig å velge en rekursiv algoritme, men noen ganger føles det mest naturlig. Hvis du er er sto fan av rekursjon, bør du sjekke ut Stackless Python som er en versjon av Python som løsrevet fra C-stakken og med støtte for mikrotråder.

http://www.stackless.com/
-------------------------
**Q**

#### **Hva er** **_duck typing?_**

**A**

_Duck-typing_ er et begrep som stammer fra Ruby-verdenen og referer til den kjente Duck testen: _If it walks like a duck and quacks like a duck, it must be a duck._.

Innen programmering betyr dette at selve datatypen ikke er det avgjørende, men hvordan den oppfører seg.

I Python er dette en dypt innbakt prinsipp og det er viktig å bruke det, spesielt når man skriver kode andre skal bygge videre på.

Kron-eksempelet på duck typing er det såkalte _file-like object_. Et file-object kan f.eks lages på denne måten:
```python
file_object = file('a_text_file.txt', 'r')
```
# og vi kan undersøke hvilke metoder objektet
# har ved å bruke dir(obj)
```python
print dir(fileobject)

... som forkortet skriver ut en liste med bl.a. disse metodenavnene:

['close', 'next', 'read', 'readline', 'write']

Filobjektet støtter mao. statements som _file_object.read()_, _file_object.close()_

Vi kan for ordens skyld sjekke hvilke datatype _file_object_ har:

file_object = file('a_text_file.txt', 'r')
```
# skriv ut datatypen
```python
print type(file_object)
```
# sjekk om objektet er en ekte instans av klassen file:
```python
print isinstance(file_object, file)
```

Output her blir _<type 'file'>_ og _True_. La oss sammeligne dette med et et objet som _ikke_ er en fil:
```python
import urllib

file_like_object = urllib.urlopen('http://www.python.org')
print dir(file_like_object)
```
... som skriver ut en liste med bl.a. disse metodenavnene:
```python
['close', 'geturl', 'headers', 'info', 'next', 'read', 'readline', 'url']
```
Vi kjenner igjen typiske fil-metoder som _read()_ og _close()_, men ser at det også er fremmede metoder som f.eks _geturl()_.

Og for ordens skyld: Dreier det seg om en fil?
```python
import urllib

file_like_object = urllib.urlopen('http://www.python.org')

print type(file_like_object)
print isinstance(file_like_object, file)
```
... Nei! Her blir output _<type 'instance'>_ og _False_. Det store poenget er imidlertid at _file_like_object_ (som er en socket - et nettverksobjekt) _oppfører seg som en fil_, i hvert fall til en viss grad.

Så: For en funksjon som krever et fil-objekt som argument, er det ikke viktig om det faktisk er en ekte fil som sendes inn. Det som er viktig, er om argumentet passerer Duck testen: _If it read() like a file and close() like a file, it must be a file..._

Dette er essensen i begrepet _duck typing_. Liker man faglige uttrykk, så er navnet på dette i objekt-orientert terminologi _signaturbasert polymorfisme_. En mindre høytsvevende måte å si det på er at to objekter deler grensesnitt.

En viktig konsekvens av dette er at du ofte bør unngå å sjekke datatypen, eller hvilke klasse et objekt er en instans av. Du bør å istedet sjekke hvilke metoder objektet har.

For å bruke omigjen eksemplene ovenfor: Sett at du har en liten funksjon som trekker ut alle ord av en fil, fjerner skilletegn osv, fjerner like ord og returnerer en sortert liste med ord. Da vil du kanskje være fristet til å legge inn en sjekk først. Hvis det ikke er en fil, returner False. Noe slikt:
```python
def extract_words(file_object):

   # sjekk om file_object er en fil
   if type(file_object) != file:
       return False
   if isinstance(file_object, file) != file
       return False

   # OK! Vi har en ekte fil, go on
   # read, split, replace osv
```

Ikke gjør det på den måten... Denne funksjonen vil forkaste all HTML'en vi har tilgang på via socket'en fra _urllib.urlopen()_ og alle andre fil-like objekter. Sjekk heller om objektet støtter _read()_. Bruk hasattr(objekt, 'metode') for å se om _attributtet_ er der:
```python
def extract_words(file_object):

   # sjekk om file_object er et fil-likt objekt
   if not hasattr(file_object, 'read'):
       return False

   # OK! Vi kan lese fra dette objektet, go on
   # read, split, replace osv
```
Du kan også følge en annen sti innen Python Zen: _Easier to Ask Forgiveness than Permission_ (EAFP):
```python
def extract_words(file_object):

   try:
       text_data =  file_object.read()

       # OK! Vi kan lese fra dette objektet, go on
       # split, replace osv

   except AttributeError:
       # Oooops... støtter ikke read()
       return False
```
Tilsvarende blir det for mange andre objekter. Lager du en funksjon som krever en sekvens du kan loope over, sjekk om den er _iterable_, ikke om det er en liste osv. Let etter egenskaper, ikke datatyper. Da blir koden din ved et trylleslag langt mer anvendelig.

Skriver du klasser kan du enkelt gjøre dem til _file-like_ ved å legge til filmetodene du behøver, f.eks _read()_. Ditto for andre objekter. På den måten kan du også tvinge andres kode til å akseptere ditt objekt.

Mer om Python Duck typing:

[http://www.voidspace.org.uk/python/article...ck_typing.shtml](http://www.voidspace.org.uk/python/articles/duck_typing.shtml)

Om Ruby Duck typing:

[http://wiki.rubygarden.org/Ruby/page/show/DuckTyping](http://wiki.rubygarden.org/Ruby/page/show/DuckTyping)
------------------------
**Q**

#### **Hva er en metode?**

**A**

Metoder blir ofte definert som en funksjon innen en klasse definisjon. Metoder blir ofte kalt på denne måten: class.method(arguments)..
```python
Eksempel:

class A:

def method(self, x):
           return x
```

**Q**

#### **Er det noen kodings standarder eller stiler du må følge når du bruker python?**

**A**

Ja. Det er en programmeringsstil som er dokumentert her [http://www.python.org/dev/peps/pep-0008/](http://www.python.org/dev/peps/pep-0008/) .

**Q**

#### **Hvordan gjør jeg om en string til et nummer?**

**A**

Hvis du vil gjøre om til en integer kan du bruke int funksjonen, eller float funksjonen for desimaler.

Eksempel:
```python
int(’231’)
a = ‘789’
int(a)

f = ‘234’
float(f)
```

**Q**

#### **Hvordan gjøre om et nummer til en string?**

**A**

Tilsvarende som over, bare at du bruker funksjonen str istedenfor float eller int.
```python
str(23)
a = 56
str(a)
```
---------------------
