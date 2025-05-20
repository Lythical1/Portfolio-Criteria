# Documentatie Testscenario's

Dit document beschrijft de testscenario's voor de JobSpot-applicatie, inclusief stappen, verwachte resultaten en testgegevens voor zowel hoofdscenario's als alternatieve scenario's.

## Hoe Tests Uit te Voeren

Om alle tests in de applicatie uit te voeren, gebruik je een van de volgende methoden:

1. Gebruik het meegeleverde script:
```bash
php run-tests.php
```

2. Voer PHPUnit direct uit:
```bash
./vendor/bin/phpunit
```

## Overzicht Testbestanden

| Testbestand | Doel | 
|-----------|---------|
| FileUploadTest.php | Test de avatar-uploadfunctionaliteit |
| InterviewTest.php | Test het sollicitatiegesprekbeheersysteem |
| JobSearcherTest.php | Test de zoek- en filterfunctionaliteit voor vacatures |
| SearchHelperTest.php | Test tekstnormalisatie en zoekalgoritmen |
| UsersTest.php | Test gebruikersbeheeroperaties |

## Testscenario's

### 1. Bestandsupload Tests

#### Scenario 1.1: Avatar Upload met Geldig Bestand
**Stappen:**
1. Maak een mock-bestand met een geldig afbeeldingsformaat (JPEG)
2. Stel de bestandsgrootte in binnen de limieten (< 16MB)
3. Roep de uploadAvatar-methode aan

**Testgegevens:**
- Bestandsnaam: 'valid.jpg'
- Bestandstype: 'image/jpeg'
- Bestandsgrootte: klein (< 16MB)

**Verwacht Resultaat:**
- Bestand wordt succesvol geüpload
- Een unieke bestandsnaam wordt gegenereerd met 'avatar_' prefix
- Methode retourneert het nieuwe bestandspad

#### Scenario 1.2: Avatar Upload met Ongeldig Bestandstype
**Stappen:**
1. Maak een mock-bestand met ongeldig formaat (tekstbestand)
2. Roep de uploadAvatar-methode aan

**Testgegevens:**
- Bestandsnaam: 'test.txt'
- Bestandstype: 'text/plain'

**Verwacht Resultaat:**
- Exception wordt geworpen met melding "Invalid file type."
- Geen bestand wordt geüpload

#### Scenario 1.3: Avatar Upload met Te Groot Bestand
**Stappen:**
1. Maak een mock-bestand met geldig formaat maar te grote omvang
2. Roep de uploadAvatar-methode aan

**Testgegevens:**
- Bestandsnaam: 'large.jpg'
- Bestandstype: 'image/jpeg'
- Bestandsgrootte: 17MB (overschrijdt limiet van 16MB)

**Verwacht Resultaat:**
- Exception wordt geworpen met melding "File size exceeds the maximum limit."
- Geen bestand wordt geüpload

### 2. Sollicitatiegesprek Beheertests

#### Scenario 2.1: Ophalen van Aankomende Sollicitatiegesprekken
**Stappen:**
1. Stel een gebruikers-ID in voor de query
2. Mock de database om verwachte sollicitatiegespreksgegevens te retourneren
3. Roep de getUpcomingInterviews-methode aan

**Testgegevens:**
- Gebruikers-ID: 1
- Verwachte sollicitatiegesprekken: Twee voorbeeldgesprekken met verschillende datums

**Verwacht Resultaat:**
- Retourneert array van aankomende sollicitatiegesprekken
- Elk gesprek bevat functietitel en bedrijfsnaam
- Gesprekken zijn voor de opgegeven gebruiker

### 3. Vacature Zoeker Tests

#### Scenario 3.1: Alle Vacature Zoekers Ophalen
**Stappen:**
1. Roep de getSearchers-methode aan met lege query
2. Mock de database om zoekersgegevens te retourneren

**Testgegevens:**
- Lege querystring
- Voorbeeld vacaturezoekersgegevens in de mock

**Verwacht Resultaat:**
- Retourneert array met sleutel 'searchers' die alle vacaturezoekers bevat
- Geen filtering wordt toegepast

#### Scenario 3.2: Filtert Vacature Zoekers op Meerdere Categorieën
**Stappen:**
1. Stel categorie-ID-array in voor filtering
2. Mock de database om gefilterde resultaten te retourneren
3. Roep de filterSearchers-methode aan

**Testgegevens:**
- Categorie-ID's: [2, 3]
- Verwachte gegevens: Gefilterde vacaturezoekers die overeenkomen met deze categorieën

**Verwacht Resultaat:**
- Retourneert alleen vacaturezoekers die overeenkomen met de opgegeven categorieën
- SQL bevat correcte IN-clausule met placeholders
- Correct aantal resultaten wordt geretourneerd

#### Scenario 3.3: Filtert Vacature Zoekers op Enkele Categorie
**Stappen:**
1. Stel één categorie-ID in voor filtering
2. Mock de database om gefilterde resultaten te retourneren
3. Roep de filterSearchers-methode aan

**Testgegevens:**
- Categorie-ID: 2
- Verwachte gegevens: Vacaturezoekers voor categorie 2

**Verwacht Resultaat:**
- Retourneert alleen vacaturezoekers voor de opgegeven categorie
- SQL bevat correcte WHERE-clausule
- Correcte resultaten worden geretourneerd

### 4. Zoek Helper Tests

#### Scenario 4.1: String Normalisatie
**Stappen:**
1. Roep normalizeString aan met verschillende invoerteksten
2. Controleer de geretourneerde genormaliseerde strings

**Testgegevens:**
- "Hello World!"
- "Special-Chars!@#"
- null
- ""
- "  Trim Spaces  "

**Verwacht Resultaat:**
- Teksten worden omgezet naar kleine letters
- Speciale tekens worden verwijderd
- Witruimte wordt bijgesneden
- Null-waarden worden correct afgehandeld

#### Scenario 4.2: String Gelijkheidsvergelijking
**Stappen:**
1. Roep areSimilar aan met paren van strings
2. Test met verschillende drempelwaarden

**Testgegevens:**
- Exacte overeenkomsten: "developer" en "developer"
- Hoofdletterverschillen: "Developer" en "developer"
- Kleine verschillen: "developer" en "developr"
- Grote verschillen: "developer" en "programmer"
- Tests met aangepaste drempelwaarden

**Verwacht Resultaat:**
- Retourneert true voor strings binnen de gelijkheidsdrempel
- Retourneert false voor strings buiten de drempel
- Hoofdletterongevoelige vergelijking werkt
- Aangepaste drempelwaarden worden gerespecteerd

#### Scenario 4.3: Salaris Formattering
**Stappen:**
1. Roep formatSalary aan met verschillende salaristeksten
2. Controleer geformatteerde uitvoer

**Testgegevens:**
- Enkel salaris: "50000"
- Salarisbereik: "40000-60000"
- Met valutaprefix: "EUR50000"
- Lege string: ""
- Null-waarde

**Verwacht Resultaat:**
- Enkel salaris geformatteerd met valutasymbool
- Bereiken geformatteerd met valutasymbolen en scheidingsteken
- Valutaprefixen correct afgehandeld
- Lege/null-waarden tonen "Salary not specified"

### 5. Gebruikersbeheer Tests

#### Scenario 5.1: Gebruiker Ophalen op ID
**Stappen:**
1. Stel een gebruikers-ID in
2. Mock de database om gebruikersgegevens te retourneren
3. Roep de getUser-methode aan

**Testgegevens:**
- Gebruikers-ID: 1
- Verwachte gebruikersgegevens met naam, e-mail en telefoon

**Verwacht Resultaat:**
- Retourneert gebruikersgegevens voor het opgegeven ID
- SQL-query gebruikt het juiste gebruikers-ID
- Alle gebruikersvelden zijn opgenomen

#### Scenario 5.2: Gebruikersinformatie Bijwerken
**Stappen:**
1. Stel gebruikersgegevens in voor update
2. Mock de database-updateoperatie
3. Roep de updateUserInfo-methode aan

**Testgegevens:**
- Gebruikers-ID: 1
- Voornaam: "John"
- Achternaam: "Smith"
- E-mail: "john.smith@example.com"
- Telefoon: "987654321"

**Verwacht Resultaat:**
- SQL-updatequery is correct gevormd
- Alle velden zijn opgenomen in de update
- Gebruikers-ID wordt gebruikt in WHERE-clausule