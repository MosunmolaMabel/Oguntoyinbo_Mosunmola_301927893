## Portfolio 1 Database Design and  Modelling
## Case Study
A Ride-hailing and parcel logistics platform operating in major metropolitan areas using fictional operational data. It manages real-time transport bookings and localized parcel dispatch services through a centralized relational structure. The core focus is  connecting registered passengers and merchants with available drivers for point-to-point journeys and deliveries
## Users
Main system actors include Passengers, Drivers,Dispatch Clerks and System Administrators. Passengers and Dispatch Clerks create ride requests and logistics dispatches, Drivers accept and complete assigned trips, while System Administrators manage user accounts and vehicle compliance data.
## Main Operations
Key operations include registering new passenger and driver accounts,recording vehicle specifications,logging new ride requests,assigning available drivers to pending dispatch orders,updating trip statuses upon completion and processing post-trip payment transactions.
## Business rules
1.Each driver is uniquely identified by a DriverID, and each vehicle is uniquely identified by a vehicleID.
2.A registered driver may operate only one active vehicle at any given time,but a vehicle can be assigned to multiple drivers across different shifts over time.
3.Every ride or parcel dispatch must be associated with exactly one registered passenger or merchant account.
4.Estimated trip fares must be a positive decimal value greater than zero.
5.The scheduled pick-up date and time must be equal to or later than the booking creation date and time.
6.A driver cannot accept a new trip request while currently marked with an active status of "In Progress".
7.Completed trips must log a valid payment status of either "Paid","Pending", or "Refunded".
8.A dispatch cancellation cannot be processed once the driver status is updated to "Picked Up".
## Scope
The scope includes passenger profiles,driver and vehicle registrations,ride bookings,parcel dispatch logs and basic payment transaction processing.Excluded from this database are real-time GPS tracking coordinates,dynamic algorithmic surge pricing calculations,route mapping integration and third-party driver background checks.