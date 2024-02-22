## Selezionare tutti gli eventi gratis, cioè con prezzo nullo (19)

`mySql`

`SELECT * FROM `events` WHERE events.price IS NULL;`

## Selezionare tutte le location in ordine alfabetico (82)

`SELECT * FROM `locations`ORDER BY`name` ASC;`

## Selezionare tutti gli eventi che costano meno di 20 euro e durano meno di 3 ore (38)

`SELECT * from `events` where price <20 and time_to_sec(duration)/60<180;`

## Selezionare tutti gli eventi di dicembre 2023 (25)

`SELECT * FROM `events` WHERE start BETWEEN '2023-12-01' AND '2024-01-01';`

## Selezionare tutti gli eventi con una durata maggiore alle 2 ore (823)

### in realta è maggiore o uguale alle 3 ore (823)

`SELECT events.duration FROM `events` WHERE time_to_sec(duration)/60>=180`

## Selezionare tutti gli eventi, mostrando nome, data di inizio, ora di inizio, ora di fine e durata totale (1040)

`SELECT name, date(start) as `start_date`,time(start) as `time_start`, time(end_of_sale) as `end_time`, duration FROM `events`;`

## Selezionare tutti gli eventi aggiunti da “Fabiano Lombardo” (id: 1202) (132)

`SELECT * FROM `events` where user_id=1202;`

## Selezionare il numero totale di eventi per ogni fascia di prezzo (81)

`SELECT price, COUNT(price) as `tot_events`FROM`events` GROUP by price;`

## Selezionare tutti gli utenti admin ed editor (9)

`SELECT * FROM `users` where role_id=1 or role_id=2;`

## Selezionare tutti i concerti (eventi con il tag “concerti”) (72)

`SELECT * FROM ((`events`
INNER JOIN event_tag ON events.id = event_tag.event_id)
INNER JOIN tags ON event_tag.tag_id = tags.id)
where tags.name LIKE 'Concerti';`

## Selezionare tutti i tag e il prezzo medio degli eventi a loro collegati (11)

`SELECT tags.name, AVG(price) FROM ((`events`
INNER JOIN event_tag ON events.id = event_tag.event_id)
INNER JOIN tags ON event_tag.tag_id = tags.id)
GROUP BY tags.name;`

## Selezionare tutte le location e mostrare quanti eventi si sono tenute in ognuna di esse (82)

`SELECT COUNT(events.name) as `events_number`, locations.name FROM (`events`
INNER JOIN locations ON events.location_id = locations.id)
GROUP by locations.id;`

## Selezionare tutti i partecipanti per l’evento “Concerto Classico Serale” (slug: concerto-classico-serale, id: 34) (30)

`SELECT events.total_tickets, users.first_name, users.last_name FROM `events`
INNER JOIN users ON events.user_id = users.id
INNER JOIN bookings on events.id=bookings.event_id  
where events.slug LIKE 'concerto-classico-serale';`

## Selezionare tutti i partecipanti all’evento “Festival Jazz Estivo” (slug: festival-jazz-estivo, id: 2) specificando nome e cognome (13)

`SELECT COUNT(bookings.id) as count_book, events.name, users.first_name, users.last_name FROM `events`
INNER JOIN users
ON events.user_id=users.id
INNER JOIN bookings
on events.id=bookings.event_id
WHERE events.slug = 'festival-jazz-estivo'
GROUP by bookings.id;`

## Selezionare tutti gli eventi sold out (dove il totale delle prenotazioni è uguale ai biglietti totali per l’evento) (18)

`SELECT * FROM `events`
INNER JOIN bookings
ON events.id=bookings.event_id
where events.total_tickets=bookings.user_id;`

## Selezionare tutte le location in ordine per chi ha ospitato più eventi (82)

`SELECT COUNT(events.total_tickets) as total_events, locations.name FROM `events`
INNER JOIN locations
ON events.location_id=locations.id  
GROUP BY events.location_id
ORDER BY total_events DESC;`

## Selezionare tutti gli utenti che si sono prenotati a più di 70 eventi (74)

`SELECT COUNT(users.id) as tot_booking, users.first_name, users.last_name, bookings.user_id FROM `users`
INNER JOIN bookings
ON users.id=bookings.user_id
GROUP by users.id
HAVING tot_booking > 70;`

## Selezionare tutti gli eventi, mostrando il nome dell’evento, il nome della location, il numero di prenotazioni e il totale di biglietti ancora disponibili per l’evento (1040)

`SELECT events.name as events_name, locations.name as locations_name, COUNT(bookings.id) as tot_book, events.total_tickets-COUNT(bookings.id) as available_tickets FROM `events`
INNER JOIN locations
on events.location_id=locations.id
INNER JOIN users
on events.user_id=users.id
INNER JOIN bookings
ON events.id=bookings.event_id
GROUP BY events.id;`
