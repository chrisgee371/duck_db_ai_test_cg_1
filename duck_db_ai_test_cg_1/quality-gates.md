# Lineage rules

This light build still requires model-safe lineage.

## Rule

Whenever one generated model depends on another generated model, reference the upstream model using:

`{{ ref('model_name') }}`

Do **not** use `{{ source() }}` for models created inside this build.

## Why

Even though this build is a single iteration, using model refs keeps the resulting DAG stable and prevents disconnected duplicate fragments if the build is extended later.
