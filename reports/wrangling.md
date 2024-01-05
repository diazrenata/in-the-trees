# Wrangling pseudocode

1.  Filter by (1) locations based on static attributes, (2) trees based
    on static attributes. These will be filtering operations on
    relatively small datasets, and they will greatly shrink the data
    that need to be filtered on next.

2.  Then extract measurements over time, select desired columns from all
    tables, and filter based on time-varying attributes.

<!-- -->


    plots_to_use <- filter(PLOT_INFO, STATECD = ..., COUNTYCD = ..., NYEARS_PLOT = ...)

    trees_to_use <- tree_info |>
        filter(PLOT_UNIQUE_ID %in% plots_to_use$PLOT_UNIQUE_ID) |>
        filter(SPCD = ..., NYEARS_TREE = ...)
        
    tree_records <- tree_raw |>
        select(all_of(tree_cols)) |>
        filter(TREE_UNIQUE_ID %in% trees_to_use$TREE_UNIQUE_ID) |>
        left_join(select(PLOT_RAW, all_of(plot_cols))) |>
        left_join(select(COND_RAW, all_of(cond_cols))) |>
        filter(INVYR %in% invyr_window, CONDID = ..., STATUSCD = ...)