{
  "schema_version": "1.0",
  "delivery_variant": "light_single_iteration",
  "metrics": [
    {
      "metric_name": "sessions",
      "grain": "segment",
      "definition": "count(*) from int__session_core grouped at mart grain"
    },
    {
      "metric_name": "orders",
      "grain": "segment",
      "definition": "sum(ordered_flag)"
    },
    {
      "metric_name": "conversion_rate",
      "grain": "segment",
      "definition": "orders / sessions"
    },
    {
      "metric_name": "gross_revenue_usd",
      "grain": "segment",
      "definition": "sum(coalesce(gross_revenue_usd, 0))"
    },
    {
      "metric_name": "net_revenue_usd",
      "grain": "segment",
      "definition": "sum(coalesce(net_revenue_usd, 0))"
    },
    {
      "metric_name": "revenue_per_session",
      "grain": "segment",
      "definition": "net_revenue_usd / sessions"
    },
    {
      "metric_name": "refund_value_share",
      "grain": "segment",
      "definition": "(gross_revenue_usd - net_revenue_usd) / gross_revenue_usd when gross_revenue_usd > 0 else 0"
    },
    {
      "metric_name": "baseline_conversion_rate",
      "grain": "month",
      "definition": "sum(orders) / sum(sessions) over all segments in the same session_month"
    },
    {
      "metric_name": "baseline_revenue_per_session",
      "grain": "month",
      "definition": "sum(net_revenue_usd) / sum(sessions) over all segments in the same session_month"
    },
    {
      "metric_name": "baseline_refund_value_share",
      "grain": "month",
      "definition": "sum(gross_revenue_usd - net_revenue_usd) / sum(gross_revenue_usd) over all segments in the same session_month"
    },
    {
      "metric_name": "severity_score",
      "grain": "segment",
      "definition": "round(100 * (0.50 * conv_gap + 0.30 * rps_gap + 0.20 * least(refund_excess, 1)), 2)",
      "notes": [
        "conv_gap = greatest(0, 1 - conversion_rate / nullif(baseline_conversion_rate, 0))",
        "rps_gap = greatest(0, 1 - revenue_per_session / nullif(baseline_revenue_per_session, 0))",
        "refund_excess = greatest(0, refund_value_share / nullif(baseline_refund_value_share, 0) - 1)",
        "If the monthly refund baseline is 0, treat refund_excess as 0."
      ]
    }
  ]
}