* [tbl.r] `tbl()` is a S3 generic function that dispatches on a data source that has `src` as one of its classes.

* [src-sqlite.r] `tbl.src_sqlite()` takes a `sqlite` data source and a table name. It calls `tbl_sql()`.

* [tbl-sql.r] `tbl_sql()` takes a subclass name, a data source and a table name. It also has a `vars` argument that's defaulted to `attr(from, "vars")`. It calls `make_tbl()`. It creates a class hierarchy with the subclass name supplied, "sql" and lastly "lazy". The arguments passed to `make_tbl()` as dots are `src` which is the data source and `ops` that's generated by `op_base_remote(src, from, vars)`.

* [tbl.r] `make_tbl()` takes a subclass name, prepend "tbl_" in front of the name, make a list with the dots and assign the subclass and `tbl` class to the list.

* [lazy-ops.R] `op_base_remote()` takes a data source, a table name or a sql subquery, a connection and a `vars` argument. If `vars` is `null`, then `db_query_fields(con, x)` is called to determine the column names. It then calls `op_base("remote", src, x, vars)`.

* [lazy-ops.R] `op_base()` takes a operation name, a data source, a table name identifier or a sql subquery, and a column names vector. It returns a list that has three elements: `src`, `x` and `vars`. The list has 3 classes: "op_base_" prepended to the operation name supplied, "op_base" and "op".

* After `tbl()` is successfully executed, you get an object that's a list with 4 classes: "tbl_sqlite", "tbl_sql", "tbl_lazy" and "tbl". It has 2 elements: `src` that stores the data source and `ops` that stores the same data source, the table name or sql subquery and the column names vector. The `ops` element has 3 classes: "op_base_remote", "op_base", and "ops".

* [tbl-lazy.R] `filter_.tbl_lazy()` takes `.data` that has `tbl_lazy` class, `.dots` that specifies the filtering and `...` to add other filtering. It calls `add_op_single()` to add the filering operation to the data.

* [lazy-ops.R] `add_op_single()` takes an operation `name`, a `.data` to operate on, a `dots` list of filtering and a `args` list. It mainly operates on the `ops` element of `.data`. It calls `op_single()` and assigns the value to `.data$ops` before it returns `.data`.

* [lazy-ops.R] `op_single()` takes an operation `name`, an `ops` element, a `dots` list and an `args` list. It returns a list with 4 elements: `name`, `x`, `dots` and `args`. The list has 3 classes: the operation name prepended with "op_", "op_single" and "op".

* [tbl-lazy.R] Similar verbs to `filter_.tbl_lazy()` are `arrange_.tbl_lazy()`, `select_.tbl_lazy()`, `rename_.tbl_lazy()`, `summarize_.tbl_lazy()`, `mutate_.tbl_lazy()`. They all take a `.data`, a `.dots` lazy list and a `...` to specify additional manipulation dots. They then pass the operation `name`, `.data$ops` and `dots` list to `add_op_single()`.

* [tbl-lazy.R] `group_by_.tbl_lazy()` calls `add_op_single()` with extra `args = list(add = add)`. Similarly `distinct_.tbl_lazy()` calls `add_op_single()` with extra `args = list(.keep_all = .keep_all)`.

* [tbl-lazy.R] `head.tbl_lazy()` calls `add_op_single()` with no `dots` because it operates on the whole table, but it does take `args = list(n = n)` in which `n` is supplied to `head.tbl_lazy()`.

* [tbl-lazy.R] `ungroup.tbl_lazy()` calls `add_op_single()` with not `dots` or `args`.
