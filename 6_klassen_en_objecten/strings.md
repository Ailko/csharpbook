We gebruiken het einde van klassen en objecten om iets dieper in de ``String`` klasse te duiken en aan te tonen dat er tal van nuttige zaken bestaan om met strings te werken.

## Verbatim character

Door een ``@`` (verbatim character) voor een string te plaatsen zeggen we concreet: "de hele string die nu volgt moet je beschouwen zoals hij er staat. Je mag alle escape karakters negeren.

Dit wordt vaak gebruikt om een filepath iets leesbaarder te maken.

* Zonder verbatim: ``string path= "c:\\Temp\\myfile.txt";``
* Met verbatim: ``string path= @"c:\Temp\myfile.txt";``

## Splitsen en samenvoegen

### Split

Aan het einde van dit hoofdstuk willen we een csv-bestand (comma separated value) splitsen. De ``Split`` methode laat toe een string te splitsen op een bepaald teken. Het resultaat is steeds een **array van strings**.

```csharp
string data= "12,13,20";
string[] gesplitst= data.Split(',');

for(int i=0; i<gesplitst.Length;i++)
{
    Console.WriteLine(gesplitst[i]);
}
```

Uiteraard kan je dit dus gebruiken om op eender welk ``char`` te splitsen en dus nit enkel een ``','`` (komma).

### Join

Via ``Join`` kunnen we array van string terug samenvoegen. Het resultaat is een nieuwe string.

Volgende voorbeeld zal de eerder array van het vorige voorbeeld opnieuw samenvoegen maar nu met telkens een ``;`` tussen iedere string:

```csharp
string joined = String.Join(";", gesplitst);
```

Voorgaande methoden zijn ``static`` en moet je dus via de klasse ``String`` doen en niet via de objecten zelf.

## Andere nuttige methoden


Volgende methoden kan je rechtstreeks op de string-objecten aanroepen (i.e. niet ``static`` methoden)

* ``int Length``: geeft totaal aantal karakters in de string
* ``IndexOf(string para)``: geeft ``int`` terug die de index bevat waar de string die je als parameter meegaf begint
* ``Trim()``: verwijdersd alle onnodige spaties vooraan en achteraan de string en geeft de opgekuiste string terug
* ``ToUpper()``: zal de meegegeven string naar ALLCAPS omzetten en geeft de nieuwe string als resultaat terug
* ``ToLower()``: idem maar dan naar kleine letters
* ``Replace(string old, string news)``: zal in de string alle substring die gelijk zijn aan ``old`` vervangen door de meegegeven ``news`` string

## Arrays vergelijken

De correcte manier om strings te vergelijken is met de ``Compare(string s1, string s2)`` methode. Deze zal een ``int terug geven:

* -1 : de string ``s1`` komt voor de string``s2`` indien je ze alfabetisch zou sorteren.
* 0: beide strings zijn identiek
* 1: de string ``s2`` komt voor ``s1``

# Tekst files uitlezen

Via ``System.File.ReadAllLines()`` kunnen we een tekstbestand uitlezen. De methode geeft een array van string terug. Per lijn die eindigt met een newline (``\r\n``) zal een nieuwe string aan de array toegevoegd worden.

```csharp
string[] lines = File.ReadAllLines(@"c:\mypoem.txt");
for (int i = 0; i < lines.Length; i++)
{
    Console.WriteLine(lines[i]);
}
```

## CSV uitlezen

Dankzij ``ReadAllLines`` en ``Split`` hebben we nu alle bouwstenen om eenvoudig een csv-bestand te verwerken.

Stel je voor dat een bestand ``soccer.csv`` volgende tekst bevat:

```text
Dams;Tim;1981
Hamdaoui;Mounir;1984
Stoffels;José;1950
```

Volgende code zal dit bestand uitlezen en de individuele data op het scherm tonen:

```csharp
string[] lines = File.ReadAllLines(@"c:\soccerstars.csv");
for (int i = 0; i < lines.Length; i++)
{
    string[] splitted = lines[i].Split(';');

    Console.WriteLine($"Voornaam speler {i}= {splitted[1]}" );
    Console.WriteLine($"Achternaam speler {i}= {splitted[0]}");
    Console.WriteLine($"Geboortejaar speler {i}= {splitted[2]}");
}
```