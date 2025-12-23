### Query 1: JOIN

Retrieve booking information along with vehicle name and customer name

This query fetches a list of all bookings, including the booking ID, customer name, vehicle name, rental period, and booking status.

##### Solution:

```sql
SELECT
  b.id AS booking_id,
  u.name AS customer_name,
  v.vehicle_name AS vehicle_name,
  b.rent_start_date AS start_date,
  b.rent_end_date AS end_date,
  b.booking_status AS status
FROM bookings b
JOIN users u ON b.user_id = u.id
JOIN vehicles v ON b.vehicle_id = v.id
ORDER BY b.id;
```

### Query 2: EXISTS

Find all vehicles that have never been booked.

This query returns all vehicles that have no associated bookings in the bookings table.

##### Solution:
```sql
SELECT
  v.id AS vehicle_id,
  v.vehicle_name AS name,
  v.type,
  v.model,
  v.registration_number,
  v.daily_rent_price AS rental_price,
  v.availability_status AS status
FROM vehicles v
WHERE NOT EXISTS (
  SELECT 1
  FROM bookings b
  WHERE b.vehicle_id = v.id
) 
ORDER BY v.id ASC;
```

### Query 3: WHERE

Retrieve all available vehicles of a specific type (e.g. cars).

This query lists all currently available vehicles of type 'car'. You can change 'car' to any other vehicle type as needed.

##### Solution:

```sql
SELECT
  id AS vehicle_id,
  vehicle_name AS name,
  type,
  model,
  registration_number,
  daily_rent_price AS rental_price,
  availability_status AS status
FROM vehicles
WHERE availability_status = 'available'
  AND type = 'car';
  ```

### Query 4: GROUP BY and HAVING

Find the total number of bookings for each vehicle and display only those vehicles that have more than 2 bookings.

This query counts the total number of bookings for each vehicle and returns only those vehicles that have been booked more than twice.

##### Solution:

```sql
SELECT
  v.vehicle_name,
  COUNT(b.id) AS total_bookings
FROM bookings b
JOIN vehicles v ON b.vehicle_id = v.id
GROUP BY v.vehicle_name
HAVING COUNT(b.id) > 2;
```
