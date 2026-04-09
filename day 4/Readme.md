Multi-Zone Event Parking System - README
Overview
This database schema is designed for large-scale event parking management, specifically for events like Comic-Con India. The system handles vehicle entry, spot allocation, reserved categories, sessions, tickets, and payments. It is built to manage high traffic across multiple zones and levels.

Entities and Attributes
Vehicle
id: Primary Key

vehicleNumber: Unique registration number

vehicleCategoryId: Foreign Key (Type of vehicle)

ownerName: Name of the owner/driver

createdAt: Date of registration

Vehicle Category
id: Primary Key

name: bike, car, SUV, EV, cab

Parking Zone
id: Primary Key

name: Area name (e.g., North Zone)

description: Details about the zone

Parking Level
id: Primary Key

zoneId: Foreign Key (Link to Zone)

levelNumber: Integer representation of the floor/level

Parking Spot
id: Primary Key

levelId: Foreign Key (Link to Level)

spotNumber: Unique identifier for the slot

spotCategoryId: Foreign Key (Link to Spot Category)

isAvailable: Boolean (True/False)

Parking Spot Category
id: Primary Key

name: general, VIP, exhibitor, staff, EV charging, cosplayer

Access Category
id: Primary Key

name: VIP, exhibitor, staff, cosplayer

description: Specific access rights for the visitor

Parking Ticket
id: Primary Key

ticketNumber: Unique serial number

vehicleId: Foreign Key (Link to Vehicle)

issuedAt: Timestamp of generation

Parking Session
id: Primary Key

vehicleId: Foreign Key

spotId: Foreign Key

ticketId: Foreign Key

accessCategoryId: Foreign Key

entryTime: Timestamp of entry

exitTime: Timestamp of exit

status: active or completed

Payment
id: Primary Key

sessionId: Foreign Key (Link to Session)

amount: Total calculated fee

paymentMethod: cash, UPI, card

status: paid, pending, failed

paymentTime: Timestamp of transaction

Core Relationships
Vehicle & Session: A vehicle can have multiple parking sessions to allow for multiple entries over the event duration.

Spot & Session: A parking spot can be assigned to different sessions at different times once it becomes available.

Hierarchy: One Zone contains many Levels, and one Level contains many Spots.

Tracking: The Parking Session table acts as the central link between the vehicle, its assigned spot, and the issued ticket.

Payment: Each completed session is linked to one payment record for financial tracking.

System Logic
Spot Availability: Availability is tracked through the isAvailable flag in the Parking Spot table. When a session starts, it is set to false; when a session ends, it is set to true.

Reserved Access: The spotCategoryId ensures that specific spots are reserved only for corresponding accessCategory types (e.g., VIP spots for VIP guests).

Fee Calculation: Fees are calculated during the exit process based on the duration between entryTime and exitTime.

Real-time Tracking: Any session with an exitTime marked as NULL indicates a vehicle currently parked inside the facility.