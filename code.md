```
Building [icon: building, color: blue] {
    building_id int pk
    building_name string
    address string
    city string
    total_floors int
    total_elevators int
    created_at timestamp
}

Floor [icon: layers, color: indigo] {
    floor_id int pk
    building_id int
    floor_number int
    floor_name string
    is_ground_floor boolean
    description text
}

Elevator [icon: arrow-up-down, color: emerald] {
    elevator_id int pk
    building_id int
    elevator_name string
    shaft_number string
    capacity_person int
    max_weight decimal
    current_status string
    last_maintenance_date date
    is_active boolean
}

ElevatorFloor [icon: link, color: amber] {
    elevator_floor_id int pk
    elevator_id int
    floor_id int
    can_stop boolean
}

RideRequest [icon: bell, color: orange] {
    request_id int pk
    building_id int
    floor_id int
    requested_floor int
    request_type string
    passenger_count int
    request_timestamp timestamp
    status string
}

RideAssignment [icon: route, color: rose] {
    assignment_id int pk
    request_id int
    elevator_id int
    assigned_at timestamp
    pickup_time timestamp
    dropoff_time timestamp
    status string
}

RideLog [icon: clipboard-list, color: purple] {
    ride_log_id int pk
    assignment_id int
    elevator_id int
    request_id int
    pickup_floor_id int
    dropoff_floor_id int
    start_time timestamp
    end_time timestamp
    duration_seconds int
    passenger_count int
    energy_consumed decimal
}

MaintenanceRecord [icon: wrench, color: red] {
    maintenance_id int pk
    elevator_id int
    maintenance_type string
    start_date timestamp
    end_date timestamp
    technician_name string
    description text
    status string
    cost decimal
}

```

// ==================== RELATIONSHIPS ====================

// Building to Floor

Floor.building_id > Building.building_id

// Building to Elevator

Elevator.building_id > Building.building_id

// Elevator serves multiple Floors (Many-to-Many)

ElevatorFloor.elevator_id > Elevator.elevator_id
ElevatorFloor.floor_id > Floor.floor_id

// RideRequest from Floor

RideRequest.floor_id > Floor.floor_id
RideRequest.building_id > Building.building_id

// Request to Assignment

RideAssignment.request_id > RideRequest.request_id

// Assignment to Elevator
RideAssignment.elevator_id > Elevator.elevator_id

// RideLog linked to Assignment and Elevator

RideLog.assignment_id > RideAssignment.assignment_id
RideLog.elevator_id > Elevator.elevator_id
RideLog.request_id > RideRequest.request_id
RideLog.pickup_floor_id > Floor.floor_id
RideLog.dropoff_floor_id > Floor.floor_id

// Maintenance on Elevator
MaintenanceRecord.elevator_id > Elevator.elevator_id