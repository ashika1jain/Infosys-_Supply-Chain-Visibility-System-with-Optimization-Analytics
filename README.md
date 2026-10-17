# Supply Chain Visibility System with Optimization Analytics

## Project Overview

This project uses Power BI to analyze supply chain operations, including sales performance, inventory movement, delivery performance, supplier performance, transportation costs, and warehouse efficiency.

The objective is to transform supply chain data into interactive dashboards that help users monitor key performance indicators (KPIs), identify trends, and compare operational performance.

## Objectives

- Analyze sales, order volume, and product demand.
- Monitor inventory movement and inventory performance.
- Evaluate delivery status and shipping delays.
- Compare supplier performance and transportation costs.
- Analyze warehouse capacity utilization, picking accuracy, and processing time.
- Build interactive dashboards for operational and executive-level reporting.

## Tools and Technologies

- Microsoft Power BI
- Power Query for data cleaning and transformation
- DAX for KPI calculations
- Data modeling using fact and dimension tables
- GitHub for project documentation and version control

## Dataset

**DataCo SMART SUPPLY CHAIN FOR BIG DATA ANALYSIS**

Dataset source: https://www.kaggle.com/datasets/shashwatwork/dataco-smart-supply-chain-for-big-data-analysis

The dataset contains order, product, customer, sales, shipping, and delivery-related information.

**Data limitation:** The original dataset does not provide all the warehouse, inventory, supplier, carrier, and operational cost information needed for the project. Some fields and metrics were therefore simulated or derived using documented assumptions.

## Project Milestones

### Milestone 1: Inventory Movement & Sales Performance
- Total orders, sales, quantity, and profit analysis
- Sales and quantity trends over time
- Category-wise performance analysis

### Milestone 2: Inventory and Delivery Analysis
- Inventory movement and inventory trend analysis
- Inventory turnover proxy
- Delivery status distribution
- Regional delivery performance
- Actual versus scheduled shipping time analysis

### Milestone 3: Supplier & Transportation Analysis
- Supplier-wise shipment volume and quality score
- Carrier-wise transportation cost
- Route-wise transportation cost
- Fulfillment and on-time delivery metrics

### Milestone 4: Warehouse Efficiency & Executive Overview
- Warehouse capacity utilization
- Picking accuracy and processing time
- Warehouse-wise order and inventory analysis
- Zone-wise order distribution
- Executive overview and dashboard performance comparison

## Data Modeling

The Power BI model uses fact and dimension tables to organize the data and support filtering across report pages.

Key tables include:
- FactOrders
- FactInventory
- FactShipment
- FactWarehouse
- DimProduct
- DimCustomer
- DimGeography
- DimShipping
- DimDate
- DimWarehouse

## Key Performance Indicators

- Total Orders
- Total Sales
- Total Profit
- Profit Margin
- Total Inventory
- Inventory Turnover Proxy
- On-Time Delivery Percentage
- Fulfillment Rate
- Total Transportation Cost
- Warehouse Capacity Utilization
- Warehouse Picking Accuracy
- Average Processing Hours

## Dashboards

Screenshots of the report pages are available in the `screenshots/` folder.

The complete interactive report can be explored by opening the Power BI file in Microsoft Power BI Desktop, subject to the availability of the required data sources.

## Important Assumptions and Limitations

- Warehouse assignments are simulated because actual warehouse identifiers were not available in the source dataset.
- Inventory levels and replenishment are based on a simplified inventory simulation.
- Supplier, carrier, transportation cost, and certain warehouse operational metrics use simulated or derived values.
- The inventory turnover metric is a proxy, not the traditional accounting measure based on cost of goods sold and average inventory.
- Results involving simulated fields should be interpreted as analytical demonstrations rather than verified real-world operational outcomes.

## Learning Outcomes

- Data cleaning and transformation using Power Query
- Data modeling and relationship management
- DAX measure development
- Interactive dashboard design
- KPI analysis and performance monitoring
- Documentation of assumptions and data limitations

## Author

Ashika Jain

Project: Supply Chain Visibility System with Optimization Analytics
