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