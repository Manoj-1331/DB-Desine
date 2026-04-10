# Smart Elevator Control

## Overview

This database schema is designed for LiftGrid Systems, an infrastructure platform that manages elevator operations across multiple buildings such as corporate towers, malls, hospitals, and residential complexes.

The system handles real-time elevator requests, ride allocation, elevator movement tracking, maintenance history, and performance monitoring.

It follows a clean separation between **infrastructure data (buildings, floors, elevators)** and **dynamic operational data (requests, rides, status, maintenance)**, making it scalable and production-ready.

---

## Entities and Attributes

### 1. Infrastructure Management

* **Building**
  Stores building-level information.

* **Floor**
  Represents floors inside a building.

* **Elevator Shaft**
  Physical shafts in which elevators operate.

* **Elevator**
  Actual elevator units assigned to shafts.

---

### 2. Configuration & Serving Logic

* **Elevator Floor (Junction Table)**
  Handles Many-to-Many relationship between elevators and floors.
  Allows:

  * One elevator → multiple floors
  * One floor → multiple elevators

---

### 3. Request & Ride Flow

* **Floor Request**
  Generated when a user presses a button (Up/Down).

* **Ride Assignment**
  Maps a request to a specific elevator.

* **Ride Log**
  Stores completed ride history (start time, end time, status).

---

### 4. Operational Monitoring

* **Elevator Status**
  Tracks real-time state (idle, moving, maintenance).

* **Maintenance**
  Stores full maintenance history without overwriting past records.

---

## Core Relationships Logic

* One building has multiple floors (1:N)
* One building has multiple elevators (1:N)
* One shaft contains exactly one elevator (1:1)
* Elevators and floors have Many-to-Many relation (via elevator_floor)
* One floor generates multiple requests (1:N)
* One request is assigned to one elevator (1:1)
* One elevator handles multiple requests (1:N)
* One elevator performs multiple rides (1:N)
* One elevator has one current status (1:1)
* One elevator has multiple maintenance records (1:N)

---

## System Flow

1. User generates request from a floor.
2. System records floor request.
3. Controller assigns request to an elevator.
4. Elevator moves and completes ride.
5. Ride log is stored.
6. Elevator status updates continuously.
7. Maintenance events are recorded separately.

---

## Key Features

* Multi-building support
* Real-time request handling
* Efficient elevator allocation
* Many-to-Many floor serving logic
* Ride history for analytics
* Independent maintenance tracking
* Clean separation of static and dynamic data

---

## Eraser.io Diagram Code

```id="liftgrid-erd"
building [icon: building] {
  id string pk
  name string
  location string
  createdAt date
}

floor [icon: layers] {
  id string pk
  buildingId string fk
  floorNumber int
}

elevator_shaft [icon: box] {
  id string pk
  buildingId string fk
  shaftNumber string
}

elevator [icon: move] {
  id string pk
  buildingId string fk
  shaftId string fk
  elevatorNumber string
}

elevator_floor [icon: grid] {
  id string pk
  elevatorId string fk
  floorId string fk
}

floor_request [icon: arrow-up] {
  id string pk
  floorId string fk
  requestTime datetime
  direction string
  status string
}

ride_assignment [icon: link] {
  id string pk
  requestId string fk
  elevatorId string fk
  assignedAt datetime
}

ride_log [icon: clock] {
  id string pk
  elevatorId string fk
  requestId string fk
  startTime datetime
  endTime datetime
  status string
}

elevator_status [icon: activity] {
  id string pk
  elevatorId string fk
  status string
  updatedAt datetime
}

maintenance [icon: tool] {
  id string pk
  elevatorId string fk
  issue string
  startDate datetime
  endDate datetime
  status string
}

building.id < floor.buildingId
building.id < elevator_shaft.buildingId
building.id < elevator.buildingId

elevator_shaft.id - elevator.shaftId

elevator.id < elevator_floor.elevatorId
floor.id < elevator_floor.floorId

floor.id < floor_request.floorId

floor_request.id - ride_assignment.requestId
elevator.id < ride_assignment.elevatorId

elevator.id < ride_log.elevatorId
floor_request.id < ride_log.requestId

elevator.id - elevator_status.elevatorId

elevator.id < maintenance.elevatorId
```

---

## Summary

Flow of system:

**Building → Floor → Request → Assignment → Ride → Logs + Status + Maintenance**

This schema represents a real-world scalable elevator control system capable of handling high-rise infrastructure efficiently with proper tracking, allocation, and analytics support.

---


