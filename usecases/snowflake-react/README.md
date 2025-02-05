# Getting Started with Dozer react example

```yaml
app_name: snowflake-tutorial
connections:
  - config: !Snowflake
      server: "{{SN_SERVER}}"
      port: 443
      user: "{{SN_USER}}"
      password: "{{SN_PASSWORD}}"
      database: "{{SN_DATABASE}}"
      schema: PUBLIC
      warehouse: "{{SN_WAREHOUSE}}"
      driver: "SnowflakeDSIIDriver"
      role: "{{SN_ROLE}}"
    name: sn_data

sources:
  - name: customers
    connection: !Ref sn_data
    table_name: CUSTOMERS
    columns:

  - name: orders
    connection: !Ref sn_data
    table_name: ORDERS
    columns:

sql: |
  SELECT C_CUSTKEY, C_NAME, COUNT(O_CUSTKEY), AVG(O_TOTALPRICE)
  INTO customers_orders
  FROM customers
  JOIN orders on C_CUSTKEY = O_CUSTKEY
  GROUP BY C_CUSTKEY, C_NAME;

  SELECT C_CUSTKEY, C_NAME, C_ADDRESS, C_NATIONKEY, C_PHONE, C_ACCTBAL, C_MKTSEGMENT, C_COMMENT
  INTO customers_data
  FROM customers;

  SELECT O_ORDERKEY, O_CUSTKEY, O_ORDERSTATUS, O_TOTALPRICE, O_ORDERDATE, O_ORDERPRIORITY, O_CLERK, O_SHIPPRIORITY, O_COMMENT
  INTO orders_data
  FROM orders;

endpoints:
  - name: customers
    path: /customers
    table_name: customers_data
    index:
      primary_key:
        - C_CUSTKEY

  - name: orders
    path: /orders
    table_name: orders_data
    index:
      primary_key:
        - O_ORDERKEY

  - name: customers_orders
    path: /customers_orders
    table_name: customers_orders
    index:
      primary_key:
        - C_CUSTKEY
        - C_NAME
```

## Available Scripts

In the project directory, you can run:

### `yarn start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in your browser.

The page will reload when you make changes.\
You may also see any lint errors in the console.
