# Exercices CRUD

## Niveau 1

### Aide avant de commencer

Pour les champs _id ou références { $oid: "..." }, vous devrez utiliser les ObjectId réels du jeu de données (extraits depuis tes fichiers JSON ou la base).
Exemple :
```js
db.users.findOne({ _id: ObjectId("68307a086234a1f4a7ed3783") })
```

### Read - Lecture simple

**Objectif** : Comprendre la recherche simple dans une collection.
- Trouver tous les utilisateurs.
```js
db.users.find()
```
- Trouver toutes les catégories.
```js
db.categories.find()
```
- Trouver tous les produits avec un stock supérieur à 50.
```js
db.products.find({
    stock: {$gt: 50}
})
```
- Trouver toutes les commandes dont le statut est *pending*.
```js
db.orders.find({
    status: "pending"
})

```
- Trouver toutes les critiques (*reviews*) d’un produit donné (par exemple avec un _id connu).
```js
db.reviews.find({
    productId: ObjectId('68334f0565f54e1656f1d222')
})
```
### Read - Recherche avancée et projection

**Objectif** : Utiliser filtres plus précis et afficher certains champs seulement.

- Trouver les produits dont le prix est compris entre 20 et 100 euros, afficher uniquement *name*, *price* et *stock*.
```js
db.products.find(
    {price: {$gt: 20, $lt: 100}},
    {
        name: 1,
        price: 1,
        stock: 1,
        _id: 0
    }
)
```
- Trouver les commandes effectuées par un utilisateur donné (utilise un *_id* de user).
```js   
db.orders.find({
     userId: ObjectId('68334f0565f54e1656f1d202')
})
```
- Trouver les reviews où la note est supérieure ou égale à 4, affiche seulement le *comment* et la *rating*.
```js
db.reviews.find(
    {rating: {$gte: 4}},
    {comment: 1,
     _id: 0,
     rating: 1
    } 
)
```
- Trouver les utilisateurs qui sont admins (*isAdmin: true*), afficher leur *name* et *email* uniquement.
```js
db.users.find(
    {isAdmin: true},
    {
     _id: 0,
     name: 1,
     email: 1
    }
)
```
### Create - Insertion

**Objectif** : Ajouter de nouvelles données dans une collection.
- Ajouter un nouvel utilisateur avec un nom, email, mot de passe hashé, et la date de création (aujourd’hui).
```js
db.users.insertMany([
    {
        name: 'Emanuel Bertrand',
        email: 'emanuelbertrand@gmail.com',
        passwordHash: 'cournosql',
        createAt: new Date()
    }
])
```
- Ajouter une nouvelle catégorie (exemple : “Jouets”).
```js
db.categories.insertOne(
   { name: "Jouets"}
)
```
- Ajouter un nouveau produit dans la catégorie “Jouets” avec un stock de 100.
```js
db.products.insertOne({
    name: "Voiture",
    description: 'Innovative Bike featuring criminal technology and Granite construction',
    price: 17.59,
    category: ObjectId('68357890a1716478856c4bd9'),
    stock: 100,
    createDate: new Date()
})
```
- Ajouter une nouvelle commande pour un utilisateur avec au moins un produit et un total calculé.
```js
db.orders.insertOne({
    userId: ObjectId('68334f0565f54e1656f1d1f9'),
     products: [{ productId: ObjectId('68334f0565f54e1656f1d222'), quantity: 1 }],
    total: 20
})
```
- Ajouter une review pour un produit existant par un utilisateur existant.
```js
db.reviews.insertOne({
    comment: 'It is great !',
    userId: ObjectId('68334f0565f54e1656f1d1f1'),
    products: [{ productId: ObjectId('68334f0565f54e1656f1d237')}]
})
```
### Update - Modification partielle

**Objectif** : Modifier un ou plusieurs champs d’un document.
- Mettre à jour le stock d’un produit (par exemple diminuer de 10 unités).
```js
db.products.updateOne(
    {_id: ObjectId('68334f0565f54e1656f1d226')},
    [
        {$set: {stock: 85}}
    ]
)
```

- Modifier le statut d’une commande (par exemple passer de *pending* à *shipped*).
```js
db.orders.updateOne(
    {_id: ObjectId('68334f0565f54e1656f1d257')},
    [
        {$set: {status: 'shipped'}}
    ]
)
```
- Mettre à jour l’email d’un utilisateur.
```js
db.users.updateOne(
    {_id: ObjectId('68334f0565f54e1656f1d1fc')},
    [
        {$set: {email: 'matilde@gmail.com'}}
    ]
)
```
- Ajouter un champ *discount* dans un produit (par exemple 10%).
```js
db.products.updateOne(
    {_id: ObjectId('68334f0565f54e1656f1d226')},
    [{$set: {discount: '10%'}}]
)
```
- Mettre à jour la note et le commentaire d’une *review* existante.
```js
db.reviews.updateOne(
    {_id: ObjectId('68334f0565f54e1656f1d276')},
    [{$set: {rating: 6, comment: 'Change the comment.'}}]
)
```
### Update - Remplacement complet

**Objectif** : Remplacer entièrement un document.

- Remplacer une catégorie entière par une nouvelle, par exemple changer le nom de “Livres” en “Littérature”.
```js
db.categories.replaceOne(
   {name: "Livres"},
   {name: "Littérature"}
)
```
- Remplacer entièrement une *review* (avec un nouveau commentaire, rating, etc.).
```js
db.reviews.updateOne(
    {_id: ObjectId('68334f0565f54e1656f1d276')},
    [{$set: {comment: 'New comment', rating: 12}}]
)
```

### Delete - Suppression

**Objectif** : Supprimer des documents.
- Supprimer une catégorie “Beauté”.
```js
db.categories.deleteOne({
    name: "Beauté"
}) 
```
- Supprimer un produit avec un stock à zéro.
```js
db.products.deleteOne({
    stock: 0
})
```
- Supprimer toutes les commandes dont le statut est *pending*.
```js
db.orders.deleteMany({
    status: "pending"
})
```
- Supprimer toutes les reviews d’un utilisateur spécifique.
```js
db.reviews.deleteMany({
    _id: ObjectId('68334f0565f54e1656f1d276')
})
```
- Supprimer un utilisateur spécifique.
```js
db.users.deleteOne({_id: ObjectId('68334f0565f54e1656f1d1fc')})
```

### Bonus : Combiner plusieurs opérations

**Objectif** : Apprendre à faire des recherches plus des mises à jour complexes.

- Trouver tous les produits d'une' catégorie et augmenter leur prix de 5%.
```js

```
- Trouver toutes les commandes d’un utilisateur donné et changer leur statut à “paid”.
```js

```
- Supprimer toutes les reviews ayant une note inférieure à 3.
```js

```

## Niveau 2

### Création avec références multiples

**Consigne** : Créer une nouvelle commande pour un utilisateur donné (USER_ID), contenant 2 produits différents (PRODUCT_ID1 et PRODUCT_ID2) avec des quantités respectives 3 et 1. Calcule le total en additionnant prix * quantité des produits. Mets le statut à "pending" et remplis une adresse factice.

### Mise à jour conditionnelle multiple

**Consigne** : Pour tous les produits dans la catégorie “Électronique” (CATEGORY_ID à remplacer), augmenter le prix de 15% uniquement pour les produits dont le stock est supérieur à 10.

### Ajout d’un élément dans un tableau

**Consigne** : Ajouter une nouvelle image URL "http://example.com/newimage.jpg" à la liste des images d’un produit (PRODUCT_ID).

### Suppression d’un élément dans un tableau

**Consigne** : Supprimer toutes les commandes de l’utilisateur (USER_ID) où le statut est "pending".

### Mise à jour d’un élément dans un tableau d’objets

**Consigne** : Dans une commande (ORDER_ID), modifier la quantité du produit PRODUCT_ID à 5 (dans le tableau products).

### Remplacement complet d’un document

**Consigne** : Remplacer entièrement une catégorie (CATEGORY_ID) par une nouvelle catégorie nommée "Sport".

### Suppression conditionnelle multiple

**Consigne** : Supprimer toutes les reviews dont la note est inférieure à 3.

### Mise à jour avec agrégation et date actuelle

**Consigne** : Pour un produit (PRODUCT_ID), modifier sa catégorie à CATEGORY_ID et mettre à jour la date de création createdAt à la date et heure actuelle.

### Mise à jour incrémentale multiple

**Consigne** : Pour tous les utilisateurs qui ne sont pas admin (isAdmin: false), ajouter un champ loginCount initialisé à 0.