# Exercices :

1. Récupérer le classement de chaque client en fonction du montant total de ses paiements (à l'aide de l'expression régulière CTE sur `olist_order_payments_dataset`, puis en appelant la fonction `RANK()`).

2. Pour chaque commande, afficher le montant du paiement et le montant moyen des commandes du client (à l'aide des fonctions `AVG() OVER()` sur `olist_orders_dataset` et `olist_order_payments_dataset`).

3. Calculer la différence en jours entre deux commandes consécutives d'un même client (à l'aide de la fonction `LAG()` sur `order_purchase_timestamp`).
