concat(
    variables('hour'),
    if(equals(variables('min'), ''), '', concat(':', variables('min'))),
    if(equals(variables('sec'), ''), '', concat(':', variables('sec')))
)
