# Oefeningen 1
## 1.1
### Klasse Main
```java
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.Month;

/**
 *
 * @author maxim
 */
public class Main {
    public static void main(String[] args) {
        Vlucht vlucht = new Vlucht(Locatie.PARIJS, LocalDateTime.of(2019, Month.OCTOBER, 6, 11, 50, 0), "BE99109");
        Passagier passagier = new Passagier("Janssens", "Maxim", "00.08.000-30321", vlucht);
        passagier.setGeboortedatum(LocalDate.of(2000, Month.AUGUST, 24));
        System.out.println("Passagier: " + passagier.toString());
    }
}
```
### Enumeratie Locatie (voor bestemming & vertrek)
Enumeraties in Java zijn een stuk veelzijdiger dan in C#, en het is bijgevolg gebruikelijk dat deze worden gedeclareerd in een apart bestand. De extra functionaliteiten van enums worden gebruikt in oefening 3.
```java
public enum Locatie {
    PARIJS, ROME, KEULEN, BRUSSEL;
}
```
### Klasse Passagier
Zoals je misschien al gemerkt hebt, werkt de shortcut 'prop-tab-tab' hier niet meer. Dit komt omdat properties in Java niet bestaan. In plaats daarvan werken we met gewone fields (die expliciet `private` dienen te staan, anders kunnen de waarden rechtsreeks worden aangepast vanuit andere klassen binnen hetzelfde package.). Om de waarden te kunnen opvragen of aanpassen vanuit andere klasses, maken we gebruik van respectievelijk getters en setters. Dit zijn methodes die niets anders doen dan de waarde van een bepaald field returnen of instellen. `setGeboortedatum` hieronder is een voorbeeld van een setter.

Een ander verschil met C# is dat je het `override`-keyword niet meer hoeft te gebruiken als je een methode overschrijft. Bijvoorbeeld bij de `toString()`-methode. Wel is het gebruikelijk om `@Override` op het lijntje erboven te zetten, zodat je zelf weet wanneer je een methode overschrijft. Dit wordt een annotatie genoemd, en deze worden niet mee gekopieerd wanneer je een stuk code kopiert. Daarom zal je deze annotaties ook niet terugvinden in de oefeningen die hier op GitHub staan. Als je een annotatie vergeet te plaatsen, zal je IDE hiervan een melding geven.
```java
import java.time.LocalDate;

/**
 *
 * @author maxim
 */
public class Passagier {
    private String naam, voornaam, rijksregisternummer;
    private LocalDate geboortedatum;
    private Vlucht vlucht;
    
    public void setGeboortedatum(LocalDate geboortedatum) {
        this.geboortedatum = geboortedatum;
    }
    
    public Passagier(String naam, String voornaam, String rijksregisternummer, Vlucht vlucht){
        this(naam, voornaam, rijksregisternummer, vlucht, null);
    }
    
    public Passagier(String naam, String voornaam, String rijksregisternummer, Vlucht vlucht, LocalDate geboortedatum){
        this.naam = naam;
        this.voornaam = voornaam;
        this.rijksregisternummer = rijksregisternummer;
        this.vlucht = vlucht;
        this.geboortedatum = geboortedatum;
    }
    
    public String toString(){
        return "Naam: " + naam + "\nVoornaam: " + voornaam + "\nRijksregisternummer: " + rijksregisternummer + "\nGeboortedatum: " + geboortedatum + "\nVlucht: " + vlucht;
    }
}

```
### Klasse Vlucht
```java
import java.time.LocalDateTime;

/**
 *
 * @author maxim
 */
public class Vlucht {
    private Locatie bestemming;
    private Locatie vertrek = Locatie.BRUSSEL;
    private LocalDateTime tijdstipVertrek;
    private String vluchtnummer;
    
    public Vlucht(Locatie bestemming, LocalDateTime tijdstipVertrek, String vluchtnummer){
        this.bestemming = bestemming;
        this.tijdstipVertrek = tijdstipVertrek;
        this.vluchtnummer = vluchtnummer;
    }
    
    @Override
    public String toString(){
        return "Bestemming: " + bestemming + "\nVertrek: " + vertrek + "\nTijdstip vertrek: " + tijdstipVertrek + "\nVluchtnummer: " + vluchtnummer;
    }
}
```
## Oefening 1.3
### Klasse Main
```java
import java.time.LocalDateTime;
import java.time.Month;
import javafx.util.Duration;

/**
 *
 * @author maxim
 */
public class Main {
    public static void main(String[] args) {
        Presentator erik = new Presentator("Van Looy", "Erik");
        Programma deSlimsteMens = new Programma("De Slimste mens", erik, Duration.hours(1), LocalDateTime.of(2019, Month.MARCH, 2, 20, 30), Genre.KOMEDIE);
        Zender vier = new Zender("Vier");
        vier.addProgramma(deSlimsteMens);
        
        System.out.println(vier);
    }
}
```
### Klasse Presentator
```java
public class Presentator {
    private String naam, voornaam;
    
    public Presentator(String naam, String voornaam){
        this.naam = naam;
        this.voornaam = voornaam;
    }
    
    @Override
    public String toString(){
        return voornaam + " " + naam;
    }
}
```
### Klasse Programma
```java
import java.time.LocalDateTime;
import javafx.util.Duration;

/**
 *
 * @author maxim
 */
public class Programma {
    private String naam;
    private Presentator presentator;
    private Duration tijdsduur;
    private LocalDateTime uitzendTijdstip;
    private Genre genre;
    
    public Programma(String naam, Presentator presentator, Duration tijdsduur, LocalDateTime uitzendTijdstip, Genre genre){
        this.naam = naam;
        this.presentator = presentator;
        this.tijdsduur = tijdsduur;
        this.uitzendTijdstip = uitzendTijdstip;
        this.genre = genre;
    }
    
    @Override
    public String toString(){
        return "Naam: " + naam + "\nPresentator: " + presentator + "\nTijdsduur: " + tijdsduur.toHours() + "h\nUitzend tijdstip: " + uitzendTijdstip + "\n" + genre.getBeschrijving();
    }
}
```
### Enumeratie Genre
In de volgende enumeratie zie je al één van de extra functionaliteiten van enums binnen Java. Enumeraties gedragen zich eigenlijk gewoon als klasses. Hier hebben we een beschrijving toegevoegd, en via de `private`(!) constructor, kunnen we de beschrijving instellen.
```java
public enum Genre {
    ACTIE("Een programma vol actie"), THRILLER("Een spannend programma"), ROMANTIEK("Een programma vol romantiek"), KOMEDIE("Een grappig programma");
    
    private String beschrijving;
    
    private Genre(String beschrijving){
        this.beschrijving = beschrijving;
    }
    
    public String getBeschrijving(){
        return beschrijving;
    }
}
```
### Klasse Zender
```java
import java.util.ArrayList;

/**
 *
 * @author maxim
 */
public class Zender {
    private String naam;
    private ArrayList<Programma> programmas = new ArrayList<Programma>();
    
    public Zender(String naam){
        this.naam = naam;
    }
    
    public void addProgramma(Programma programma){
        programmas.add(programma);
    }
    
    public String programmasToString(){
        String result = "";
        for(Programma programma : programmas){
            result += "\n" + programma;
        }
        return result;
    }
    
    @Override
    public String toString(){
        return "Zender: " + naam + "\nProgramma's: " + programmasToString();
    }
}
```