# Autonomous Delivery Robot: Requirements Specification

| Field        | Details                                  |
| ------------ | ---------------------------------------- |
| Student Name | Adan Waseem                              |
| Roll No.     | 088                                      |
| Course       | Software Verification & Validation (SVV) |
| Repository   | SVV-Lab                                  |

---

## 1. System Overview

An autonomous delivery robot receives a delivery request, navigates to the destination while monitoring its surroundings, performs the delivery, and returns to the warehouse. If the battery becomes critically low during navigation, the robot stops the current delivery journey and returns to the warehouse.

---

## 2. Functional / Behavioral Requirements

| Req. ID | Requirement                                                                                                                                                                                                              |
| ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| R1      | When powered on, the robot shall enter the `IDLE` state and remain there until a `Delivery Request Received` event occurs.                                                                                               |
| R2      | When in `IDLE` and a `Delivery Request Received` event occurs, the robot shall transition to `NAVIGATING` and begin moving toward the destination.                                                                       |
| R3      | While in `NAVIGATING`, the robot shall continuously monitor its surroundings for obstacles.                                                                                                                              |
| R4      | When in `NAVIGATING` and an `Obstacle Detected` event occurs, the robot shall stop normal navigation and transition to `AVOIDING_OBSTACLE`.                                                                              |
| R5      | When in `AVOIDING_OBSTACLE` and an `Obstacle Avoided` event occurs, the robot shall transition back to `NAVIGATING` and continue toward the destination.                                                                 |
| R6      | When in `NAVIGATING` and a `Destination Reached` event occurs, the robot shall transition to `DELIVERING` and start the delivery process.                                                                                |
| R7      | When in `DELIVERING` and a `Delivery Successful` event occurs, the robot shall transition to `RETURNING` and begin travelling back to the warehouse.                                                                     |
| R8      | When in `NAVIGATING` and a `Critical Battery` event occurs, the robot shall stop the current delivery journey and transition to `RETURNING`.                                                                             |
| R9      | When in `RETURNING` and a `Warehouse Reached` event occurs, the robot shall transition to `IDLE` and wait for another delivery request.                                                                                  |
| R10     | The robot shall not transition directly from `IDLE` to `DELIVERING` or from `AVOIDING_OBSTACLE` to `DELIVERING`. The `DELIVERING` state shall be entered only from `NAVIGATING` following a `Destination Reached` event. |

---

## 3. States

| # | State               | Short Description                                                                    |
| - | ------------------- | ------------------------------------------------------------------------------------ |
| 1 | `IDLE`              | Robot is at the warehouse, waiting for a delivery request.                           |
| 2 | `NAVIGATING`        | Robot is moving toward the destination and continuously monitoring its surroundings. |
| 3 | `AVOIDING_OBSTACLE` | Robot has temporarily stopped normal navigation and is avoiding a detected obstacle. |
| 4 | `DELIVERING`        | Robot has reached the destination and is performing the delivery.                    |
| 5 | `RETURNING`         | Robot is travelling back to the warehouse.                                           |

---

## 4. Events / Conditions

| # | Event / Condition           | Description                                                                |
| - | --------------------------- | -------------------------------------------------------------------------- |
| 1 | `Delivery Request Received` | A delivery request with a destination is received.                         |
| 2 | `Destination Reached`       | The robot reaches the delivery destination.                                |
| 3 | `Delivery Successful`       | The package has been successfully delivered.                               |
| 4 | `Warehouse Reached`         | The robot arrives back at the warehouse.                                   |
| 5 | `Obstacle Detected`         | Sensors detect an obstacle while the robot is navigating.                  |
| 6 | `Obstacle Avoided`          | The detected obstacle has been successfully avoided and the path is clear. |
| 7 | `Critical Battery`          | The robot's battery reaches a critically low level during navigation.      |

---

## 5. Assumptions and Scope Clarifications

1. `Critical Battery` behavior is specified for the `NAVIGATING` state because the assignment scenario explicitly states that the robot monitors its battery during navigation.
2. The assignment does not specify what happens if the battery becomes critical during `DELIVERING`; therefore, this case is outside the defined behavior of the current requirements.
3. The assignment does not specify obstacle handling while `RETURNING`; therefore, obstacle behavior during the return journey is outside the defined requirements.
4. Events that are not associated with a valid transition from the current state are not treated as valid state transitions unless a requirement explicitly defines their behavior.
