# Laravel Splade Date Range filter in Table Component
Date Range Filter Table Laravel Splade

Thanks to JIMMO help me to solve this.

For now my only workaround is to make custom month & year filter, using spatie query builder
ref : https://spatie.be/docs/laravel-query-builder/v5/installation-setup

Splade support Table with Spatie's Query Builder => https://splade.dev/docs/table-spatie-query-builder

```
$yearFilter = AllowedFilter::callback('year', function ($query, $value) {
            $query->where(function ($query) use ($value) {
                Collection::wrap($value)->each(function ($value) use ($query) {
                    $query->whereYear('users.created_at', $value);
                });
            });
});

$monthFilter = AllowedFilter::callback('month', function ($query, $value) {
            $query->where(function ($query) use ($value) {
                Collection::wrap($value)->each(function ($value) use ($query) {
                    $query->whereMonth('users.created_at', $value);
                });
            });
});

return QueryBuilder::for(User::class)
            ->defaultSort('-created_at')
            ->allowedSorts(['created_at'])
            ->allowedFilters(['username', 'name', 'email', $yearFilter, $monthFilter]);
```

Then u need to manually set the year & month filter options in splade table

Example:

```
->selectFilter(key: 'year', options: ['2024' => '2024', '2023' => '2023', '2022' => '2022', '2021' => '2021'], label: 'Tahun Izin', noFilterOptionLabel: '-')
->selectFilter(key: 'month', options: ['01' => 'January', '02' => 'February', '03' => 'March', '04' => 'April', '05' => 'May', '06' => 'June', '07' => 'July', '08' => 'August', '09' => 'September', '10' => 'October', '11' => 'November', '12' => 'December'], label: 'Month Permission', noFilterOptionLabel: '-')
```

It working fine for me, maybe this thread can help you to another alternative waiting splade upgrading Table filter improvements in branch #162 

Thanks 
