# Basic E-Commerce Order Data Processing

## Project Overview

This project processes customer, product, and order CSV files for a
simple e-commerce business. The goal is to clean the data, combine
related information, calculate order amounts, and generate useful
reports.

## Input Files

Place these files in the `raw/` folder:

-   `customers.csv` -- customer details (`customer_id`, `customer_name`,
    `city`)
-   `products.csv` -- product details (`product_id`, `product_name`,
    `price`)
-   `orders.csv` -- order details (`order_id`, `customer_id`,
    `product_id`, `quantity`, `status`)

## Data Model

-   **Fact table:** `Orders`
-   **Dimension tables:** `Customers` and `Products`
-   **Primary keys:** `Customers.customer_id`, `Products.product_id`,
    `Orders.order_id`
-   **Foreign keys:** `Orders.customer_id` references
    `Customers.customer_id`; `Orders.product_id` references
    `Products.product_id`

**Grain:** One row in the Orders fact table represents one product line
in an order, for a customer, including its quantity and calculated
amount.

``` text
Customers (1) ──── (*) Orders (*) ──── (1) Products
```

## Processing Steps

1.  Read the three CSV files.
2.  Check the data and remove duplicate records.
3.  Handle missing or invalid values and correct column data types.
4.  Join orders with customer and product information using their IDs.
5.  Calculate `order_amount = quantity * price`.
6.  Save the cleaned and combined output.

## AWS Workflow

  -----------------------------------------------------------------------
  AWS service                         Purpose
  ----------------------------------- -----------------------------------
  Amazon S3                           Stores original files in `raw/` and
                                      cleaned output in `processed/`.

  AWS Glue                            Reads, cleans, joins, and
                                      transforms the CSV data.

  AWS Glue Data Catalog               Stores table metadata for querying.

  Amazon Athena                       Runs SQL queries on the processed
                                      data and creates reports.

  AWS IAM                             Controls access permissions for the
                                      AWS services.
  -----------------------------------------------------------------------

## Example Output Columns

`order_id`, `customer_id`, `customer_name`, `city`, `product_id`,
`product_name`, `quantity`, `price`, `order_amount`, `status`

## Example SQL Reports

-   Display orders with customer and product names.
-   Calculate total order amount for each customer.
-   Count orders by status.
-   Find the product with the highest sales.

## Expected Result

The final processed dataset contains clean order information, customer
and product details, and the calculated order amount. It can then be
queried in Athena to support simple business reporting.

## Notes

-   Keep the original CSV files unchanged in the `raw/` folder.
-   Store transformed files separately in the `processed/` folder.
-   Confirm that IDs match across the three input files before joining.
-   Check for duplicate IDs, missing values, invalid quantities, and
    invalid prices before producing reports.
