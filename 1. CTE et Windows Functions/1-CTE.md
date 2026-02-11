# Les CTE (Common Table Expressions)

Les expressions de table communes (CTE) permettent de définir des tables temporaires, ce qui rend les requêtes plus lisibles et modulaires.

## 1. Syntaxe avec WITH
```sql
WITH customer_orders AS (
    SELECT
        o.customer_id,
        COUNT(*) AS nb_orders
    FROM olist_orders_dataset o
    GROUP BY o.customer_id
)
SELECT
    c.customer_unique_id,
    c.customer_city,
    co.nb_orders
FROM olist_customers_dataset c
JOIN customer_orders co ON c.customer_id = co.customer_id
ORDER BY co.nb_orders DESC;
```

- **Lisibilité :** Évite les sous-requêtes imbriquées difficiles à lire.

- **Réutilisabilité :** Une même CTE peut être appelée plusieurs fois au sein d'une même requête.