@(3..1157 | Where-Object { $_ % 3 -eq 0 }) | ForEach-Object {
    [PSCustomObject]@{
        Sum  = $_ | Measure-Object -Sum | Select-Object -ExpandProperty Sum
        Avg  = $_ | Measure-Object -Average | Select-Object -ExpandProperty Average
        Max  = $_ | Measure-Object -Maximum | Select-Object -ExpandProperty Maximum
        Min  = $_ | Measure-Object -Minimum | Select-Object -ExpandProperty Minimum
    }
}
