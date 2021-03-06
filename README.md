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
> Παράδειγμα εκτέλεσης

![user auth](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/ccf75483ca7fad1c011f9f22e06db3f5c0dba978/images/users/auth.png)

## Λειτουργίες Απλού Χρήστη

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
> Παράδειγμα εκτέλεσης

![show cart](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/17d39b71bbdc6966fe924283bb4149e29e30d0d4/images/users/show%20cart/show.png)

### Διαγραφή προϊόντος απο το καλάθι (/deleteFromCart)

Για να διαγράψει κάποιος χρήστης ένα προϊόν απο το καλάθι του, πρέπει να εισάγει το id του προϊόντος, καθώς και το session id του. Μετή την αυθεντικοποίηση και εύρεση του στο collection users, ελέγχουμε όπως και στο προηγούμενο ερώτημα το καλάθι του. Μόνο στη περίπτωση που ο χρήστης έχει καλάθι και προϊόντα μεσα του, ελέγχουμε με μία for loop ποιό απο τα product ids ταυτίζεται με εκείνο που έδωσε ο χρήστης. Όταν βρεθεί αυτο το id, και εφόσον έχουμε φτιάξει ένα αντίγραφο του καλαθιού του (cart1), διαγράφουμε απο αυτό το  προϊόν με εκείνο το id και στη συνέχεια κάνουμε set το cart του χρητης με το ενημερωμένο. Αν δεν βρεθεί μέσα στο cart το id του προϊόντος που έδωσε ο χρήστης, τοτε εμφανίζεται ανάλογο μήνυμα αποτυχίας. 
> Παραδειγμα εκτέλεσης

![delete from cart](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/17d39b71bbdc6966fe924283bb4149e29e30d0d4/images/users/delete%20from%20cart/delcart.png)

### Αγορά προϊόντων (/placeOrder)

Για την αγορά των προϊόντων που υπάρχουν στο καλάθι του, ο χρήστης εισάγει τον 16 ψήφιο αριθμό της κάρτας του και το session id του. Μετά την αυθεντικοποίηση και την εύρεση του χρήστη, ελέγχεται εάν ο αριθμός κάρτας που εισήγαγε είναι 16 ψηφία και αν δεν είναι εμφανίζει μήνυμα αποτυχίας. Αν η συνθήκη είνια αληθής, τότε ελέγχεται εάν ο χρήστης έχει καλάθι με προϊόντα προς αγορά και αν ναι, προσθέτουμε κάθε προϊόν που υπάρχει στο καλάθι  και τις πληροφοριες και ποσότητα και τιμή στην απόδειξη.  Επιπλέον, αφαιρείται απο το stock του κάθε προϊόντος η ποσότητα που αγόρασε ο χρήστης και τέλος αδειάζουμε το cart θέτοντας το ίσο με "None". Στη συνέχεια ελέγχουμε αν ο χρήστης έχει ήδη "orderHistory" πεδίο, και αν υπάρχει το αποθηκεύουμε σε τοπικο dictionary και το ενημερώνουμε προσθέτοντας τα στοιχεία του cart. Αλλίως απλα δημιουργείται εκείνη τη στιγμή κάνοντας το set με το cart του χρήστη.
> Παραδειγμα εκτέλεσης

![make order](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/e56e118ae151e5836e7dd87ae80c9cb1b84c551d/images/users/place%20order/order.png)
![wrong_order](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/e56e118ae151e5836e7dd87ae80c9cb1b84c551d/images/users/place%20order/worder.png)

### Εμφάνιση ιστορικού παραγγελιών (/orderHistory)

Αυτό το ερώτημα δεν δέχεται κάποιο data από τον χρήστη, μόνο το session id του. Μετά την αυθεντικοποίηση και την εύρεση του χρήστη, ελέγχεται εάν υπάρχει κάποιο ιστορικό παραγγελιών. Αν υπάρχει τότε εμφανίζεται κάθε προϊόν μέσα σε αυτό καθώς και η ποσότητα του. Αν όχι τότε εμφανίζεται ανάλογο μήνυμα. 
> Παράδειγμα εκτέλεσης

![order history](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/e56e118ae151e5836e7dd87ae80c9cb1b84c551d/images/users/order%20history/oh.png)

### Διαγραφή του λογαριασμού (/deleteUser)

Σε περίπτωση που ο χρήστης επιθυμεί να διαγράψει τον λογαριασμό του, τότε εισάγει τον κωδικό του (για λόγους ασφαλείας) καθώς και το session id του. Μετά την αυθεντκοποίηση και εύρεση του χρήστη στο collection Users, ελέγχεται εάν ο κωδικός που εισήγαγε ταυτίζεται με τον κωδικό που αντιστοιχεί στον λογαριασμό και στην συνέχεια γίνεται διαγραφή του απο τη βάση με βάση το email του. Σε περίπτωση που ο κωδικός είναι λανθασμένος, η διαγραφή δεν πραγματοποιείται και εμαφανίζεται μήνυμα αποτυχίας. 
> Παράδειγμα εκτέλεσης

![delete user](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/6f376a193d97e3ce9263758754478c2af612dd90/images/users/delete%20acc/delete1.png)
![wrong user del](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/6f376a193d97e3ce9263758754478c2af612dd90/images/users/delete%20acc/wdelet.png)

## Λειτουργίες Διαχειρηστή

### Εισαγωγλη νέου προϊόντος (/addProduct)

Για να εισάηγει ένας διαχειρηστής ένα νέο προϊόν στο collection Products, πρέπει να δώσει ως data το **"name", "category", "stock", "price" και "description"**, καθώς και το session id του. Αφού αυθεντικοποιηθεί ως διαχειρηστής, τότε το προϊόν γίνεται inserted στο collection products με τις πληροφορίες που δώθηκαν απο αυτόν. Στη περίπτωση που πάει να εκτελέσει αυτή τη λειτουργία κάποιος απλός πελάτης, τότε εμφανίζεται ανάλογο μήνυμα αποτυχίας.
> Παραδειγματα εκτέλεσης

![add product](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/7c2d6c2c6909b60fe924225d61ec073f614ec3b4/images/admin%20/add%20product/addprod.png)
![wrong add prod](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/7c2d6c2c6909b60fe924225d61ec073f614ec3b4/images/admin%20/add%20product/wrongadd.png)

### Διαγραφή προϊόντος απο το σύστημα (/deleteProduct)

Για την διαγραφή ενός προϊόντος απο τη βάση, ο διαχειρηστής εισάγει το id του προϊόντος που θέλει να διαγράψει, μαζί με το session id του. Αφού αυθεντικοποιηθεί ως διαχειρηστής, αναζητείται το id που εισήαγε στο collection products και επιστρέφεται το προϊόν που αντιστοιχεί σε αυτό. Αν βρεθεί τότε γίνεται delete απο το products collection με βαση το id του. Αν δεν βρεθεί, τότε εμφανίζεται ανάλογο μήνυμα αποτυχίας.
> Παραδειγμα εκτέλεσης

![delete product](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/7c2d6c2c6909b60fe924225d61ec073f614ec3b4/images/admin%20/delete%20product/deleteprod.png)

### Ενημέρωση προϊόντος (/updateProduct)

Για να ενημερώσει ένα ή παραπάνω πεδία για ένα συγκεκριμένο προϊόν, ο διαχειρηστής πρέπει να εισάγει το id του προϊόντος που θέλει να ενημερώσει μαζί με το ποια πεδία θέλει να ενημερώσει και πώς (name, category, stock, price), καθώς και το session id του. Μόλις αυθεντικοποιηθεί ως διαχειρηστής, τότε αναζητείται το προϊόν προς ενημέρωση στο collection products και αν βρεθεί ελέγχεται ένα ένα τα πιθανά πεδία αν περιλαμβάνονται στο data και αν ναι τότε τα ενημερώνει με τη νέα τιμή τους. Μετά τις ενημερώσεις, αναζητείται εκ νέου το προϊόν για να τυπωθούν οι αλλαγές που έγιναν σε αυτό. Αν το προϊόν δεν βρεθεί, τότε εμφανίζεται ανάλογο μήνυμα αποτυχίας.
> Παραδειγμα εκτελεσης

![update prod](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/7c2d6c2c6909b60fe924225d61ec073f614ec3b4/images/admin%20/update%20product/updateprod.png)

## Containerization

Για την διαδικασία του containerization, δημιουργούμε ένα φάκελο "project" ο οποίος έχει τη παρακάτω μορφή:

![folder](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/efc2a3e545be1a46f50e6581a0c37deddd90a090/images/container/folder.png)
![project](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/efc2a3e545be1a46f50e6581a0c37deddd90a090/images/container/project.png)
> project folder περιεχόμενα

![flask](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/efc2a3e545be1a46f50e6581a0c37deddd90a090/images/container/flask.png)
> flask folder περιεχόμενα

![data](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/efc2a3e545be1a46f50e6581a0c37deddd90a090/images/container/data.png)
> data folder περιεχόμενα

![dockerfile](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/efc2a3e545be1a46f50e6581a0c37deddd90a090/images/container/dockerfile.png)
> Dockerfile κωδικας

![yml](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/efc2a3e545be1a46f50e6581a0c37deddd90a090/images/container/docker-compose.png)
> yml κωδικας

![prepare data](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/efc2a3e545be1a46f50e6581a0c37deddd90a090/images/container/prepare-data.png)
> prepare_data.py κωδικας

Με την εντολή **"docker-compose build"** χτίζουμε το flask-service 

![build1](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/efc2a3e545be1a46f50e6581a0c37deddd90a090/images/container/build1.png)
![build2](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/efc2a3e545be1a46f50e6581a0c37deddd90a090/images/container/build2.png)

με την ολοκλήρωση του, ενεργοποιούμε το flask service με την εντολή **"docker-compose up -d"**.

![run1](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/efc2a3e545be1a46f50e6581a0c37deddd90a090/images/container/run1.png)
![run2](https://github.com/moonxsugar/Ergasia2_E18130_Nomiki_Parginou/blob/efc2a3e545be1a46f50e6581a0c37deddd90a090/images/container/run2.png)

Αφού ενεργοποιήθηκε με επιτυχία, είμαστε πλέον έτοιμοι για την χρήση του.
