```{r}
install.packages('nycflights13')
install.packages("dplyr")
```

```{r}
library(nycflights13)
library(dplyr)
#1. Сколько встроенных в пакет nycflights13 датафреймов?

numb_of_df <- ls("package:nycflights13")
length(numb_of_df)

#2. Сколько строк в каждом датафрейме?
#3. Сколько столбцов в каждом датафрейме?

rows <- c(dim(airlines)[1], dim(airports)[1], dim(flights)[1], dim(planes)[1], dim(weather)[1])

columns <- c(dim(airlines)[2], dim(airports)[2], dim(flights)[2], dim(planes)[2], dim(weather)[2])

DF_struct <- data.frame(Dataframe = numb_of_df, Rows = rows, Columns = columns)

DF_struct
```

```{r}
#4. Как просмотреть примерный вид датафрейма?
library(nycflights13)
glimpse(flights)
```

```{r}
#5. Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных (представлено в наборах данных)?

count(select(flights %>%
               group_by(carrier) %>%
               summarise(N = n()
                         )
             )
      )

```

```{r}
#6. Сколько рейсов принял аэропорт John F Kennedy Intl в мае?
count(filter(flights,origin=='JFK', month == 5))
```

```{r}
#7. Какой самый северный аэропорт?
airports$name[which.max(airports$lat)]
max(airports$lat)
```

```{r}
#8. Какой аэропорт самый высокогорный (находится выше всех над уровнем моря)?
airports$name[which.max(airports$alt)]
```

```{r}
#9. Какие бортовые номера у самых старых самолетов?
library(nycflights13)
#glimpse(planes)
select(planes,tailnum, -(year)) %>% top_n(10)
```

```{r}
#10. Какая средняя температура воздуха была в сентябре в аэропорту John F Kennedy Intl (в градусах Цельсия).
library(nycflights13)
#glimpse(weather)
w <- filter(weather,origin=='JFK', month == 9)
(5/9)*(mean(w$temp)-32)
```

```{r}
#11. Самолеты какой авиакомпании совершили больше всего вылетов в июне?
library(dplyr)
library(nycflights13)
flights.j <- flights %>%
  filter(month == 5) %>%
  group_by(carrier) %>%
  summarise(N = n())
select(arrange(flights.j,desc(N)) %>% top_n(1),carrier)

```

```{r}
#12. Самолеты какой авиакомпании задерживались чаще других в 2013 году?
flights.2013 <- flights %>% 
  filter(year == 2013) %>%
  filter(dep_delay>0) %>%
  group_by(carrier) %>% 
  summarise(N = n())
select(arrange(flights.2013,desc(N)) %>% top_n(1),carrier)
```
