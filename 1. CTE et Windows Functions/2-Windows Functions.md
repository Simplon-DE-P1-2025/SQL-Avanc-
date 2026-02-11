# Fonctions de Fenêtrage (Window Functions)

Les fonctions de fenêtrage effectuent des calculs sur un ensemble de lignes associées à la ligne actuelle, mais ne les regroupent pas (contrairement à `GROUP BY`).

## 1. RANK() et DENSE_RANK()
```sql
-- Classer les produits par chiffre d'affaires dans chaque catégorie
WITH product_revenue AS (
    SELECT
        oi.product_id,
        p.product_category_name,
        SUM(oi.price + oi.freight_value) AS revenue
    FROM olist_order_items_dataset oi
    JOIN olist_products_dataset p ON p.product_id = oi.product_id
    GROUP BY oi.product_id, p.product_category_name
)
SELECT
    product_id,
    product_category_name,
    revenue,
    RANK() OVER (
        PARTITION BY product_category_name
        ORDER BY revenue DESC
    ) AS revenue_rank
FROM product_revenue;
```

## 2. LAG() et LEAD()
Accéder à la ligne précédente ou suivante.
```sql
-- Comparer le paiement d'une commande avec la précédente pour un même client
WITH customer_payments AS (
    SELECT
        o.customer_id,
        o.order_id,
        o.order_purchase_timestamp,
        op.payment_value
    FROM olist_orders_dataset o
    JOIN olist_order_payments_dataset op ON op.order_id = o.order_id
)
SELECT
    customer_id,
    order_id,
    order_purchase_timestamp,
    payment_value,
    LAG(payment_value) OVER (
        PARTITION BY customer_id
        ORDER BY order_purchase_timestamp
    ) AS prev_payment_value
FROM customer_payments;
```

## 3. Sommes cumulées
```sql
WITH daily_revenue AS (
    SELECT
        CAST(DATE_TRUNC('day', o.order_purchase_timestamp) AS DATE) AS order_day,
        SUM(oi.price + oi.freight_value) AS revenue
    FROM olist_orders_dataset o
    JOIN olist_order_items_dataset oi ON oi.order_id = o.order_id
    GROUP BY 1
)
SELECT
    order_day,
    revenue,
    SUM(revenue) OVER (ORDER BY order_day) AS running_total
FROM daily_revenue;
```
