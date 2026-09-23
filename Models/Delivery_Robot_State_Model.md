\# 2. State Model



\## 2.1 Purpose



This state model represents the operational states of the autonomous delivery robot and the events that cause transitions between those states.



\## 2.2 States



| State                 | Description                                                                                                            |

| --------------------- | ---------------------------------------------------------------------------------------------------------------------- |

| \*\*IDLE\*\*              | Robot is waiting at the warehouse. It remains idle until a delivery request is received.                               |

| \*\*NAVIGATING\*\*        | Robot is moving toward the delivery destination while continuously monitoring its surroundings and battery status.     |

| \*\*AVOIDING\_OBSTACLE\*\* | Robot has detected an obstacle and temporarily stops normal navigation while avoiding it.                              |

| \*\*DELIVERING\*\*        | Robot has reached the destination and performs the package delivery.                                                   |

| \*\*RETURNING\*\*         | Robot returns to the warehouse after successful delivery or when critical battery level is detected during navigation. |



\## 2.3 State Transitions



| Current State     | Event / Condition         | Next State        | Requirement |

| ----------------- | ------------------------- | ----------------- | ----------- |

| IDLE              | Delivery Request Received | NAVIGATING        | R2          |

| NAVIGATING        | Obstacle Detected         | AVOIDING\_OBSTACLE | R4          |

| AVOIDING\_OBSTACLE | Obstacle Avoided          | NAVIGATING        | R5          |

| NAVIGATING        | Destination Reached       | DELIVERING        | R6          |

| DELIVERING        | Delivery Successful       | RETURNING         | R7          |

| NAVIGATING        | Critical Battery          | RETURNING         | R8          |

| RETURNING         | Warehouse Reached         | IDLE              | R9          |



\## 2.4 State Flow



```text

&#x20;                   Delivery Request

&#x20;                        |

&#x20;                        v

&#x20;                     +------+

&#x20;                     | IDLE |

&#x20;                     +------+

&#x20;                        |

&#x20;                        v

&#x20;                +---------------+

&#x20;                |  NAVIGATING   |

&#x20;                +---------------+

&#x20;                   |          |

&#x20;      Obstacle     |          | Destination Reached

&#x20;      Detected     |          v

&#x20;                   |      +-----------+

&#x20;                   |      | DELIVERING|

&#x20;                   |      +-----------+

&#x20;                   |           |

&#x20;                   |           | Delivery Successful

&#x20;                   |           v

&#x20;                   |      +-----------+

&#x20;                   |      | RETURNING |

&#x20;                   |      +-----------+

&#x20;                   |           |

&#x20;                   |           | Warehouse Reached

&#x20;                   |           v

&#x20;                   |        +------+

&#x20;                   |        | IDLE |

&#x20;                   |        +------+

&#x20;                   |

&#x20;                   v

&#x20;            +-------------------+

&#x20;            | AVOIDING\_OBSTACLE |

&#x20;            +-------------------+

&#x20;                   |

&#x20;                   | Obstacle Avoided

&#x20;                   |

&#x20;                   +-----------> NAVIGATING



NAVIGATING -- Critical Battery --> RETURNING

```



\## 2.5 Requirement Traceability



The state model represents the following requirements:



\* \*\*R2:\*\* IDLE → NAVIGATING after a delivery request.

\* \*\*R3:\*\* NAVIGATING includes continuous surroundings monitoring.

\* \*\*R4:\*\* NAVIGATING → AVOIDING\_OBSTACLE when an obstacle is detected.

\* \*\*R5:\*\* AVOIDING\_OBSTACLE → NAVIGATING after the obstacle is avoided.

\* \*\*R6:\*\* NAVIGATING → DELIVERING when the destination is reached.

\* \*\*R7:\*\* DELIVERING → RETURNING after successful delivery.

\* \*\*R8:\*\* NAVIGATING → RETURNING when the battery becomes critical.

\* \*\*R9:\*\* RETURNING → IDLE when the warehouse is reached.

\* \*\*R10:\*\* The model does not define a direct IDLE → DELIVERING or AVOIDING\_OBSTACLE → DELIVERING transition.



\## 2.6 Modeling Scope



The assignment explicitly defines battery monitoring during navigation. Therefore, the critical-battery transition shown here starts from \*\*NAVIGATING\*\*.



The assignment does not define specific behavior for an obstacle detected while RETURNING or for a critical battery condition occurring during DELIVERING. These cases are therefore not added as state transitions in this model.



