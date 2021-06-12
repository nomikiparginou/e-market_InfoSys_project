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

![user auth](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/ccf75483ca7fad1c011f9f22e06db3f5c0dba978/images/users/auth.png)

### Αναζήτηση Προϊόντος (/searchProduct)
Για την αναζήτηση προϊόντος, ο χρήστης εισάγει είται το όνομα, είτε την κατηγορία, είτε το id του προϊόντος καθώς και το session id του. Αφού γίνει authorized, ελέγχεται το input που έδωσε και με την συνάρτηση *find()* αναζητούνται τα προϊόντα με βάση το κριτήριο που δώθηκε και επιστρέφονται στο **product_list**. Με ένα for loop εκτυπώνονται οι πληροφορίες για κάθε προϊόν στο product_list. Αν δεν βρεθεί κανένα προϊόν, εμφανίζεται ανάλογο αποτέλεσμα. 
> Παραδείγματα εκτέλεσης

![search1](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/ccf75483ca7fad1c011f9f22e06db3f5c0dba978/images/users/search%20prod/search1.png)
![search2](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/ccf75483ca7fad1c011f9f22e06db3f5c0dba978/images/users/search%20prod/search2.png)
![search3](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/ccf75483ca7fad1c011f9f22e06db3f5c0dba978/images/users/search%20prod/search3.png)

### Προσθήκη προϊόντων στο καλάθι (/addToCart)

Για την προσθήκη κάποιου προϊόντος στο καλάθι, ο χρήστης εισάγει τον κωδικό του προϊόντος και τη ποσότητα που επιθυμεί. Μετά την αυθεντικοποίηση του και την εύρεση του στο collection Users, αναζητούμε με βαση το id το προϊόν που θέλει. Αν βρεθεί το προϊόν, τότε ελέγχουμε αν το stock του είναι μεγαλύτερο ή ίσο απο την ποσότητα που ζήτησε ο χρήστης. Αν ζητήσει μεγαλύτερη ποσότητα απο το stock, τότε εμφανίζει ανάλογο μήνυμα. Αλλιώς ελέγχεται αν υπάρχει ήδη key *"cart"* στον συγκεριμένο user και άν οχι, τότε τον κάνει update προσθέτοντας του το key "cart" και για τιμή εισάγει το pair "id:quantity" του προϊόντος που θέλει να προσθέσει στο καλάθι του. Αν το key "cart" υπάρχει ήδη στον χρήστη, τότε ελέγχεται αν είναι άδειο (δηλαδή αν έχει ξανακάνει αγορά αλλά το καλάθι του τώρα είναι άδειο) και εκτελεί την ίδια λειτουργία με την προηγούμενη περίπτωση. Αν δεν είναι άδειο όμως, τότε επιστρέφεται το περιεχόμενό τού στο dictionary "cart1", το όποιο στη συνέχεια κάνουμε update προσθέτοντας του το νεό προϊόν με τη ποσότητα του  και κάνουμε στη συνέχεια update το cart του χρήστη με το νεό. Τέλος, αναζητούμε πάλι τον χρήστη για να τυπώσουμε το νέο του καλάθι και να βεβαιωθούμε πως προτέθηκε το νέο προϊόν με επιτυχία. 
> Παραδείγματα εκτέλεσης

![add prod](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/ccf75483ca7fad1c011f9f22e06db3f5c0dba978/images/users/add%20cart/cart2.png)
![out of stock](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/ccf75483ca7fad1c011f9f22e06db3f5c0dba978/images/users/add%20cart/outstock.png)
![wrong prd](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/ccf75483ca7fad1c011f9f22e06db3f5c0dba978/images/users/add%20cart/wadd.png)

### Εμφάνιση καλαθιού (/showCart)

Το endpoint αυτο δεν δέχεται κάποιο data, μόνο το session id του χρήστη που επιθυμεί να εμφανίσει το καλάθι του. Μετά την αυθεντικποίηση του χρήστη, ελέγχουμε αν έχει καλάθι, και αν δεν έχει ή είναι άδειο τοτε εμφανίζεται το ανάλογο μήνυμα. Αν το καλάθι περιέχει μέσα πράγματα, τότε για κάθε product id μέσα στο καλάθι, κάνουμε αναζήτηση του προϊόντος στο collection products. Από αυτό μετά παίρνουμε όνομα, τιμή και υπολογίζεται το συνολικό κόστος του καλαθιού κάνοντας τιμή * ζητούμενη ποσότητα για κάθε προϊόν. 

![show cart](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/17d39b71bbdc6966fe924283bb4149e29e30d0d4/images/users/show%20cart/show.png)

### Διαγραφή προϊόντος απο το καλάθι (/deleteFromCart)

Για να διαγράψει κάποιος χρήστης ένα προϊόν απο το καλάθι του, πρέπει να εισάγει το id του προϊόντος, καθώς και το session id του. Μετή την αυθεντικοποίηση και εύρεση του στο collection users, ελέγχουμε όπως και στο προηγούμενο ερώτημα το καλάθι του. Μόνο στη περίπτωση που ο χρήστης έχει καλάθι και προϊόντα μεσα του, ελέγχουμε με μία for loop ποιό απο τα product ids ταυτίζεται με εκείνο που έδωσε ο χρήστης. Όταν βρεθεί αυτο το id, και εφόσον έχουμε φτιάξει ένα αντίγραφο του καλαθιού του (cart1), διαγράφουμε απο αυτό το  προϊόν με εκείνο το id και στη συνέχεια κάνουμε set το cart του χρητης με το ενημερωμένο. Αν δεν βρεθεί μέσα στο cart το id του προϊόντος που έδωσε ο χρήστης, τοτε εμφανίζεται ανάλογο μήνυμα αποτυχίας. 

![delete from cart](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/17d39b71bbdc6966fe924283bb4149e29e30d0d4/images/users/delete%20from%20cart/delcart.png)

### Αγορά προϊόντων (/placeOrder)
