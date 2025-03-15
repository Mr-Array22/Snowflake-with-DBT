# Snowflake Data Project

A dbt (data build tool) project for transforming and modeling data in Snowflake.

## Project Overview

This project implements a modern data transformation workflow using dbt with Snowflake as the data warehouse. It follows a layered approach to data modeling:

1. **Source Layer**: Raw data from the finance_db database
2. **Staging Layer**: Cleaned and standardized data models
3. **Marts Layer**: Business-level aggregations and metrics

## Project Structure

```
snowflake_data_project/
├── models/                    # Data transformation models
│   ├── staging/               # Staging models (views)
│   │   ├── stg_customers.sql
│   │   ├── stg_orders.sql
│   │   ├── stg_products.sql
│   │   └── stg_order_items.sql
│   ├── marts/                 # Business-level models (tables)
│   │   └── fct_daily_orders.sql
│   └── sources.yml            # Source definitions
├── tests/                     # Data quality tests
│   └── snowflake.yml          # Test configurations
├── macros/                    # Reusable SQL macros
├── snapshots/                 # Slowly changing dimension snapshots
├── seeds/                     # Static data files
├── analyses/                  # Ad-hoc analytical queries
├── dbt_project.yml            # Project configuration
└── README.md                  # Project documentation
```

## Data Flow

```
Raw Data Sources                Staging Layer                 Marts Layer
(Snowflake Tables)              (Views)                       (Tables)
+------------------+            +------------------+          +------------------+
| raw_data.customers|  ------>  | stg_customers    |  \       |                  |
+------------------+            +------------------+   \      |                  |
                                                        ----> | fct_daily_orders |
+------------------+            +------------------+   /      |                  |
| raw_data.orders  |  ------>  | stg_orders       |  /       |                  |
+------------------+            +------------------+          +------------------+
                                       |
+------------------+            +------------------+
| raw_data.products |  ------>  | stg_products     |
+------------------+            +------------------+
                                       |
+------------------+            +------------------+
| raw_data.order_items| -----> | stg_order_items  |
+------------------+            +------------------+
```

## Data Sources

The project uses the following data sources from the `finance_db.raw` schema:

- `customers`: Customer information
- `orders`: Order transactions
- `products`: Product catalog
- `order_items`: Line items for each order

## Model Layers

### Staging Models

The staging layer provides a clean, consistent interface to the raw data:

- `stg_customers`: Standardized customer data
- `stg_orders`: Standardized order data
- `stg_products`: Standardized product data
- `stg_order_items`: Standardized order line items

All staging models are materialized as views for efficiency.

### Marts Models

The marts layer contains business-level transformations:

- `fct_daily_orders`: Daily order metrics aggregated by date and order ID

All marts models are materialized as tables for query performance.

## Data Quality Tests

The project includes data quality tests to ensure data integrity:

- Value validation for order status (must be 'Completed', 'Pending', or 'Cancelled')
- Uniqueness checks for customer IDs

## Getting Started

### Prerequisites

- Snowflake account with access to the finance_db database
- dbt installed (version 1.0.0 or later)
- Properly configured dbt profile for Snowflake connection

### Setup

1. Clone this repository
2. Configure your dbt profile in `~/.dbt/profiles.yml`:

```yaml
snowflake_data_project:
  target: dev
  outputs:
    dev:
      type: snowflake
      account: [your-snowflake-account]
      user: [your-username]
      password: [your-password]
      role: [your-role]
      database: finance_db
      warehouse: [your-warehouse]
      schema: analytics
      threads: 4
```

### Running the Project

Execute the following commands:

```bash
# Install dependencies
dbt deps

# Run all models
dbt run

# Run tests
dbt test

# Generate documentation
dbt docs generate
dbt docs serve
```

## Development Workflow

1. Define sources in `models/sources.yml`
2. Create staging models in `models/staging/`
3. Build business-level models in `models/marts/`
4. Add tests in `tests/`
5. Run and test your models with `dbt run` and `dbt test`

## Resources
- [Reference Cousre](https://www.youtube.com/playlist?list=PLYaPThD6DUS-N6aovIgkc871NJZyw8rY7)
- [dbt Documentation](https://docs.getdbt.com/docs/introduction)
- [dbt Discourse](https://discourse.getdbt.com/)
- [dbt Slack Community](https://community.getdbt.com/)