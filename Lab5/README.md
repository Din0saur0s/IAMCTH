## Цель работы

1.  Закрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R
3.  Закрепить навыки исследования метаданных DNS трафика

## Исходные данные

RStudioCloud

## Код программы

### Подготовка данных (Задания 1-3)

### Задание 4

    ## [1] 123352

### Задание 5

    ## [1] 0.9625458

### Задание 6

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

    ## Selecting by N

    ## # A tibble: 10 × 1
    ##    ip_host        
    ##    <chr>          
    ##  1 10.10.117.210  
    ##  2 192.168.202.93 
    ##  3 192.168.202.103
    ##  4 192.168.202.76 
    ##  5 192.168.202.97 
    ##  6 192.168.202.141
    ##  7 10.10.117.209  
    ##  8 192.168.202.110
    ##  9 192.168.203.63 
    ## 10 192.168.202.106

### Задание 7

    ## Selecting by N

    ## # A tibble: 15 × 1
    ##    query          
    ##    <chr>          
    ##  1 dropbox.com    
    ##  2 apple.com      
    ##  3 sorbs.net      
    ##  4 google.com     
    ##  5 hec.net        
    ##  6 microsoft.com  
    ##  7 fireeye.com    
    ##  8 org.localdomain
    ##  9 ahbl.org       
    ## 10 apews.org      
    ## 11 nszones.com    
    ## 12 spamcop.net    
    ## 13 spamhaus.org   
    ## 14 spamrats.com   
    ## 15 tornevall.org

### Задание 8

    ##     query                 N          
    ##  Length:397         Min.   :  1.000  
    ##  Class :character   1st Qu.:  1.000  
    ##  Mode  :character   Median :  1.000  
    ##                     Mean   :  3.108  
    ##                     3rd Qu.:  2.000  
    ##                     Max.   :104.000

### Задание 9

    ## # A tibble: 428,292 × 3
    ## # Groups:   log.4$query [5,176]
    ##    ip_host        query                                                  log.4…¹
    ##    <chr>          <chr>                                                  <chr>  
    ##  1 192.168.202.76 "HPE8AA67"                                             "HPE8A…
    ##  2 192.168.202.76 "HPE8AA67"                                             "HPE8A…
    ##  3 192.168.202.76 "HPE8AA67"                                             "HPE8A…
    ##  4 192.168.202.76 "WPAD"                                                 "WPAD" 
    ##  5 192.168.202.76 "WPAD"                                                 "WPAD" 
    ##  6 192.168.202.76 "WPAD"                                                 "WPAD" 
    ##  7 192.168.202.89 "EWREP1"                                               "EWREP…
    ##  8 192.168.202.89 "EWREP1"                                               "EWREP…
    ##  9 192.168.202.89 "EWREP1"                                               "EWREP…
    ## 10 192.168.202.89 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\… "*\\x0…
    ## # … with 428,282 more rows, and abbreviated variable name ¹​`log.4$query`

### Задание 10

    ## Loading required package: devtools

    ## Loading required package: usethis
