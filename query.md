# 1 - Selezionare tutti gli eventi gratis, cioè con prezzo nullo (19)

```SQL
SELECT * FROM `events` WHERE `price` is NULL;
```

# 2 - Selezionare tutte le location in ordine alfabetico (82)

```SQL
SELECT * FROM `locations` ORDER BY `name`;
```

# 3 - Selezionare tutti gli eventi che costano meno di 20 euro e durano meno di 3 ore (38)

```SQL
SELECT * FROM `events` WHERE `price` < 20 AND `duration` < '03:00:00';
```

# 4 - Selezionare tutti gli eventi di dicembre 2023 (25)

```SQL
SELECT * FROM `events` WHERE MONTH(`start`) = 12 AND YEAR(`start`) = 2023;
```

# 5 - Selezionare tutti gli eventi con una durata maggiore alle 2 ore (823)

```SQL
SELECT * FROM `events` WHERE HOUR(`duration`) > 2;
```


# 6 - Selezionare tutti gli eventi, mostrando nome, data di inizio, ora di inizio, ora di fine e durata totale (1040)
```SQL
SELECT `name`, DATE(`start`) AS `date`, TIME(`start`) AS `start`, TIME( ADDTIME(`start`, `duration`)) AS `end`, `duration` FROM `events`;
```

# 7 - Selezionare tutti gli eventi aggiunti da “Fabiano Lombardo” (id: 1202) (132)

```SQL
SELECT * FROM `events` WHERE `user_id` = 1202;
```

# 8 - Selezionare il numero totale di eventi per ogni fascia di prezzo (81)

```SQL
SELECT COUNT(`price`) AS `total`, `price` FROM `events` GROUP BY `price`;
```

# 9 - Selezionare tutti gli utenti admin ed editor (9)

```SQL
SELECT * FROM `users` WHERE `role_id` = 1 OR `role_id` = 2;
```

# 10 - Selezionare tutti i concerti (eventi con il tag “concerti”) (72)

```SQL
SELECT * FROM `events` JOIN `event_tag` ON `event_tag`.`event_id` = `events`.`id` WHERE `tag_id` = 1;
```

# 11 - Selezionare tutti i tag e il prezzo medio degli eventi a loro collegati (11)

```SQL
SELECT `tags`.`name` AS `tag`, AVG(`events`.`price`) AS `avarege_price` FROM `tags` JOIN `event_tag` ON `event_tag`.`tag_id` = `tags`.`id` JOIN `events` ON `events`.`id` = `event_tag`.`event_id` GROUP BY `tags`.`name`;
```

# 12 - Selezionare tutte le location e mostrare quanti eventi si sono tenute in ognuna di esse (82)

```SQL
SELECT `locations`.*, COUNT(`events`.`id`) AS `events_number`  FROM `locations` JOIN `events` ON `events`.`location_id` = `locations`.`id` GROUP BY `locations`.`id`;
```

# 13 - Selezionare tutti i partecipanti per l’evento “Concerto Classico Serale” (slug: concerto-classico-serale, id: 34) (30)

```SQL
SELECT `users`.*, `events`.`name` AS `event_name` FROM `users` JOIN `bookings` ON `bookings`.`user_id` = `users`.`id` JOIN `events` ON `events`.`id` = `bookings`.`event_id` WHERE `events`.`id` = 34;
```

# 14 - Selezionare tutti i partecipanti all’evento “Festival Jazz Estivo” (slug: festival-jazz-estivo, id: 2) specificando nome e cognome (13)

```SQL
SELECT `users`.`first_name`,`users`.`last_name`, `events`.`name` AS `event_name` FROM `users` JOIN `bookings` ON `bookings`.`user_id` = `users`.`id` JOIN `events` ON `events`.`id` = `bookings`.`event_id` WHERE `events`.`id` = 2;
```

# 15 - Selezionare tutti gli eventi sold out (dove il totale delle prenotazioni è uguale ai biglietti totali per l’evento) (18)

```SQL
SELECT `events`.*, COUNT(`bookings`.`event_id`) AS `tickets_sold` FROM `events` JOIN `bookings` ON `bookings`.`event_id` = `events`.`id` GROUP BY `events`.`id` HAVING `events`.`total_tickets` = `tickets_sold`;
```

# 16 - Selezionare tutte le location in ordine per chi ha ospitato più eventi (82)

```SQL
SELECT `locations`.*, COUNT(`events`.`id`) AS `total_events` FROM `locations` JOIN `events` ON `events`.`location_id` = `locations`.`id` GROUP BY `locations`.`id` ORDER BY `total_events` DESC;
```

# 17 - Selezionare tutti gli utenti che si sono prenotati a più di 70 eventi (74)

```SQL
SELECT `users`.*, COUNT(`bookings`.`user_id`) AS `total_bookings` FROM `users` JOIN `bookings` ON `bookings`.`user_id` = `users`.`id` GROUP BY `users`.`id` HAVING `total_bookings` > 70;
```

# 18 - Selezionare tutti gli eventi, mostrando il nome dell’evento, il nome della location, il numero di prenotazioni e il totale di biglietti ancora disponibili per l’evento (1040)

```SQL
SELECT `events`.`name` AS `event_name`, `locations`.`name` AS `location_name`, COUNT(`bookings`.`event_id`) AS `total_bookings`, (`events`.`total_tickets` - COUNT(`bookings`.`event_id`)) AS `available_tickets` FROM `locations` JOIN `events` ON `events`.`location_id` = `locations`.`id` JOIN `bookings` ON `bookings`.`event_id` = `events`.`id` GROUP BY `events`.`id`;
```