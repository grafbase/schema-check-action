# Grafbase Schema Check Action

This is a GitHub Action to run schema checks on your GraphQL subgraphs and
standalone graphs before publishing them.

Using it can look like this:

```yaml
- uses: grafbase/schema-check-action@main
  with:
    grafbase_access_token: ${{ secrets.GRAFBASE_ACCESS_TOKEN }}
    project_ref: tomhoule/grafbase-single-graph-ci-example@main
    schema_path: ./sdl.graphql
    subgraph_name: products
```

## Inputs

- `grafbase_access_token` (required): an access token generated on the Grafbase dashboard.
- `project_ref` (required): the project and (optional) branch of the graph to check.
- `schema_path` (required): the file system path to the schema to check (as GraphQL SDL).

   Tip: the easiest way to produce the GraphQL schema for your graph is
   `grafbase introspect --dev > api.graphql`. You can run that command in a
   workflow step before the action, and pass `schema_path: api.graphql` to this
   action.
- `subgraph_name`: the name of the subgraph to check, in a federated project.
  This should not be provided if the project is a single graph.

## Behavior

There are no outputs. The step will fail with the schema check errors if there are any.

## Workflow

In general, a schema check fits in your deployment workflow in any place where
you publish a (sub)graph: you run a check before publishing to make sure that
the schema can be safely published.

For example, if your schema is published on merge to `main`, you would run the
check against `myaccount/myproject@main` on pull requests to `main`.

## Example repositories

- [Single graph with the Postgres connector](https://github.com/tomhoule/grafbase-schema-check-action-single-graph-example) ([workflow file](https://github.com/tomhoule/grafbase-schema-check-action-single-graph-example/blob/main/.github/workflows/check.yml))
