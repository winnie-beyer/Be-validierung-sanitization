 1. **Erstellen Sie die package.json mit `npm init -y`**
 2. **Setzen Sie den Typ in der packag.json auf `type: modules`**
 3. **Installieren Sie die benötigten Pakete `npm i   dotenv express  mongoose`**

 4. **Einrichten von Umgebungsvariablen:**
 - Erstellen Sie eine .env-Datei mit folgendem Inhalt
  ```javascript

  MONGO_URL = your monGO URL
  PORT=3000
  ```

5. **Datenbankverbindung erstellen**
 - Erstellen Sie eine neue ordner  `database`
 - In `database` Erstellen Sie eine neue Datei  `connectDB.js`
 - In der `connectDB.js`-Datei erstellen Sie eine Datenbankverbindung mit mongoDB, wie folgt:

```javascript
import mongoose from "mongoose";

const connectDB = (url) => {
  return mongoose.connect(url);
};

export default connectDB;
```

6. **Benutzermodell erstellen**
 - Create a new file  models/user

 ```js
import mongoose from 'mongoose'
import validator from 'validator'
const { Schema } = mongoose;

const userSchema = new Schema({
    biography: {
        type: String,
        validate: {
            validator: function (v) {
                // Überprüfe die Länge
                if (v.length > 500) { return false }

                // Überprüfe auf HTML-Tags - kein XSS bitte!
                if (v.includes(">") || v.includes("<")) { return false }

                // Anstatt so zu validieren, könnten wir bereinigen
                // indem wir die schlechten Zeichen automatisch entfernen

                // Gültig!
                return true
            },
            message: props => 'Ungültige Biografie!'
        },
        required: true
    },
    email: {
        type: String,
        required: true,
        validate: {
            validator: function (v) {
                return validator.isEmail(v)
            },
            message: props => `${props.value} ist keine gültige E-Mail!`
        },
    },
    password: {
        type: String,
        required: true,
        validate: {
            validator: function (v) {
                return v.length > 12
            },
            message: props => `Kein gültiges Passwort!`
        },
    },
    username: {
        type: String,
        required: true,
        validate: {
            validator: function (v) {
                // Beachte, die "whitelist" Funktion ist für die Bereinigung
                // Wir sollten wahrscheinlich die Benutzereingabe für
                // den Benutzernamen nicht ändern, obwohl wir könnten!

                // Erlaube nur alphanumerische Zeichen und Unterstriche
                return validator.matches(v, /^[a-zA-Z0-9_]+$/)
            },
            message: props => `${props.value} ist kein gültiger Benutzername!`
        },
    },
})

```

7. **Express-Server erstellen und die erforderlichen Middleware importieren**

 - Erstellen Sie eine neue Datei  `server.js`
 - In der `server.js`-Datei erstellen Sie eine Express-Server  mit erforderlichen Middleware und connectDb mit `url` abrufen, wie folgt:

```javascript
import express from "express";
import dotenv from "dotenv";
import cors from "cors";
import connectDB from "./database/connectDB.js";
dotenv.config();

const app = express();

app.use(cors());
app.use(express.json());

const port = process.env.PORT || 5060;
const startServer = async () => {
  try {
    await connectDB(process.env.MONGO_URL);
    console.log("Verbindung mit MongoDB hat geklappt");
    app.listen(port, () => {
      console.log("Server läuft auf:", port);
    });
  } catch (error) {
    console.log(error);
  }
};

startServer();


```


8. **Importieren Sie  User model:**
 - Import the `User` model from `./model/user.js` which defines the schema for storing user data.
  ```js
  const User = require("./model/user");
  ```



## Endpoints

9. **Benutzerregistrierungs-Endpunkt::**
 - Definieren Sie einen POST-Endpunkt unter /register für die Benutzerregistrierung.
- Validieren Sie die Benutzereingabe (E-Mail, Passwort, Biografie, Benutzername).
- Erstellen Sie einen neuen Benutzer in der Datenbank.
- Senden Sie die Daten des neuen Benutzers in der Antwort.

```javascript   
app.post("/register", async (req, res) => {
    const { email, password, biography, username } = req.body

    if (!email || !password || !biography || !username) {
        return res.status(400).json({ error: 'Invalid registration' })
    }

    try {
        const user = await User.create({ biography, email, password, username })
        res.status(201).json(user)
    } catch (error) {
        res.status(500).json({ error: 'Error creating user' })
    }
})


```
10.  **Benutzeranmelde-Endpunkt:**
   - Definieren Sie einen POST-Endpunkt unter /login für die Benutzeranmeldung.
   - Validieren Sie die Benutzereingabe (E-Mail und Passwort).
 


 ```javascript  
app.post("/login", async (req, res) => {
    const { email, password } = req.body

    if (!email || !password ) {
        return res.status(400).json({ error: 'Invalid login' })
    }

    const user = await User.findOne({ email })
    if (!user) {
        return res.status(401).json({ error: 'Invalid login' })
    }

 
    res.json({ status: "success ", user })
})
```



11.  **get all users**
   - Definieren Sie einen GET-Endpunkt unter /users für die Benutzeranmeldung.
    - Senden Sie eine JSON-Antwort mit allen Benutzern.
    - 
```javascript    
// This is simply for testing purposes
app.get('/users', async (req, res) => res.json(await User.find()))
```

