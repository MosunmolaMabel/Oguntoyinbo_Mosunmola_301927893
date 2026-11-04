
Ride and Parcel Dispatch Service Database Design
1. Entities — what the database keeps records of
I use six entities:

Entity	Information stored
account	A registered passenger or merchant
driver	A registered driver
vehicle	A registered vehicle
driver_vehicle_assignment	Which vehicle a driver operates during an assignment
booking	A ride booking or parcel dispatch, including its status
payment	Payment transactions for bookings
Ride and parcel records share the booking entity; booking_type distinguishes them. For example, a parcel dispatch and a ride can each have an estimated fare and scheduled pick-up time.

2. Attributes and keys — fields, PostgreSQL types, PKs, FKs, unique fields, and required or optional values
A primary key (PK) uniquely identifies a row. A foreign key (FK) links it to another table. Required fields cannot be missing (NULL).

Entity	Attributes and keys
account	account_id BIGINT PK; account_type VARCHAR(10), full_name VARCHAR(120), email VARCHAR(254) required; email unique. account_type is Passenger or Merchant.
driver	driver_id BIGINT PK; full_name VARCHAR(120), license_number VARCHAR(50) required; license_number unique.
vehicle	vehicle_id BIGINT PK; registration_number VARCHAR(20) required and unique; make VARCHAR(60), model VARCHAR(60) optional.
driver_vehicle_assignment	assignment_id BIGINT PK; driver_id BIGINT, vehicle_id BIGINT required FKs; active_from TIMESTAMPTZ required; active_to TIMESTAMPTZ optional.
booking	booking_id BIGINT PK; account_id BIGINT required FK; assignment_id BIGINT optional FK until assigned; booking_type VARCHAR(10), status VARCHAR(20), created_at TIMESTAMPTZ, scheduled_pickup_at TIMESTAMPTZ, estimated_fare NUMERIC(10,2) required.
payment	payment_id BIGINT PK; booking_id BIGINT required FK; payment_status VARCHAR(10) required.
BIGINT stores whole numbers, VARCHAR stores text, NUMERIC(10,2) stores decimal amounts, and TIMESTAMPTZ stores date and time. A payment row is recorded for each transaction; a booking can have zero or more payment records.

3. Relationships — how the tables connect
Relationship	Meaning, including zero-record cases
account → booking	An account can have zero or more bookings; each booking belongs to exactly one account.
driver → driver_vehicle_assignment	A driver can have zero or more assignments over time; each assignment has exactly one driver.
vehicle → driver_vehicle_assignment	A vehicle can have zero or more assignments over time; each assignment has exactly one vehicle.
driver_vehicle_assignment → booking	An assignment can have zero or more bookings; a booking has zero or one assignment until a driver and vehicle are assigned.
booking → payment	A booking can have zero or more payments; each payment belongs to exactly one booking.
4. Business rules and constraints — match every numbered rule to a database check or operation
The driver_id and vehicle_id primary keys uniquely identify drivers and vehicles.
Assignments record driver–vehicle pairings over time. A partial unique index allows at most one assignment per driver where active_to is missing. Assignment dates must be valid; preventing overlapping assignments for a vehicle requires an additional database check or transaction.
A required booking.account_id foreign key connects every booking to exactly one registered account.
A database check requires estimated_fare > 0.
A database check requires scheduled_pickup_at >= created_at.
The booking-acceptance operation checks whether the assigned driver already has a booking with status “In Progress” and rejects another. Because the driver is linked through an assignment, this needs transaction logic or a database trigger.
Payment status is limited to “Paid”, “Pending”, or “Refunded”. The completion operation requires a payment record with one of these statuses.
The cancellation operation rejects a dispatch whose status is “Picked Up”; this status check belongs in the operation or a database trigger.
5. Design assumptions — up to three points, only if needed
“Driver status” in rule 8 means the dispatch’s booking status.
A driver operates one vehicle per active assignment; the assignment history identifies the vehicle used for a booking.
<<<<<<< Updated upstream
=======
I am completing MIT 8103 Portfolio 1. Turn my agreed database design
below into one Markdown file containing a Mermaid ER diagram.

MY DATABASE DESIGN
[]Ride and Parcel Dispatch Service Database Design

Entities — what the database keeps records of I use six entities:
Entity Information stored account A registered passenger or merchant driver A registered driver vehicle A registered vehicle driver_vehicle_assignment Which vehicle a driver operates during an assignment booking A ride booking or parcel dispatch, including its status payment Payment transactions for bookings Ride and parcel records share the booking entity; booking_type distinguishes them. For example, a parcel dispatch and a ride can each have an estimated fare and scheduled pick-up time.

Attributes and keys — fields, PostgreSQL types, PKs, FKs, unique fields, and required or optional values A primary key (PK) uniquely identifies a row. A foreign key (FK) links it to another table. Required fields cannot be missing (NULL).
Entity Attributes and keys account account_id BIGINT PK; account_type VARCHAR(10), full_name VARCHAR(120), email VARCHAR(254) required; email unique. account_type is Passenger or Merchant. driver driver_id BIGINT PK; full_name VARCHAR(120), license_number VARCHAR(50) required; license_number unique. vehicle vehicle_id BIGINT PK; registration_number VARCHAR(20) required and unique; make VARCHAR(60), model VARCHAR(60) optional. driver_vehicle_assignment assignment_id BIGINT PK; driver_id BIGINT, vehicle_id BIGINT required FKs; active_from TIMESTAMPTZ required; active_to TIMESTAMPTZ optional. booking booking_id BIGINT PK; account_id BIGINT required FK; assignment_id BIGINT optional FK until assigned; booking_type VARCHAR(10), status VARCHAR(20), created_at TIMESTAMPTZ, scheduled_pickup_at TIMESTAMPTZ, estimated_fare NUMERIC(10,2) required. payment payment_id BIGINT PK; booking_id BIGINT required FK; payment_status VARCHAR(10) required. BIGINT stores whole numbers, VARCHAR stores text, NUMERIC(10,2) stores decimal amounts, and TIMESTAMPTZ stores date and time. A payment row is recorded for each transaction; a booking can have zero or more payment records.

Relationships — how the tables connect Relationship Meaning, including zero-record cases account → booking An account can have zero or more bookings; each booking belongs to exactly one account. driver → driver_vehicle_assignment A driver can have zero or more assignments over time; each assignment has exactly one driver. vehicle → driver_vehicle_assignment A vehicle can have zero or more assignments over time; each assignment has exactly one vehicle. driver_vehicle_assignment → booking An assignment can have zero or more bookings; a booking has zero or one assignment until a driver and vehicle are assigned. booking → payment A booking can have zero or more payments; each payment belongs to exactly one booking.
Business rules and constraints — match every numbered rule to a database check or operation The driver_id and vehicle_id primary keys uniquely identify drivers and vehicles. Assignments record driver–vehicle pairings over time. A partial unique index allows at most one assignment per driver where active_to is missing. Assignment dates must be valid; preventing overlapping assignments for a vehicle requires an additional database check or transaction. A required booking.account_id foreign key connects every booking to exactly one registered account. A database check requires estimated_fare > 0. A database check requires scheduled_pickup_at >= created_at. The booking-acceptance operation checks whether the assigned driver already has a booking with status “In Progress” and rejects another. Because the driver is linked through an assignment, this needs transaction logic or a database trigger. Payment status is limited to “Paid”, “Pending”, or “Refunded”. The completion operation requires a payment record with one of these statuses. The cancellation operation rejects a dispatch whose status is “Picked Up”; this status check belongs in the operation or a database trigger.
Design assumptions — up to three points, only if needed “Driver status” in rule 8 means the dispatch’s booking status. A driver operates one vehicle per active assignment; the assignment history identifies the vehicle used for a booking.

Start with the title: "[Ride-Hailing/Logistics sytems] ER Diagram".
Include one fenced mermaid block starting with erDiagram.
Use direction LR so the diagram runs left to right.
Use exactly the tables, fields, types and relationships in my design.
Mark primary keys PK, foreign keys FK and unique fields UK.
Show whether each relationship means exactly one, zero or one,
or zero or many. Do not add or change any business rules.
After the diagram, explain PK, FK, UK and the relationship symbols
in simple words. Include one small example from my case study.
State which fields are optional and briefly note any rules that
need additional constraints or transaction logic.
Write this as a short assessment answer in present tense, not a plan.
Return only the Markdown file contents, without an outer code fence.
Do not generate SQL or claim anything has been executed.
>>>>>>> Stashed changes
