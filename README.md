# 2Η ΕΡΓΑΣΙΑ ΕΞΑΜΗΝΟΥ ΠΛΗΡΟΦΟΡΙΑΚΩΝ ΣΥΣΤΗΜΑΤΩΝ
## 'Ονομα: ΝΟΜΙΚΙ ΠΑΡΓΙΝΟΥ
## ΑΜ: Ε18130

Για την υλοποίηση του συστήματος **DSMarkets** δημιουργήθηκε ένα αρχείο python "*market.py*" που εκτελεί όλες τις λειτουργίες που ζητήθηκαν, καθώς και δύο αρχεία *"users.json"* και *"products.json"* που περιέχουν κάποιους αρχικούς χρήστες και κάποια αρχικά προϊόντα αντίστοιχα.

> Παράδειγμα περιεχομένων products.json
````json
{
    "_id": {
        "$oid": "5e99cb577a781a4aac69da46"
    },
    "name": "milk",
    "category": "dairy",
    "stock": {"$numberInt":"10"},
    "description": "High Pasteurised 1.5lt",
    "price":"1,5"
}
{
    "_id": {
        "$oid": "5e99cb577a781a4aac69da4d"
    },
    "name": "sugar",
    "category": "sweetener",
    "stock": {"$numberInt":"5"},
    "description": "white regular",
    "price":"2,5"
}
````

> Παράδειγμα περιεχομένων user.json
````json
{
    "_id": {
        "$oid": "5e99cb577a781a4aac69da3a"
    },
    "username": "nomiki",
    "email": "nomikipar@gmail.com",
    "password": "p@ssW0rd",
    "category": "admin"
}
{
    "_id": {
        "$oid": "5e99cb577a781a4aac69da3b"
    },
    "username": "ericksonpope",
    "email": "ericksonpope@gmail.com",
    "password": "baABwib2i",
    "category": "simple user"
}
````
### Login με χρήση του endpoint /login και επεξήγηση του authorization

Η διαδικασία του login γίνεται με τα εξής βήματα:
1. Κατά το login, ο χρήστης εισάγει email και password
2. Γίνεται αναζήτηση του email στο collection με τους Users
3. Αν βρεθεί, ελέγχεται εαν το πεδίο *"category"* ισούται με **simple user** ή **admin**.
4. Στη περίπτωση που ισούται με **simple user** τότε εισάγεται στο dictionary user_sessions.
5. Ενώ στη περίπτωση που ισούται με **admin** τότε εισάγεται στο dictionary admin_sessions.
6. Τέλος εμφανίζεται μήνυμα επιτυχίας που συνοδεύεται με την κατηγορία του χρήστη  και το session id.

![user login](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/071cf7f255d138ccf4f6924da379430286e908d5/images/admin%20/login/adminlog.png)

To authorization σε κάθε endpoint μετά το login γίνεται με τα παρακάτω βήματα:
1. Ο χρήστης εισάγει μόνο το session id του στα headers του request (δεν απαιτείται email ή password δηλαδή).
2. Αν το endpoint εκτελεί λειτουργία χρήστη, τότε το session id περνάται στη συνάρτηση is_user_session_valid και ελέγχεται εάν το uuid βρίσκεται μέσα στο user_sessions.
3. Αν το endpoint εκτελεί λειτουργία διαχειριστή, τότε το session id περνάται στη συνάρτηση is_admin_session_valid και ελέγχεται εάν το uuid βρίσκεται μέσα στο admin_sessions. 
4. Στα endpoints με λειτουργίες χρήστη που απαιτούν την αναζήτηση του χρήστη μέσα στο collection, μετά το authentication του, με τη χρήση του user1 = users_sessions.get(uuid), λαμβάνουμε απο το dictionary user_sessions το value που αντιστοιχεί στο key με τιμή το uuid του χρήστη. Το value που μας επιστρέφεται στο user1 είναι μια λίστα όπου στη θέση 0 βρίσκεται το email του χρήστη και στη θέση 1 η χρονική στιγμή που έκανε login. Κάνοντας χρήση του user1[0] λοιπόν μπορούμε να αναζητήσουμε τον χρήστη στο collection Users. 

