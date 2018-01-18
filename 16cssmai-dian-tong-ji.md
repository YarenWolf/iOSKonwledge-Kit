# CSS 埋点统计

> 当一个网站或者





```
<?php

    $actionName =  $_REQUEST["action"];

    //时间格式化
    $time = time();
    $time = Date("Y-m-d",$time);
    
    echo "访问动作->" .$actionName. " 访问时间->" . $time;
?>
```



