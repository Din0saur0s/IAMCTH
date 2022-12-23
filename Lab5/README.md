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

    #install.packages("tidyverse")

    header <- read.csv("header.csv")

    # Импортируйте данные DNS
    # Добавьте пропущенные данные о структуре данных (назначении столбцов)
    # Преобразуйте данные в столбцах в нужный формат

    log <- read.csv(file = "dns.log", sep = "")
    colnames(log) <-  c(
      'ts',
      'uid',
      'ip_host',
      'port_host',
      'ip_recipient',
      'port_recipient',
      'protocol',
      'trans_id',
      'query',
      'qclass',
      'qclass_name',
      'qtype',
      'qtype_name',
      'rcode',
      'rcode_name',
      'QR',
      'AA',
      'TC RD',
      'RA',
      'Z',
      'answers',
      'TTLs',
      'rejected'
    )

### Задание 4

    # Сколько участников информационного обмена в сети Доброй Организации?

    d1<-length(unique((grep(":", log$ip_host, value = TRUE)),(grep(":", log$ip_recipient, value = TRUE))))
    d2<-length(unique(log$ip_host, log$ip_recipient))
    d2-d1

    ## [1] 123352

### Задание 5

    #5. Какое соотношение участников обмена внутри сети и участников обращений к внешним ресурсам?

    toMatch <- c("192.168.","10.10","100.","172.")
    ins <- length(unique (grep(paste(toMatch,collapse="|"), log$ip_recipient, value=TRUE)))
    #внешние
    outs <- d2-d1-ins
    ins/outs*100 

    ## [1] 0.9625458

### Задание 6

    # Найдите топ-10 участников сети, проявляющих наибольшую сетевую активность.
    library(dplyr)

    log.2 <- log %>%
                   group_by(ip_host) %>%
                   summarise(N = n()
                             )
    select(arrange(log.2,desc(N))%>%top_n(10),ip_host)

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

    # Найдите топ-10 доменов, к которым обращаются пользователи сети и соответственное количество обращений
    library(stringr)
    library(dplyr)

    net.1 <- c(".net",".com",".org")
    net.2 <- unique (grep(paste(net.1,collapse="|"), log$query, value=TRUE))
    log.2 <- as.data.frame(str_extract(net.2, "\\w*.[a-z]*$"))
    names(log.2)[1] <- "query"

    log.3 <- log.2 %>%
      group_by(query) %>% 
      summarise(N = n())
    domains <- select(arrange(log.3,desc(N)) %>% top_n(10),query)

    ## Selecting by N

    domains

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

    # Опеределите базовые статистические характеристики (функция summary()) интервала времени между последовательным обращениями к топ-10 доменам.

    library(dplyr)
    log.3%>%summary()

    ##     query                 N          
    ##  Length:397         Min.   :  1.000  
    ##  Class :character   1st Qu.:  1.000  
    ##  Mode  :character   Median :  1.000  
    ##                     Mean   :  3.108  
    ##                     3rd Qu.:  2.000  
    ##                     Max.   :104.000

### Задание 9

    # Часто вредоносное программное обеспечение использует DNS канал в качестве канала управления, периодически отправляя запросы на подконтрольный злоумышленникам DNS сервер. По периодическим запросам на один и тот же домен можно выявить скрытый DNS канал. Есть ли такие IP адреса в исследуемом датасете?

    library(dplyr)
    log.4<-data.frame(log$ip_host,log$query)
    names(log.4)[names(log.4) == 'log.ip_host'] <- 'ip_host'
    names(log.4)[names(log.4) == 'log.query'] <- 'query'

    log.4%>%group_by(log.4$query)

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

    # Определите местоположение (страну, город) и организацию-провайдера для топ-10 доменов. Для этого можно использовать сторонние сервисы, например https://v4.ifconfig.co/
      
    #install.packages("devtools")
    require(devtools)

    ## Loading required package: devtools

    ## Loading required package: usethis

    #devtools::install_github("hrbrmstr/domaintools",force = TRUE)
    library(domaintools)
    # for (x in domains$query)
    # whois(x)

    # Error in whois(x) : Forbidden (HTTP 403).
    # А должно было работать...
