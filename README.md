# central-system

```mermaid

classDiagram

    Trip *-- User
    Trip *-- Location : from
    Trip *-- Location : to
    Trip *-- Drone
    Trip --o Report
    Drone <|-- IGPS
    Drone <|-- IBattery
    Drone <|-- IScale
    Drone <|-- IHeading
    Report *-- ReportCategory
    ProhibitedZone *-- Location

    class Trip {
        +request_time: date_time
        +weight: float
        +distance: float
        +est_time: float
        +state: TravelState
        +cost_trip: float
        +delivered_price: float
        +total_price: float
        +packing_time: float | null
        +battery_init: float | null
        +arrival_time: float | null
        +delivery_time: float | null
        +real_distance: float | null
        +battery_arrival: float | null
        +round_trip_time: float | null
        +total_time: float | null
        +battery_end: float | null
        +canceled_time: date_time | null
        +unfinished_time: date_time | null
        
        +new()
    }

    class Location {
        +name: string | null
        +latitude: float 
        +longitude: float 
    }

    class ProhibitedZone {
        +description: string
        +since: date_time
        +to: date_time | null
        +radius: float
    }

    class User {
      +name: string 
      +lastname: string 
      +phone: int
      +email: string | null 

      +reqTravel()
    }

    class Drone {
        +name: string
        +number: int

        -get_battery()
        -get_location()
        -start_trip()
    }

    class Report {
        +timestamp: date_time
        +title: string
        +description: string
    }

    class ReportCategory {
        +name: string
    }

    class TripState {
        <<enumeration>>
        +Requested
        +Delivered
        +Finished
        +Canceled
        +Unfinished
    }

    class IGPS {
        <<interface>>
        -get_location()
    }

    class IBattery {
        <<interface>>
        -get_battery()
    }

    class IScale {
        <<interface>>
        -get_weight()
    }

    class IHeading{
        <<interface>>
        -get_heading()
    }
```
## Trip State Diagram

```mermaid
stateDiagram-v2
    [*] --> Requested : User.request()
    Requested --> Unfinished : Drone.system_error()
    Delivered --> Finished: Drone.finish_trip()
    Requested --> Canceled: User.cancel_trip()
    Delivered --> Unfinished: Drone.system_error()
    Unfinished --> [*]
    Canceled --> [*]
    Requested --> Delivered: Drone.start_return_trip()
    Finished --> [*]
```
