# Day 1. Data Validation and Cleaning 
## Data Cleaning Decisions
* Removed null product_id rows 41 records
* Standardized text fields
* Converted mixed date formats
* Fixed numeric type inconsistencies
* Removed exact duplicates
* Grouped duplicates order_id + product_id
* Fixed referential integrity order_id + customer_id

507 registros se agruparon debido a que algunos products_id se repetian 
en order_id, ayudando a disminuir registros duplicados pero sin afectar 
las cantidades y el revenue calculado. 

Se arreglo la integridad referencial entre order_id + customer_id
debido a que una order_id referenciaba a varios customer_id, 
se duplico la consulta, se agrupo order_id por MAX(customer_id), posteriormente 
se realizo una unión para estandarizar la tabla de hechos. 

54 records with invalid or null order dates were removed from fact_sales,
representing approximately 1% of total observations.
Given the low proportion and the importance of temporal integrity,
removal was preferred over artificial date imputation.


| Métrica              | Valor |
| -------------------- | ----- |
| Registros originales | 5020  |
| Registros filtrados  | 41+54 |
| Duplicados agrupados | 507   |
| Nulos corregidos     | 0     |
| -------------------- | ----- |
| Registros finales    | 4418  |

# Day 2. Data Model in Power Pivot 

Creation of star schema model with: 
* fact sales
* dim product
* dim customers
* dim dates

Creation of table dim date with range from dates in the column order date of fact sales table.

Validation of cardinality one to many from dimension tables to fact table.

Creation of measures in Pwer Pivot: 
* Total Revenue
* Total Customers
* Total Orders
* Total Units Sold 
* Average Order Value 

Creation of Dashboard Executive Overview showing visualizations like: 
* Revenue by Month 
* Revenue by Category 
* Top 5 Products by Revenue

Slicers: 
* Category
* City
* Supplier
* Customer Segment
Connection of slicers to all pivot tables to get more interactivity.

# Day 3. Creation of Basket Base Table  
Use of merge inner join in Power Query to create Basket Base table with some transformations of fact sales table

* Duplicate fact_sales table with only: order_id, product_id and product_name
* Rename Table as basket_base 
* Duplicate basket_base to make merge inner join and find Pair Keys. 
* Filter duplicate values and mirror values 
* Final result, Basket Base Table: | order_id | product_A | product_B | Pair_Key |
* Validation of results with pivot table and count values order_id by Pair_key 

# Day 4. Creation of support, confidence, lift 

* Support: ¿Qué tan frecuente aparece este par?
    Support = veces que aparece el par / total órdenes
* Confidence: Si alguien compra Mouse, ¿qué tan probable es que compre Keyboard?
    Confidence = veces A+B / veces A
* Lift: ¿La relación es realmente fuerte o solo casual?
    Lift = Confidence / probabilidad de B

The pair Usb Hub | Webcam appears in 5.51% of all orders,
making it one of the strongest cross-selling opportunities in the dataset.

