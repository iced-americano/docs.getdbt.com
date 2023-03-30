---
title: "Model access"
id: model-access
sidebar_label: "Model access"
description: "Define model access with group capabilities"
---

:::info Beta functionality
This functionality is new in v1.5. These docs exist to provide a high-level overview of what's to come. The specific syntax is liable to change.

For more details and to leave your feedback, join the GitHub discussion:
["Model groups & access" (dbt-core#6730)](https://github.com/dbt-labs/dbt-core/discussions/6730)
:::

## Related documentation
* [`groups`](build/groups)
* [`access`](resource-properties/access)

## Groups

Models can be grouped under a common designation with a shared owner.

Why define model `groups`?
- It turns implicit relationships into an explicit grouping
- It enables you to mark specific models as "private" for use _only_ within that group

## Access modifiers

Some models (not all) are designed to be referenced through the [ref](ref) function across groups. Models can set an [access modifier](https://en.wikipedia.org/wiki/Access_modifiers) to indicate their intended level of accessibility.

| Access    | Referenceable by              |
|-----------|-------------------------------|
| private   | same group                    |
| protected | same project/package          |
| public    | any group, package or project |

By default, all models are `protected`. This means that other models in the same project can reference them, regardless of their group. This is largely for backwards compatability when assigning groups to an existing set of models, as there may already be existing references across group assignments.

However, it is recommended to set the access modifier of a new model to `private` to prevent other project resources from taking dependencies on models not intentionally designed for sharing across groups.

:::info Under construction 🚧
The following syntax is currently under review and does not work.
:::

<File name="models/marts/customers.yml">

```yaml
groups:
  - name: cx
    owner:
      name: Customer Success Team
      email: cx@jaffle.shop

models:
  - name: dim_customers
    group: cx
    access: public
  # this is an intermediate transformation -- relevant to the CX team only
  - name: int__customer_history_rollup
    group: cx
    access: private
```

</File>