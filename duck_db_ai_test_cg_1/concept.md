{
  "schema_version": "1.0",
  "delivery_variant": "light_single_iteration",
  "contracts": [
    {
      "id": "JOIN-LIGHT-001",
      "left_model": "src__website_sessions",
      "right_model": "src__website_pageviews",
      "left_key": [
        "website_session_id"
      ],
      "right_key": [
        "website_session_id"
      ],
      "cardinality": "one_to_many",
      "recommended_join_type": "left",
      "purpose": "Derive entry_page per session."
    },
    {
      "id": "JOIN-LIGHT-002",
      "left_model": "src__orders",
      "right_model": "src__order_item_refunds",
      "left_key": [
        "order_id"
      ],
      "right_key": [
        "order_id"
      ],
      "cardinality": "one_to_zero_or_many",
      "recommended_join_type": "left",
      "purpose": "Aggregate refunds to order_id for net revenue."
    },
    {
      "id": "JOIN-LIGHT-003",
      "left_model": "src__website_sessions",
      "right_model": "int__order_net_value",
      "left_key": [
        "website_session_id"
      ],
      "right_key": [
        "website_session_id"
      ],
      "cardinality": "one_to_zero_or_one",
      "recommended_join_type": "left",
      "purpose": "Attach commercial outcome to each session."
    }
  ],
  "explicitly_not_used": [
    {
      "table_name": "order_items",
      "reason": "Removed from the light build to cut join count and reduce failure risk."
    },
    {
      "table_name": "products",
      "reason": "Removed from the light build because product-launch and bundle logic are not required for deadline-safe delivery."
    }
  ]
}