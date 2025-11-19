# ER Diagram Workshop – Submission

## Objective

To understand and apply ER modeling concepts by creating ER diagrams for real-world applications.

## Purpose

Gain hands-on experience in designing ER diagrams that represent database structure including entities, relationships, attributes, and constraints.

---

## Scenario A: City Fitness Club Management

### Business Context

FlexiFit Gym wants a database to manage its members, trainers, fitness programs, and payments in an organized and scalable way.

### Requirements

* Members are stored with details like **name**, **membership type**, and **start date**.
* Each table has a **Primary Key**:

  * `member_id` in **Members**
  * `program_id` in **Programs**
  * `trainer_id` in **Trainers**
  * `payment_id` in **Payments**
* **Foreign keys** maintain referential integrity:

  * `member_id` in **Payments** references **Members**.
* **Many-to-Many relationships**:

  * Between **Members** and **Programs** using **Member_Programs**.
  * Between **Programs** and **Trainers** using **Program_Trainers**.
* **One-to-Many relationships**:

  * One **Member** can make many **Payments**.
  * Possible member–trainer session linkages for detailed tracking.
* **Composite Keys** in join tables (e.g., `member_id + program_id`) manage M:N relationships by splitting them into two 1:N relationships.

### ER Diagram
<img width="840" height="730" alt="image" src="https://github.com/user-attachments/assets/a76519f6-6c5c-45ea-94c6-a2bbb4acf460" />


### Entities and Attributes

| Entity           | Attributes (PK, FK)                                       | Notes                                                     |
| ---------------- | --------------------------------------------------------- | --------------------------------------------------------- |
| Members          | **member_id (PK)**, name, membership_type, start_date     | Stores member personal and membership details.            |
| Trainers         | **trainer_id (PK)**, name                                 | Stores trainer information.                               |
| Programs         | **program_id (PK)**, program_name                         | Stores gym programs (e.g., Yoga, Zumba, Weight Training). |
| Payments         | **payment_id (PK)**, member_id (FK), payment_date, amount | Tracks payments made by members; `member_id` → Members.   |
| Member_Programs  | **member_id (PK, FK)**, **program_id (PK, FK)**           | Join table for M:N between Members and Programs.          |
| Program_Trainers | **program_id (PK, FK)**, **trainer_id (PK, FK)**          | Join table for M:N between Programs and Trainers.         |

---

### Relationships and Constraints

| Relationship                               | Cardinality                      | Participation                              | Notes                                                                                 |
| ------------------------------------------ | -------------------------------- | ------------------------------------------ | ------------------------------------------------------------------------------------- |
| Members – Payments                         | 1 : N                            | Members: partial, Payments: total          | A member may have 0 or many payments; each payment must belong to exactly one member. |
| Members – Programs (via Member_Programs)   | M : N (implemented as 1:N + 1:N) | Both sides typically partial               | A member can join multiple programs; a program can have multiple members.             |
| Programs – Trainers (via Program_Trainers) | M : N                            | Both sides partial/optional                | A program may have multiple trainers; a trainer may handle multiple programs.         |
| Members – Member_Programs                  | 1 : N                            | Members: partial, Member_Programs: total   | Each row in Member_Programs must reference a valid member.                            |
| Programs – Member_Programs                 | 1 : N                            | Programs: partial, Member_Programs: total  | Each row in Member_Programs must reference a valid program.                           |
| Programs – Program_Trainers                | 1 : N                            | Programs: partial, Program_Trainers: total | Links programs to trainers.                                                           |
| Trainers – Program_Trainers                | 1 : N                            | Trainers: partial, Program_Trainers: total | Links trainers to programs.                                                           |

---

### Assumptions

* `Member_Programs` and `Program_Trainers` are explicitly modeled as **associative entities** to handle M:N relationships.
* Session-level tracking between a specific member and trainer (e.g., personal training sessions, attendance) can be added as a separate entity (e.g., `Sessions`) if required, but is not mandatory in the current minimal design.
* Data types:

  * IDs are numeric (INT).
  * Dates are stored as DATE (or DATETIME if time is needed).
  * `amount` in **Payments** is numeric/decimal.
* Membership type is stored as a simple attribute (e.g., "Monthly", "Annual", "Premium") and can be normalized into a separate table if needed later.

---

## Scenario B: City Library Event & Book Lending System

### Business Context

The Central Library wants to manage book lending, library members, events, rooms, and fines in a structured database.

### Requirements

* **Primary keys**:

  * `member_id` in **Members**
  * `book_id` in **Books**
  * `event_id` in **Events**
  * `room_number` in **Rooms**
  * `fine_id` in **Fines**
* **Foreign keys & referential integrity**:

  * `member_id` in **Fines** references **Members**, so each fine is linked to a valid member.
* Event participation and speakers can be tracked using additional fields/tables.
* **One-to-Many relationships** include:

  * Members borrowing multiple books.
  * Members having multiple fines.
  * Rooms hosting multiple bookings.
  * Members participating in multiple events.
* Design is normalized and modular with separate tables for books, events, rooms, and fines to reduce redundancy and improve scalability.

### ER Diagram
<img width="857" height="685" alt="image" src="https://github.com/user-attachments/assets/11fac8ac-bfde-4394-b778-d97f9c784249" />


---

### Entities and Attributes



| Entity               | Attributes (PK, FK)                                                            | Notes                                                                                                    |
| -------------------- | ------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------- |
| Members              | **member_id (PK)**, name, email                                                | Library members.                                                                                         |
| Books                | **book_id (PK)**, title, author, category, due_date                            | Books available/issued; `due_date` currently stored at book level as per given structure.                |
| Events               | **event_id (PK)**, event_name, event_date                                      | Library events (talks, workshops, cultural programs).                                                    |
| Rooms                | **room_number (PK)**, room_type                                                | Rooms used for events or study.                                                                          |
| Fines                | **fine_id (PK)**, member_id (FK), amount                                       | Fines imposed on members; `member_id` → Members.                                                         |
| Book_Loans*          | **member_id (PK, FK)**, **book_id (PK, FK)**, loan_date, return_date, due_date | *Assumed associative entity to properly track lending, even though not explicitly in the structure text. |
| Event_Registrations* | **event_id (PK, FK)**, **member_id (PK, FK)**                                  | *Assumed for members registering/participating in events.                                                |
| Room_Bookings*       | **room_number (PK, FK)**, **event_id (PK, FK)**, booking_start, booking_end    | *Assumed for linking rooms to events.                                                                    |
| Speakers*            | **speaker_id (PK)**, name, description                                         | *Assumed entity for event speakers/authors.                                                              |
| Event_Speakers*      | **event_id (PK, FK)**, **speaker_id (PK, FK)**                                 | *Assumed to manage M:N between Events and Speakers.                                                      |


---

### Relationships and Constraints

| Relationship                  | Cardinality    | Participation                                | Notes                                                                        |
| ----------------------------- | -------------- | -------------------------------------------- | ---------------------------------------------------------------------------- |
| Members – Fines               | 1 : N          | Members: partial, Fines: total               | A member may have 0 or many fines; every fine must belong to one member.     |
| Members – Book_Loans          | 1 : N          | Members: partial, Book_Loans: total          | A member can borrow many books; each loan row must belong to a valid member. |
| Books – Book_Loans            | 1 : N          | Books: partial, Book_Loans: total            | A book can appear in many loan records over time.                            |
| Members – Event_Registrations | 1 : N          | Members: partial, Event_Registrations: total | A member may register for multiple events.                                   |
| Events – Event_Registrations  | 1 : N          | Events: partial, Event_Registrations: total  | An event can have many participants.                                         |
| Events – Room_Bookings        | 1 : N or 1 : 1 | Events: partial, Room_Bookings: total        | An event may be assigned to one or multiple rooms depending on design.       |
| Rooms – Room_Bookings         | 1 : N          | Rooms: partial, Room_Bookings: total         | A room can host many bookings over time.                                     |
| Events – Event_Speakers       | 1 : N          | Events: partial, Event_Speakers: total       | An event may have multiple speakers.                                         |
| Speakers – Event_Speakers     | 1 : N          | Speakers: partial, Event_Speakers: total     | A speaker can speak at multiple events.                                      |

---

### Assumptions

* The original **Books** table’s `due_date` attribute is treated as the **current loan’s due date** and may be moved to **Book_Loans** in a more normalized design.
* Separate entities like **Book_Loans**, **Event_Registrations**, **Room_Bookings**, **Speakers**, and **Event_Speakers** are introduced based on the narrative description and requirements for lending, events, rooms, and speakers.
* Fines are calculated outside the ER model (e.g., via business rules) and then stored in **Fines**.
* Dates are stored using DATE/DATETIME types; amounts are stored as numeric/decimal.

---

## Scenario C: Restaurant Table Reservation & Ordering

### Business Context

A popular restaurant wants to manage reservations, walk-in customers, orders, dishes, billing, and waiters using a structured database.

### Requirements

* Each table has a **Primary Key**:

  * `customer_id` in **Customers**
  * `reservation_id` in **Reservations**
  * `dish_id` in **Dishes**
  * `order_id` in **Order**
  * `bill_id` in **Bill**
  * `waiter_id` in **Waiters**
* **Foreign keys**:

  * `customer_id` in **Reservations** references **Customers**.
  * `reservation_id` in **Bill** references **Reservations**.
  * `waiter_id` in **Bill** references **Waiters**.
* **Orders table** stores detailed order information (dish name and quantity).
* **One-to-Many relationships**:

  * One customer can have multiple reservations.
  * One reservation can have multiple orders.
* Each bill corresponds to one reservation and is linked to a waiter.

*(Note: To satisfy the requirement “Each reservation can have multiple orders”, we logically assume `reservation_id` should be present as FK in the Orders table.)*

### ER Diagram
<img width="878" height="663" alt="image" src="https://github.com/user-attachments/assets/20fb1a17-1f06-4bbc-a2db-bbcbd1ea5197" />


---

### Entities and Attributes

| Entity       | Attributes (PK, FK)                                                                                     | Notes                                                                                                                                 |
| ------------ | ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| Customers    | **customer_id (PK)**, name, contact_number                                                              | Stores customer details.                                                                                                              |
| Reservations | **reservation_id (PK)**, customer_id (FK), reservation_date, reservation_time, num_guests, table_number | Links customers to table reservations.                                                                                                |
| Dishes       | **dish_id (PK)**, dish_name, category (starter/main/dessert)                                            | Menu items with category.                                                                                                             |
| Orders       | **order_id (PK)**, reservation_id (FK)*, dish_id (FK)*, quantity                                        | Stores order lines for a reservation (*extended from given: replacing plain dish_name with dish_id FK and adding reservation_id FK*). |
| Bill         | **bill_id (PK)**, reservation_id (FK), bill_amount, waiter_id (FK)                                      | Billing information per reservation.                                                                                                  |
| Waiters      | **waiter_id (PK)**, name                                                                                | Staff serving customers.                                                                                                              |

---

### Relationships and Constraints

| Relationship             | Cardinality | Participation                           | Notes                                                                              |
| ------------------------ | ----------- | --------------------------------------- | ---------------------------------------------------------------------------------- |
| Customers – Reservations | 1 : N       | Customers: partial, Reservations: total | One customer can make many reservations; each reservation belongs to one customer. |
| Reservations – Orders    | 1 : N       | Reservations: partial, Orders: total    | One reservation can have multiple ordered dishes.                                  |
| Reservations – Bill      | 1 : 1       | Reservations: total, Bill: total        | One bill per reservation (assuming 1:1).                                           |
| Waiters – Bill           | 1 : N       | Waiters: partial, Bill: total           | A waiter can handle many bills; each bill references one waiter.                   |
| Dishes – Orders          | 1 : N       | Dishes: partial, Orders: total          | Each order line references a single dish; a dish can appear in many orders.        |

---

### Assumptions

* **Orders** table is logically extended to include:

  * `reservation_id (FK)` to link orders to a reservation.
  * `dish_id (FK)` instead of `dish_name` for better normalization; `dish_name` can remain as a descriptive attribute in **Dishes**.
* Walk-in customers are either:

  * Created as **Customers** with minimal info and given a **Reservation** (for that visit), or
  * Handled via a special "walk-in" flag; implementation choice is outside core ER but noted.
* One **Bill** is generated per **Reservation** (no split bills modeled).
* `bill_amount` includes both food and service charges; breakdown can be achieved with additional attributes or tables if needed.
* Time-related attributes use DATE/DATETIME types; numeric values like `bill_amount` and `quantity` use numeric/decimal/integer types.

---


