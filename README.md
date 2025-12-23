### Vehicle Rental System – Database Design & Queries

 **User Table :
 
| u_id | name  | email                                     | phone_number | role     |
| ---- | ----- | ----------------------------------------- | ------------ | -------- |
| 1    | alice | [alice@gmail.com](mailto:alice@gmail.com) | 01989890801  | customer |
| 2    | asad  | [asad@gmail.com](mailto:asad@gmail.com)   | 01797220981  | admin    |
| 3    | bob   | [bob@gmail.com](mailto:bob@gmail.com)     | 01812345678  | customer |
| 4    | carol | [carol@gmail.com](mailto:carol@gmail.com) | 01923456789  | customer |
| 5    | david | [david@gmail.com](mailto:david@gmail.com) | 01734567890  | admin    |
| 6    | eva   | [eva@gmail.com](mailto:eva@gmail.com)     | 01898765432  | customer |
| 7    | frank | [frank@gmail.com](mailto:frank@gmail.com) | 01987654321  | customer |
| 8    | grace | [grace@gmail.com](mailto:grace@gmail.com) | 01765432109  | admin    |
| 9    | harry | [harry@gmail.com](mailto:harry@gmail.com) | 01876543210  | customer |
| 10   | irene | [irene@gmail.com](mailto:irene@gmail.com) | 01954321098  | customer |

#Vehical Table :
| v_id | v_name          | v_type | v_model | register_number | startus     | rental_price |
| ---- | --------------- | ------ | ------- | --------------- | ----------- | ------------ |
| 1    | Toyota Corolla  | car    | 2009    | CAR-101         | available   | 500          |
| 2    | Honda Civic     | car    | 2012    | CAR-102         | rented      | 600          |
| 3    | Nissan Sunny    | car    | 2010    | CAR-103         | maintenance | 450          |
| 4    | Suzuki Alto     | car    | 2015    | CAR-104         | available   | 400          |
| 5    | Hyundai Elantra | car    | 2014    | CAR-105         | rented      | 650          |
| 6    | Yamaha R15      | bike   | 2018    | BIKE-201        | available   | 300          |
| 7    | Honda CBR       | bike   | 2017    | BIKE-202        | rented      | 350          |
| 8    | Suzuki Gixxer   | bike   | 2019    | BIKE-203        | maintenance | 320          |
| 9    | Bajaj Pulsar    | bike   | 2016    | BIKE-204        | available   | 280          |
| 10   | TVS Apache      | bike   | 2020    | BIKE-205        | rented      | 330          |

#booking Table

| b_id | u_id | v_id | start_date | end_date   | b_status  | total_cost |
| ---- | ---- | ---- | ---------- | ---------- | --------- | ---------- |
| 1    | 1    | 1    | 2023-01-01 | 2023-01-05 | completed | 2000       |
| 2    | 2    | 2    | 2023-01-03 | 2023-01-06 | confirmed | 1800       |
| 3    | 3    | 3    | 2023-01-05 | 2023-01-10 | completed | 2500       |
| 4    | 4    | 4    | 2023-01-07 | 2023-01-09 | cancelled | 800        |
| 5    | 5    | 5    | 2023-01-10 | 2023-01-15 | confirmed | 3000       |
| 6    | 6    | 6    | 2023-01-12 | 2023-01-14 | completed | 1200       |
| 7    | 7    | 7    | 2023-01-15 | 2023-01-20 | pending   | 3500       |
| 8    | 8    | 8    | 2023-01-18 | 2023-01-22 | confirmed | 2800       |
| 9    | 9    | 9    | 2023-01-20 | 2023-01-25 | completed | 4000       |
| 10   | 10   | 10   | 2023-01-22 | 2023-01-24 | cancelled | 900        |



#[ERD Diagram :](https://lucid.app/lucidchart/fd3ea149-cd97-4436-a4ad-62371ddd58a5/edit?viewport_loc=-1849%2C-761%2C1663%2C769%2C0_0&invitationId=inv_e9093d7d-a62b-4c0d-885a-9fb8070c846d)

+------------------+           +------------------+          +------------------+
|   user_table     |           |    booking      |          |     vehical     |
|------------------|           |-----------------|          |-----------------|
| PK u_id          |<--------- | PK b_id          |--------->| PK v_id          |
| name             | 1       M | FK u_id          |  M     1 | v_name           |
| email (unique)   |           | FK v_id          |          | v_type           |
| phone_number     |           | start_date       |          | v_model          |
| password         |           | end_date         |          | register_number  |
| role             |           | b_status         |          | startus          |
+------------------+           | total_cost       |          | rental_price     |
                               +-----------------+          +-----------------+
															 
##Relationships between  user_table and vehical_table and booking_Table :

| Relationship             | Type      | Description                                                              |
| ------------------------ | --------- | ------------------------------------------------------------------------ |
| `user_table` → `booking` | 1-to-Many | A user can have multiple bookings                                        |
| `vehical` → `booking`    | 1-to-Many | A vehicle can be booked multiple times                                   |
|user_table -> vehical     | 1-to-1    | Each booking is associated with exactly **one user** and **one vehicle** |




# Query 1
SELECT 
  u.name AS Customer_Name,
  v.v_name AS Vehical_Name
FROM booking b
JOIN vehical v ON b.v_id = v.v_id
JOIN user_table u ON b.u_id = u.u_id;


#Query1 respective table =>
| No. | Customer Name | Vehicle Name    |
| --- | ------------- | --------------- |
| 1   | Alice         | Toyota Corolla  |
| 2   | Asad          | Honda Civic     |
| 3   | Bob           | Nissan Sunny    |
| 4   | Carol         | Suzuki Alto     |
| 5   | David         | Hyundai Elantra |
| 6   | Eva           | Yamaha R15      |
| 7   | Frank         | Honda CBR       |
| 8   | Grace         | Suzuki Gixxer   |
| 9   | Harry         | Bajaj Pulsar    |
| 10  | Irene         | TVS Apache      |

#query 1 Explanation 

booking connects users and vehicles

JOIN vehical → get vehicle name

JOIN user_table → get user name

Shows only records where booking exists (INNER JOIN)


#Query 2 
select * from vehical v 
where not exists (
  select 1 
  from booking b 
  where b.v_id =v.v_id
)
#Query 2 respective table =>
| ID | Vehicle Name    | Type  | Year | Plate No  | Status      | Rent (৳) |
| -- | --------------- | ----- | ---- | --------- | ----------- | -------- |
| 16 | Toyota Hiace    | Car   | 2016 | CAR-106   | Available   | 700.00   |
| 17 | Honda City      | Car   | 2018 | CAR-107   | Maintenance | 750.00   |
| 18 | Hero Splendor   | Bike  | 2015 | BIKE-206  | Available   | 250.00   |
| 19 | Mahindra Truck  | Truck | 2014 | TRUCK-306 | Available   | 1300.00  |
| 21 | Toyota Corolla  | Car   | 2009 | ABC-1234  | Available   | 500.00   |
| 22 | Honda Civic     | Car   | 2012 | CAR-1023  | Rented      | 600.00   |
| 23 | Nissan Sunny    | Car   | 2010 | CAR-1033  | Maintenance | 450.00   |
| 24 | Suzuki Alto     | Car   | 2015 | CAR-1043  | Available   | 400.00   |
| 25 | Hyundai Elantra | Car   | 2014 | CAR-1035  | Rented      | 650.00   |
| 26 | Toyota Corolla  | Car   | 2009 | ABCe-1234 | Available   | 500.00   |

#query 2 Explanation

For each vehicle, DB checks booking table

If no matching booking exists, vehicle is shown

EXISTS returns TRUE/FALSE, not data


# Query 3

select * from vehical
where v_type='car' and status ='available'

#Query 3 respective table =>

| ID | Vehicle Name   | Type | Year | Plate No  | Status    | Rent   |
| -- | -------------- | ---- | ---- | --------- | --------- | ------ |
| 1  | Toyota Corolla | Car  | 2009 | ABC-123   | Available | 500.00 |
| 4  | Suzuki Alto    | Car  | 2015 | CAR-104   | Available | 400.00 |



# Query 3 Explanation 

Filters by vehicle type = car

Filters by availability status

Uses only vehical table

# Query 4

 select v_name as vehical_name, count (*) as total_booking from vehical
group by v_name 
having count(*)>2

#Query 4 respective table =>


| Vehicle Name    | Total Bookings |
| --------------- | -------------- |
| Honda Civic     | 4              |
| Nissan Sunny    | 3              |
| Toyota Corolla  | 4              |
| Hyundai Elantra | 3              |
| Suzuki Alto     | 3              |


# Query 4 Explanation 

JOIN connects vehicles with bookings

COUNT(*) counts bookings per vehicle

GROUP BY groups same vehicle

HAVING filters aggregated result




















