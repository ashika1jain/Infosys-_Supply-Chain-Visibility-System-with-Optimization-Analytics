# DAX Measures Documentation

## Supply Chain Visibility System with Optimization Analytics

**Author:** Ashika Jain\
**Tool:** Microsoft Power BI

This document records the key DAX measures used across the project's
four milestones.

> **Data limitation:** The original DataCo dataset does not contain all
> warehouse, inventory, supplier, carrier, and operational-cost fields.
> Some metrics are simulated or derived from documented assumptions and
> should not be interpreted as verified real-world operational records.

## Milestone 1: Sales and Inventory Movement

### Total Orders

``` dax
Total Orders =
DISTINCTCOUNT(FactOrders[Order Id])
```

Counts distinct orders, avoiding duplicate counting when an order
contains multiple item rows.

### Total Order Items

``` dax
Total Order Items =
COUNTROWS(FactOrders)
```

Counts rows in the order fact table.

### Total Quantity

``` dax
Total Quantity =
SUM(FactOrders[Order Item Quantity])
```

Sums order-item quantity.

### Total Sales

``` dax
Total Sales =
SUM(FactOrders[Sales])
```

Sums recorded sales.

### Total Profit

``` dax
Total Profit =
SUM(FactOrders[Order Profit Per Order])
```

Sums profit values recorded in the source data.

### Average Order Value

``` dax
Average Order Value =
DIVIDE([Total Sales], [Total Orders])
```

Sales per distinct order.

### Average Quantity per Order

``` dax
Average Quantity per Order =
DIVIDE([Total Quantity], [Total Orders])
```

Average quantity per distinct order.

### Total Discount

``` dax
Total Discount =
SUM(FactOrders[Order Item Discount])
```

Sums order-item discount values.

### Discount Rate

``` dax
Discount Rate =
DIVIDE(
    [Total Discount],
    [Total Sales] + [Total Discount]
)
```

Calculates discount as a proportion of sales plus discount, following
the project's defined calculation.

### Profit Margin

``` dax
Profit Margin =
DIVIDE([Total Profit], [Total Sales])
```

Profit as a proportion of sales.

### Average Profit per Order

``` dax
Average Profit per Order =
DIVIDE([Total Profit], [Total Orders])
```

Average recorded profit per distinct order.

### Units per Product

``` dax
Units per Product =
DIVIDE(
    [Total Quantity],
    DISTINCTCOUNT(DimProduct[Product Card Id])
)
```

Average quantity per distinct product in the current filter context.

### Sales per Unit

``` dax
Sales per Unit =
DIVIDE([Total Sales], [Total Quantity])
```

Sales per unit of quantity.

### Orders per Product

``` dax
Orders per Product =
DIVIDE(
    [Total Orders],
    DISTINCTCOUNT(DimProduct[Product Card Id])
)
```

Distinct orders per distinct product in the current filter context.

## Milestone 2: Inventory and Delivery Analysis

### Total Inventory

``` dax
Total Inventory =
VAR SelectedDate =
    MAX(DimDate[Date])
RETURN
SUMX(
    VALUES(DimProduct[Product Card Id]),
    VAR LastProductDate =
        CALCULATE(
            MAX(FactInventory[Inventory Date]),
            REMOVEFILTERS(DimDate),
            FactInventory[Inventory Date] <= SelectedDate
        )
    RETURN
        CALCULATE(
            MAX(FactInventory[Closing Inventory]),
            REMOVEFILTERS(DimDate),
            FactInventory[Inventory Date] = LastProductDate
        )
)
```

Returns each product's latest available closing inventory on or before
the selected date, then sums those balances. This is a point-in-time
snapshot, not a sum of historical daily balances.

### Total Products

``` dax
Total Products =
DISTINCTCOUNT(DimProduct[Product Card Id])
```

Counts distinct products in the current filter context.

### Slow-Moving Products

``` dax
Slow-Moving Products =
CALCULATE(
    DISTINCTCOUNT(ProductDemand[Product Card Id]),
    ProductDemand[Slow Moving] = "Slow Moving"
)
```

Counts products classified as slow-moving. The project classification is
based on average daily demand below one unit.

### Inventory Turnover Proxy

``` dax
Inventory Turnover Proxy =
DIVIDE(
    [Total Quantity],
    AVERAGEX(
        ALLSELECTED(DimDate[Date]),
        [Total Inventory]
    )
)
```

Compares total quantity with average inventory snapshots over selected
dates. This is a project-specific proxy, not traditional accounting
inventory turnover, which normally uses cost of goods sold divided by
average inventory.

### On-Time Deliveries

``` dax
On-Time Deliveries =
CALCULATE(
    DISTINCTCOUNT(FactOrders[Order Id]),
    FactOrders[Delivery Status] = "Shipping on time"
)
```

Counts distinct orders marked `Shipping on time`.

### Delayed Deliveries

``` dax
Delayed Deliveries =
CALCULATE(
    DISTINCTCOUNT(FactOrders[Order Id]),
    FactOrders[Delivery Status] = "Late delivery"
)
```

Counts distinct orders marked `Late delivery`.

### On-Time Delivery %

``` dax
On-Time Delivery % =
DIVIDE(
    [On-Time Deliveries],
    [On-Time Deliveries] + [Delayed Deliveries]
)
```

On-time orders divided by on-time plus late orders. Advance-shipping and
canceled statuses are excluded from the denominator.

### Average Actual Shipping Days

``` dax
Average Actual Shipping Days =
AVERAGE(FactOrders[Days for shipping (real)])
```

Average actual shipping duration in days.

### Average Scheduled Shipping Days

``` dax
Average Scheduled Shipping Days =
AVERAGE(FactOrders[Days for shipment (scheduled)])
```

Average scheduled shipping duration in days.

### Average Shipping Delay

``` dax
Average Shipping Delay =
[Average Actual Shipping Days]
    - [Average Scheduled Shipping Days]
```

Difference between average actual and scheduled shipping durations. A
positive result means actual shipping took longer on average.

## Milestone 3: Supplier and Transportation Analysis

Supplier, carrier, route, and shipment-cost fields in this milestone
were generated or derived for the project.

### Total Shipment Orders

``` dax
Total Shipment Orders =
DISTINCTCOUNT(FactShipment[Order Id])
```

Counts distinct orders represented in the shipment table.

### Fulfilled Orders

``` dax
Fulfilled Orders =
CALCULATE(
    DISTINCTCOUNT(FactShipment[Order Id]),
    FactShipment[Delivery Status] <> "Shipping canceled"
)
```

Counts distinct orders not marked as canceled.

### Fulfillment Rate

``` dax
Fulfillment Rate =
DIVIDE([Fulfilled Orders], [Total Shipment Orders])
```

Share of shipment orders not marked as canceled.

### Total Transportation Cost

``` dax
Total Transportation Cost =
SUM(FactShipment[Shipment Cost])
```

Sums simulated shipment costs, not actual carrier expenditure.

### Average Shipment Cost

``` dax
Average Shipment Cost =
AVERAGE(FactShipment[Shipment Cost])
```

Average simulated cost per shipment row.

### On-Time Rate

``` dax
On-Time Rate =
DIVIDE(
    CALCULATE(
        DISTINCTCOUNT(FactShipment[Order Id]),
        FactShipment[Delivery Status] = "Shipping on time"
    ),
    CALCULATE(
        DISTINCTCOUNT(FactShipment[Order Id]),
        FactShipment[Delivery Status] <> "Shipping canceled"
    )
)
```

Share of non-canceled distinct orders marked `Shipping on time`. Unlike
the Milestone 2 measure, this denominator includes advance-shipping
orders and excludes only canceled orders.

## Milestone 4: Warehouse Efficiency

Warehouse assignments and certain operational metrics were simulated or
derived for analysis.

### Warehouse Total Orders

``` dax
Warehouse Total Orders =
DISTINCTCOUNT(FactWarehouse[Order Id])
```

Counts distinct orders in the warehouse fact table.

### Total Shipped Qty

``` dax
Total Shipped Qty =
SUM(FactWarehouse[Order Item Quantity])
```

Sums order-item quantities represented in the warehouse table.

### Total Ordered Qty

``` dax
Total Ordered Qty =
SUM(FactWarehouse[Order Item Quantity])
```

Uses the same quantity column for the project's ordered-quantity visual.
The model does not independently distinguish warehouse-ordered quantity
from shipped quantity.

### Warehouse Capacity Utilization

``` dax
Warehouse Capacity Utilization =
AVERAGE(FactWarehouse[Capacity Utilization %])
```

Averages row-level capacity utilization percentages. Percentages should
not be summed.

### Warehouse On-Time Delivery %

``` dax
Warehouse On-Time Delivery % =
AVERAGE(FactWarehouse[On-Time Delivery Flag])
```

Averages the numeric flag: `1` for `Shipping on time`, `0` for
`Late delivery`, and blank for other statuses. Advance-shipping and
canceled rows are excluded from the denominator.

### Warehouse Picking Accuracy %

``` dax
Warehouse Picking Accuracy % =
AVERAGE(FactWarehouse[Picking Accuracy %])
```

Averages simulated row-level picking-accuracy values.

### Average Processing Hours

``` dax
Average Processing Hours =
AVERAGE(FactWarehouse[Processing Hours])
```

Average estimated processing time in hours.

### Warehouse Operating Cost

``` dax
Warehouse Operating Cost =
SUM(FactWarehouse[Operating Cost])
```

Sums simulated operating-cost values, not actual warehouse expenditure.

## Modeling and Interpretation Notes

-   Use `DISTINCTCOUNT` for order counts because one order can contain
    multiple order-item rows.
-   Use `AVERAGE` for row-level percentage fields such as capacity
    utilization and picking accuracy. Summing percentages is misleading.
-   Inventory snapshots are point-in-time values. Adding daily closing
    balances across dates does not produce a single inventory balance.
-   The Milestone 2 and Milestone 3 on-time measures have different
    denominators and should not be treated as identical.
-   Clearly label simulated metrics, including warehouse assignments,
    inventory simulation, supplier/carrier assignments, shipment costs,
    picking accuracy, processing hours, and operating costs.
-   Filter behavior depends on model relationships. Shared dimensions
    such as `DimDate`, `DimProduct`, and `DimWarehouse` filter related
    fact tables.

## Dataset Source

DataCo SMART SUPPLY CHAIN FOR BIG DATA ANALYSIS:\
https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis

------------------------------------------------------------------------
