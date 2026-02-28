{
  "schema_version": "1.0",
  "delivery_variant": "light_single_iteration",
  "models": [
    {
      "model_name": "src__website_sessions",
      "model_type": "source",
      "grain": "one row per website_session_id",
      "primary_key": [
        "website_session_id"
      ],
      "depends_on": [],
      "required": true,
      "description": "Typed session source model over website_sessions.csv"
    },
    {
      "model_name": "src__website_pageviews",
      "model_type": "source",
      "grain": "one row per website_pageview_id",
      "primary_key": [
        "website_pageview_id"
      ],
      "depends_on": [],
      "required": true,
      "description": "Typed pageview source model over website_pageviews.csv"
    },
    {
      "model_name": "src__orders",
      "model_type": "source",
      "grain": "one row per order_id",
      "primary_key": [
        "order_id"
      ],
      "depends_on": [],
      "required": true,
      "description": "Typed order source model over orders.csv"
    },
    {
      "model_name": "src__order_item_refunds",
      "model_type": "source",
      "grain": "one row per order_item_refund_id",
      "primary_key": [
        "order_item_refund_id"
      ],
      "depends_on": [],
      "required": true,
      "description": "Typed refund source model over order_item_refunds.csv"
    },
    {
      "model_name": "int__order_net_value",
      "model_type": "intermediate",
      "grain": "one row per order_id",
      "primary_key": [
        "order_id"
      ],
      "depends_on": [
        "src__orders",
        "src__order_item_refunds"
      ],
      "required": true,
      "description": "Order-level gross revenue, refund amount, and net revenue."
    },
    {
      "model_name": "int__session_core",
      "model_type": "intermediate",
      "grain": "one row per website_session_id",
      "primary_key": [
        "website_session_id"
      ],
      "depends_on": [
        "src__website_sessions",
        "src__website_pageviews",
        "int__order_net_value"
      ],
      "required": true,
      "description": "Single-session fact table with entry page, order flag, and net revenue."
    },
    {
      "model_name": "mart__core_leak_summary",
      "model_type": "mart",
      "grain": "one row per session_month x utm_source_normalized x utm_campaign_normalized x device_type_normalized x entry_page",
      "primary_key": [
        "session_month",
        "utm_source_normalized",
        "utm_campaign_normalized",
        "device_type_normalized",
        "entry_page"
      ],
      "depends_on": [
        "int__session_core"
      ],
      "required": true,
      "description": "Core segment summary for conversion, revenue per session, and refund share."
    },
    {
      "model_name": "mart__priority_actions",
      "model_type": "mart",
      "grain": "top segments by severity within the light build",
      "primary_key": [
        "priority_rank"
      ],
      "depends_on": [
        "mart__core_leak_summary"
      ],
      "required": true,
      "description": "Ranked action table showing which segments to fix first."
    }
  ]
}