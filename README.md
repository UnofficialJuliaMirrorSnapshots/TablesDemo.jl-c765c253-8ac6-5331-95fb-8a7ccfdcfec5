# TablesDemo

[![Build Status](https://travis-ci.org/davidagold/TablesDemo.jl.svg?branch=master)](https://travis-ci.org/davidagold/TablesDemo.jl)

[![Coverage Status](https://coveralls.io/repos/davidagold/TablesDemo.jl/badge.svg?branch=master&service=github)](https://coveralls.io/github/davidagold/TablesDemo.jl?branch=master)

[![codecov.io](http://codecov.io/github/davidagold/TablesDemo.jl/coverage.svg?branch=master)](http://codecov.io/github/davidagold/TablesDemo.jl?branch=master)

This package demonstrates a minimalist tabular data type, `Table`, in Julia. As in [AbstractTables.jl](https://github.com/davidagold/AbstractTables.jl), our objective is to encourage modularity and extensibility in the development tabular data facilities. To this end, the present package itself provides as few user-facing components as possible; rather, the `Table` API is primarily designed to satisfy other tabular data interfaces that provide familiar functionality such as IO, manipulation, modeling, and visualization. In particular, this package demonstrates how a tabular data type such as `Table` can take advantage of the [StructuredQueries](https://github.com/davidagold/StructuredQueries.jl) query framework as extended by AbstractTables.

### Interfaces

The `Table` data type satisfies each of the [three AbstractTable interfaces](https://github.com/davidagold/AbstractTables.jl#interfaces). As such, it inherits all of the provided methods of these interfaces -- in particular those of the StructuredQueries-provided querying framework:
```julia
julia> using TablesDemo

julia> tbl = TablesDemo.Table(
               name = ["Niamh", "Roger", "Genevieve", "Aiden"],
               age = [27, 63, 26, 17],
               eye_color = ["green", "brown", "brown", "blue"]
           )
TablesDemo.Table
│ Row │ name        │ age │ eye_color │
├─────┼─────────────┼─────┼───────────┤
│ 1   │ "Niamh"     │ 27  │ "green"   │
│ 2   │ "Roger"     │ 63  │ "brown"   │
│ 3   │ "Genevieve" │ 26  │ "brown"   │
│ 4   │ "Aiden"     │ 17  │ "blue"    │


julia> qry = @query tbl |>
           groupby(age > 20, eye_color) |>
           summarize(avg_age = mean(age))
A Query.


julia> typeof(ans)
StructuredQueries.Query{TablesDemo.Table}

julia> collect(qry)
TablesDemo.Table
With the following grouping predicate aliases:
    age > 20 => predicate_1

│ Row │ predicate_1 │ eye_color │ avg_age │
├─────┼─────────────┼───────────┼─────────┤
│ 1   │ true        │ "brown"   │ 44.5    │
│ 2   │ false       │ "blue"    │ 17.0    │
│ 3   │ true        │ "green"   │ 27.0    │
```
