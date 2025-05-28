# Exercices Queries

## Niveau 1

- Listez tous les produits disponibles (stock > 0).

```js
 db.products.find({ stock: { $gt: 0 } })
```

- Affichez les commandes dont le statut est *"paid"*.
```js
 db.orders.find({ status: "paid"})
```

- Affichez les produits dont le prix est strictement supérieur à 100€.
```js
db.products.find({ price: { $gt: 100 } })
```
- Affichez les produits dont le prix est inférieur à 20€.
```js
db.products.find({ price: { $lt: 20}})
```

- Affichez les commandes dont le statut est "*paid"* ou *"shipped"*.
```js
db.orders.find({
    $or: [
        { status: "paid" },
        { status: "shipped" }
    ]
     })
```


- Listez les produits qui ont un *stock supérieur à 10* et un *prix inférieur à 50€*.
```js
db.products.find({
    $and: [
        { stock: {$gt: 10}},
        { stock: {$lt: 50}}
    ]
})
```


- Listez les utilisateurs qui sont administrateurs ou ont un email se terminant par @gmail.com.
```js
db.users.find( { $or: [ { isAdmin: true }, { email: { $regex: "@gmail.com$", $options: "i" } } ] } )
 
```


## Niveau 2

- Trouvez tous les produits qui ont un stock entre 10 et 50 unités et un prix entre 20€ et 150€.
```js
    db.products.find({
        $and: [
            {stock: {$gt: 10, $lt: 50}},
            {price: {$gt: 20, $lt: 150}}
        ]
    })

```
- Affichez toutes les commandes dont le total est supérieur à 300€ et le statut est *"paid"* ou *"shipped"*.
```js
db.orders.find({
    $and: [
        {total: {$gt: 300}},
        {$or: [
            {status: "paid"},
            {status: "shipped"}
        ]}
    ]
})

db.orders.find({
    $and: [
        {total: {$gt: 300}},
        {status: {$in: ["paid","shipped"]}}
    ]
})
```
⚠️ **Remarque** : Si c'est total c'est un seul champ, si c'est status, c'est deux champs car status peux avoir "paid" ou "shiped".

- Listez les utilisateurs qui ne sont pas admin et ont un email contenant gmail.com.
```js
db.users.find({
    $and: [
        {isAdmin: false},
        {email: {$regex: "@gmail.com$"}}
    ]
})
```
- Récupèrez les produits dont la catégorie appartient à une liste spécifique d'_id (ex. catégories "Vêtements" ou "Livres"), en utilisant *$in*.
```js
db.products.find({
    category: {$in: [ObjectId('68334f0565f54e1656f1d205'), ObjectId('68334f0565f54e1656f1d206')]}
})
```
- Trouvez toutes les commandes dont la ville de livraison est "Marshallshire" et le statut est "paid".
```js
db.orders.find({
    $and: [
        {"address.city": "Marshallshire"},
        {status: "paid"}
    ]
})
```
- Trouvez tous les utilisateurs qui sont admin ou ont un compte créé il y a moins de 10 jours.
```js
db.users.find({
    $or: [
        {isAdmin: true},
        {createdAt: {$lt: 10} }
    ]
})
```
## Niveau 3 (bonus)

- Récupèrez les produits avec leur nom de catégorie (jointure entre products et categories).
```js

```
- Jointure entre *orders* et *users* pour afficher :
    - l’email et le nom de l’utilisateur
    - le total et la date de la commande
```js

```
- Affichez combien de commandes ont été *paid*, *shipped*, *pending*, etc.
```js

```
- Calculez la somme des ventes (total) par jour (extraire la date depuis createdAt).
```js

```
- Détaillez les produits vendus dans *orders.products* puis calculez combien de fois chaque produit a été commandé au total.
```js

```