# 🗄️ Artworks of Rotorua Inventory Database

**Course:** CIT 505, Information Technology in Business · Assessment 2

## Scenario
Artworks of Rotorua is a Māori arts and crafts business selling to locals and tourists. Manual inventory tracking risks duplicated data and stock inconsistencies, so I designed a relational database in **MySQL Workbench** to manage suppliers and stock, and to track sold and unsold items.

## Database design
Two tables in a **one-to-many** relationship (one supplier, many stock items):

```sql
CREATE DATABASE artworks_rotorua;
USE artworks_rotorua;

CREATE TABLE suppliers (
    supplier_id     INT PRIMARY KEY AUTO_INCREMENT,
    supplier_name   VARCHAR(100),
    iwi_affiliation VARCHAR(100),
    contact_name    VARCHAR(100),
    contact_email   VARCHAR(100),
    product_type    VARCHAR(100)
);

CREATE TABLE stock_items (
    item_code        INT PRIMARY KEY AUTO_INCREMENT,
    supplier_id      INT,
    description      TEXT,
    price            DECIMAL(10,2),
    markup           DECIMAL(5,2),
    commission_sale  BOOLEAN,
    date_into_stock  DATE,
    date_sold        DATE,
    FOREIGN KEY (supplier_id) REFERENCES suppliers(supplier_id)
);
```

## Sample data
Three suppliers (wood carving, traditional weaving, greenstone jewellery) and three stock items (a carved tiki, a woven kete, a greenstone pendant), inserted with `INSERT INTO` statements.

## Business queries

**Items currently available for sale** (unsold stock joined to supplier):
```sql
SELECT item_code, supplier_name, description, price
FROM stock_items
JOIN suppliers
  ON stock_items.supplier_id = suppliers.supplier_id
WHERE date_sold IS NULL;
```

**Items sold in the last month:**
```sql
SELECT supplier_name, description, price, date_sold
FROM stock_items
JOIN suppliers
  ON stock_items.supplier_id = suppliers.supplier_id
WHERE date_sold >= DATE_SUB(CURDATE(), INTERVAL 1 MONTH);
```

## Entity Relationship Diagram
`suppliers (supplier_id PK)` ──< `stock_items (item_code PK, supplier_id FK)`

Supplier details are stored once and referenced by every item, which improves integrity and reduces redundancy.

## UI / UX and accessibility
I wireframed a staff-facing inventory screen with product search, supplier name, description, price, stock status, previous/next navigation, and an **optional audio playback** button so tourists can hear about Māori cultural products.

- **UX principles:** simplicity, consistency, clear navigation and feedback
- **Accessibility:** readable text, clear labels (screen-reader friendly), high contrast, large buttons, guided by **WCAG 2.1**

## Benefits identified
Central inventory records, better supplier and commission tracking, quick reporting (unsold stock, recent sales), and a scalable base for online growth.

**Tools / topics:** MySQL Workbench · SQL (DDL, DML, JOINs, date functions) · primary/foreign keys · ERD · wireframing · UX · WCAG

---

[⬅ Back to my profile](../README.md)
