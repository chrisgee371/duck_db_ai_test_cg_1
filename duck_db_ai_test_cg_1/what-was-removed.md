{
  "schema_version": "1.0",
  "dataset_name": "Maven Fuzzy Factory",
  "dataset_slug": "maven-fuzzy-factory",
  "source_type": "local_csv_bundle_supplied_by_user",
  "dialect_target": "duckdb",
  "repo_relative_root": "datasets/maven-fuzzy-factory",
  "delivery_variant": "light_single_iteration",
  "files": [
    {
      "table_name": "website_sessions",
      "file_name": "website_sessions.csv",
      "repo_relative_path": "datasets/maven-fuzzy-factory/raw/website_sessions.csv",
      "row_count": 472871,
      "primary_key": [
        "website_session_id"
      ],
      "timestamp_columns": [
        "created_at"
      ],
      "required_in_light_build": true,
      "notes": [
        "Session grain.",
        "Normalize null utm_source and utm_campaign to 'direct_or_unknown'.",
        "Use created_at to derive session_month with date_trunc('month', created_at)."
      ]
    },
    {
      "table_name": "website_pageviews",
      "file_name": "website_pageviews.csv",
      "repo_relative_path": "datasets/maven-fuzzy-factory/raw/website_pageviews.csv",
      "row_count": 1188124,
      "primary_key": [
        "website_pageview_id"
      ],
      "foreign_keys": [
        {
          "column": "website_session_id",
          "references": "website_sessions.website_session_id"
        }
      ],
      "timestamp_columns": [
        "created_at"
      ],
      "required_in_light_build": true,
      "notes": [
        "Use only to derive each session's entry_page.",
        "Entry page is the earliest pageview per website_session_id ordered by created_at then website_pageview_id."
      ]
    },
    {
      "table_name": "orders",
      "file_name": "orders.csv",
      "repo_relative_path": "datasets/maven-fuzzy-factory/raw/orders.csv",
      "row_count": 32313,
      "primary_key": [
        "order_id"
      ],
      "foreign_keys": [
        {
          "column": "website_session_id",
          "references": "website_sessions.website_session_id"
        }
      ],
      "timestamp_columns": [
        "created_at"
      ],
      "required_in_light_build": true,
      "notes": [
        "Use orders.price_usd as gross_revenue_usd in the light build.",
        "There is at most one order per website_session_id in the supplied data."
      ]
    },
    {
      "table_name": "order_item_refunds",
      "file_name": "order_item_refunds.csv",
      "repo_relative_path": "datasets/maven-fuzzy-factory/raw/order_item_refunds.csv",
      "row_count": 1731,
      "primary_key": [
        "order_item_refund_id"
      ],
      "foreign_keys": [
        {
          "column": "order_id",
          "references": "orders.order_id"
        }
      ],
      "timestamp_columns": [
        "created_at"
      ],
      "required_in_light_build": true,
      "notes": [
        "Aggregate refunds to order_id before joining to orders.",
        "Refunds can occur after the order-date window; use the actual refund rows provided."
      ]
    },
    {
      "table_name": "order_items",
      "file_name": "order_items.csv",
      "repo_relative_path": "datasets/maven-fuzzy-factory/raw/order_items.csv",
      "row_count": 40025,
      "primary_key": [
        "order_item_id"
      ],
      "timestamp_columns": [
        "created_at"
      ],
      "required_in_light_build": false,
      "notes": [
        "Included in the bundle but intentionally unused in the light build to reduce complexity."
      ]
    },
    {
      "table_name": "products",
      "file_name": "products.csv",
      "repo_relative_path": "datasets/maven-fuzzy-factory/raw/products.csv",
      "row_count": 4,
      "primary_key": [
        "product_id"
      ],
      "timestamp_columns": [
        "created_at"
      ],
      "required_in_light_build": false,
      "notes": [
        "Included in the bundle but intentionally unused in the light build."
      ]
    },
    {
      "table_name": "maven_fuzzy_factory_data_dictionary",
      "file_name": "maven_fuzzy_factory_data_dictionary.csv",
      "repo_relative_path": "datasets/maven-fuzzy-factory/raw/maven_fuzzy_factory_data_dictionary.csv",
      "row_count": 36,
      "primary_key": [],
      "timestamp_columns": [],
      "required_in_light_build": false,
      "notes": [
        "Supporting documentation only."
      ]
    }
  ]
}