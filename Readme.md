# BE - Sicherheit 


**Zeit für Forschung: Validierung und sanitization**

  - Verbringe 30-45 Minuten damit, über Validierung und sanitization in webdevelopment zu recherchieren und zu lesen.


> Lernziele
> Validierung, sanitization
> - Daten konsistent und sicher machen
> - Vertraue nicht darauf, dass das Frontend Eingaben validiert!
> - Validierung und sanitization zur Verhinderung von Injektionen und fehlerhaften Daten

## Validierung

- `streetAddress:String` sollte nicht 9999 Zeichen lang sein
- `email:String` sollte tatsächlich wie eine E-Mail aussehen

- Das sind Beispiele für Validierung
- Wenn unser Backend für ein E-Mail-Feld eine Zeichenkette `asdfgh` erhält
- Es sollte nicht versuchen, damit ein Konto zu erstellen oder Bestätigungs-E-Mails zu senden

- Frontend-Eingabevalidierung hilft dem Benutzer
- Wir _brauchen auch_ Backend-Validierung
- Mit Mongoose-Validatoren kannst du viel machen
    - Welche Validatoren haben wir uns nochmal angesehen?

- Es gibt auch viele spezialisierte Validierungsbibliotheken

## sanitization

- `biography:String` sollte kein HTML/JS enthalten

- Das ist ein Beispiel für Bereinigung
    - Die Daten sollten bereinigt werden
    - Dies ist ein viel schwierigeres Problem und hängt stark von deinem Inhalt ab

- Also deine Anwendung erlaubt Benutzern, eine Textbiografie zu haben
    - Welche Zeichen erlaubst du?
    - Sind `<` und `>` erlaubt?
    - Sind spezielle Steuerzeichen wie Text-Richtungsüberschreibung erlaubt?
    - Du könntest Zeichen verbieten, von denen du weißt, dass sie Probleme verursachen können
        - Blockiere vielleicht `<` und `\n` zum Beispiel
    - Der sicherste Weg wäre vielleicht, nur sichere Zeichen zuzulassen
        - Aber welche?
            - A-Z, Leerzeichen, Komma, Punkt, Bindestrich, Fragezeichen, Ausrufezeichen...
            - Was ist mit Arabisch oder Chinesisch zum Beispiel?
    - Ist schlechte, beleidigende Sprache erlaubt?
        - Auch auf Finnisch und Koreanisch und Hebräisch und Isländisch?
        - Ist es überhaupt theoretisch möglich, sie herauszufiltern?
        - Oder gehört diese Aufgabe menschlichen Moderatoren?

- Was ist mit Benutzernamen?
    - Ist `<br>` ein gültiger Benutzername? Oder `>9000`? Oder `日本`?




## Zusammenfassung

- Validierung: Überprüfung eines Werts
- sanitization: Änderung eines Werts

## Übungen


## Selbststudium

- Wie würdest du eine E-Mail mit RegEx validieren? Und eine Telefonnummer?

Here’s an example of a comprehensive RegEx pattern for email validation:


^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$

Explanation
^: Asserts the position at the start of the string.
[a-zA-Z0-9._%+-]+: Matches one or more characters that can be letters (uppercase and lowercase), digits, dots (.), underscores (_), percent signs (%), plus signs (+), or hyphens (-). This represents the local part.
@: Matches the @ symbol.
[a-zA-Z0-9.-]+: Matches one or more characters that can be letters, digits, dots, or hyphens. This represents the domain name.
\.: Escapes the dot (.) to match it literally.
[a-zA-Z]{2,}: Matches two or more letters, representing the top-level domain (TLD).
$: Asserts the position at the end of the string.









Validating an email address using Regular Expressions (RegEx) can be done by defining a pattern that matches the general structure of an email. An email address typically follows the format: local-part@domain. Here's a breakdown of how you can construct a RegEx to validate an email address:

Basic Structure of an Email Address
Local Part: The part before the @ symbol.
Can include letters (both uppercase and lowercase), digits, dots (.), hyphens (-), and underscores (_).
Dots cannot appear consecutively or at the beginning or end.
Domain Part: The part after the @ symbol.
Consists of a domain name and a top-level domain (TLD).
Domain name can include letters, digits, hyphens, and dots.
TLD is usually a set of letters (e.g., .com, .net, .org).

Full Example


import re

def validate_email(email):
    pattern = r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'
    if re.match(pattern, email):
        return True
    else:
        return False

# Test the function
emails = ["test@example.com", "invalid-email@", "user.name+tag+sorting@example.com", "another.email@example.co.uk"]

for email in emails:
    print(f"{email}: {validate_email(email)}")




### Full Example

Here’s how you can use this pattern in Python to validate telephone numbers:

```python
import re

def validate_phone_number(phone_number):
    pattern = r'^\+?(\d{1,3})?[-. (]*\(?\d{1,4}\)?[-. )]*\d{1,4}[-. ]*\d{1,4}[-. ]*\d{1,9}(\s*(ext|x|ext.)\s*\d{1,5})?$'
    if re.match(pattern, phone_number):
        return True
    else:
        return False

# Test the function
phone_numbers = [
    "+1-800-555-1234",
    "(123) 456-7890",
    "123-456-7890",
    "123.456.7890",
    "+44 20 7123 4567",
    "1234567890",
    "+49-89-636-48018 ext. 123",
    "invalid-number"
]

for number in phone_numbers:
    print(f"{number}: {validate_phone_number(number)}")
```

### Explanation of Examples

- **`+1-800-555-1234`**: Valid international number, should return `True`.
- **`(123) 456-7890`**: Valid US number with area code in parentheses, should return `True`.
- **`123-456-7890`**: Valid US number without country code, should return `True`.
- **`123.456.7890`**: Valid US number with dots as separators, should return `True`.
- **`+44 20 7123 4567`**: Valid UK number, should return `True`.
- **`1234567890`**: Valid number without any separators, should return `True`.
- **`+49-89-636-48018 ext. 123`**: Valid German number with extension, should return `True`.
- **`invalid-number`**: Invalid number, should return `False`.

### Conclusion

This RegEx pattern is designed to handle various common formats for telephone numbers, including optional country codes, area codes, separators, and extensions. It should work well for most practical purposes, though for comprehensive validation (especially for specific country formats), more tailored patterns or validation libraries may be necessary.