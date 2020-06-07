# Polymorfisme

**Polymorfisme** oftewel "meerdere vormen" is een programmeertechniek waarbij je code schrijft die door objecten van verschillende klasse, kan gebruikt worden. Hierdoor kan je dus compactere code schrijven en hoef je niet voor iedere klasse apart (deel)code te schrijven. Samen met encapsulatie en overerving is polymorfisme een derde belangrijke eigenschap van object georiënteerd programmeren.

We tonen het nut van polymorfisme aan de hand van drie voorbeelden:


## Polymorfisme in de praktijk: Dieren
Een voorbeeld maakt veel duidelijk. Stel dat we een een aantal Dier-gerelateerde klassen hebben die allemaal op hun eigen manier een geluid voortbrengen. We hanteren de klasse dier uit een eerder hoofdstuk ([abstracte klassen in overerving](../13_advancedovererving/5_abstract.md)):
```csharp
abstract class  Dier
{
   public abstract string MaakGeluid();
}
```
Twee child-klassen:
```csharp
class Paard: Dier
{
  public override string MaakGeluid()
  { 
      return "Hinnikhinnik";
  }
}

class Varken: Dier
{
  public override string MaakGeluid()
  { 
      return "Oinkoink";
  }
}
```

Dankzij polymorfisme kunnen we nu elders objecten van Paard en Varken in een ``Dier`` bewaren, maar toch hun eigen geluid laten reproduceren:
```csharp
Dier someAnimal = new Varken();
Dier anotherAnimal = new Paard();
Console.WriteLine(someAnimal.MaakGeluid()); //Oinkoink
Console.WriteLine(anotherAnimal.MaakGeluid()); //Hinnikhinnik
```

Het is belangrijk te beseffen dat  ``someAnimal`` en ``anotherAnimal`` van het type ``Dier`` zijn en dus enkel die dingen kunnen die in ``Dier`` beschreven staan. Enkel zaken die ``override`` zijn in de child-klasse zullen met de specialisatie-code werken.

### Arrays en polymorfisme

Arrays en lijsten laten heel krachtige code toe. Je kan een lijst van de basis-klasse maken en deze vullen met allerlei objecten van de basis-klasse **én de child-klassen**. 

Een voorbeeld:

```csharp
List<Dier> zoo = new List<Dier>();
zoo.Add(new Varken());
zoo.Add(new Paard());
foreach(var dier in zoo)
{
  Console.WriteLine(dier.MaakGeluid());
}
```

## Polymorfisme in de praktijk: Presidenten

![Gezocht: wie weet waar deze still vandaan komt? Ik heb geen flauw benul. We zien Robert Redford en Bush Senior, uit een film ofzo?](../assets/9_interfaces/president.jpg)

Beeld je in dat je een klasse President hebt met een methode "RunTheCountry" (voorbeeld van [StackOverflow](https://stackoverflow.com/questions/1031273/what-is-polymorphism-what-is-it-for-and-how-is-it-used) ). De President heeft toegang tot tal van adviseurs die hem kunnen helpen (inzake miltair, binnenlands beleid, economie). Zonder de voordelen van polymorfisme zou de klasse President er zo kunnen uitzien, **slechte manier**:

```csharp
public class President
{
    MilitaryMinister Petraeus= new MilitaryMinister();
    ForeignSecretary Condi = new ForeignSecretary();
    HealthOfficial MrX = new HealthOfficial();

    public void RunTheCountry()
    {
        // people walk into the Presidents office and he tells them what to do
        // depending on who they are.

        // Fallujah Advice - Mr Prez tells his military exactly what to do.
        Petraeus.IncreaseTroopNumbers();
        Petraeus.ImproveSecurity();
        Petraeus.PayContractors();

        // Condi diplomacy advice - Prez tells Condi how to negotiate

        Condi.StallNegotiations();
        Condi.LowBallFigure();
        Condi.FireDemocraticallyElectedIraqiLeaderBecauseIDontLikeHim();

        // Health care mr X

        MrX.IncreasePremiums();
        MrX.AddPreexistingConditions();
    }
}
```

De MilitaryMinister zou er zo kunnen uitzien:
```csharp
class MilitaryMinister
{
  public void IncreaseTroopNumbers()
  {
    //..
  }
  public void ImproveSecurity()
  {
    //..
  }
  etc
}
```

De HealthOfficial-klasse heeft dan weer heel andere publieke methoden. En die Foreignminister ook weer totaal andere.

Je merkt dat de President (of de programmeur van deze klasse) aardig wat specifieke kennis moet hebben van de vele verschillende departementen van het land. Uiteraard is dat onmogelijk (een fictief voorbeeld: stel je Trump voor...Denk je echt dat die zo veel weet?). Bovenstaande code is dus zeer slecht. Telkens er zaken binnen een specifiek landsonderdeel wijzigen moet dit ook in de klasse President aangepast worden. 

Dankzij polymorfisme kunnen we dit alles veel mooier oplossen:

1. We verplichten alle adviseurs dat ze overerven van de abstracte klasse ``Advisor`` die maar 1 abstracte methode heeft ``Advise``:

```csharp
abstract class Advisor
{
  abstract public void Advise();
}

class MilitaryMinister:Advisor
{
  public override void Advise()
  {
       increaseTroopNumbers();
       improveSecurity();
       payContractors();
  }
  private void increaseTroopNumbers(){ ... }
  private void improveSecurity(){ ... }
  private void payContractors(){ ... }
  }
}

class ForeignSecretary:Advisor
{
  //...
}
class HealthOfficial:Advisor
{
  //...
}
```

2° Het leven van de President wordt plots véél makkelijker:

```csharp
public class MisterPresident
{
    public void RunTheCountry()
    {
        Advisor Petraeus = new MilitaryAdvisor();
        Advisor Condi = new ForeignSecretary();
        Advisor mrX= new HealthOfficial();
        Petraeus.Advise(); // # Petraeus says send 100,000 troops to Fallujah
        Condi.Advise(); // # she says negotiate trade deal with Iran
        mrX.Advise(); // # they say we need to spend $50 billion on ObamaCare
    }
}
```

3° En we kunnen hem nog helpen door met een array of ``List<Advisor>`` te werken zodat hij ook niet steeds de "namen" van z'n adviseurs moet kennen:

```csharp
public class MisterPresident
{
    public void RunTheCountry()
    {   
        List<Advisor> allMinisters= new List<Advisor>();
        allMinisters.Add(new MilitaryAdvisor());
        allMinisters.Add(new ForeignSecretary());
        allMinisters.Add(new HealthOfficial());

        //Ask advise from each:
        foreach (Advisor minister in allMinisters)
        {
            minister.Advise();
        }
    }
}
```

En wie zei dat het presidentsschap moeilijk was?!

## Nog voorbeelden van polymorfisme nodig?

Volgende tekst heeft een leuke insteek om polymorfisme uit te leggen... aan de hand van...wait for it... Zeemeerminnen! :) [Lezen maar!](http://www.techoschool.com/Technology/Dotnet/Csharp-for-Beginners_Csharp-Polymorphism)

Volgende voorbeeld is iets praktischer: [Arena with a mage in C# .NET (inheritance and polymorphism)](https://www.ict.social/csharp/oop/arena-with-mage-in-csharp-net-inheritance-and-polymorphism)

# Why should I care?
![](../assets/care.jpg)

Polymorfisme is een heel krachtig concept. Door objecten te bewaren in hun basistype en, wanneer nodig, ze als 'zichzelf' te gebruiken wordt je code een pak eenvoudiger.
Vaak weet je niet op voorhand wat voor elementen je in je lijst wilt plaatsen. Via polymorfisme lossen we dit op. Stel bijvoorbeeld dat je een lijst van Personen hebt (``List<Person>``) waar echter elementen van subklassen in terecht kunnen komen (``Bakker``, ``Student``, etc.), dan laat polymorfisme dit gewoon toe om ook deze elementen in die lijst te plaatsen.
