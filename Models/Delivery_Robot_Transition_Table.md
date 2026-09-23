\# 3. Transition Table



\## 3.1 Valid Transitions



| #  | Current State     | Event / Condition         | Next State        | Guard / Requirement | Action / Notes                                                 |

| -- | ----------------- | ------------------------- | ----------------- | ------------------- | -------------------------------------------------------------- |

| T1 | IDLE              | Delivery Request Received | NAVIGATING        | R2                  | Begin navigation toward the destination.                       |

| T2 | NAVIGATING        | Obstacle Detected         | AVOIDING\_OBSTACLE | R4                  | Stop normal navigation and begin obstacle avoidance.           |

| T3 | AVOIDING\_OBSTACLE | Obstacle Avoided          | NAVIGATING        | R5                  | Resume navigation toward the destination.                      |

| T4 | NAVIGATING        | Destination Reached       | DELIVERING        | R6                  | Begin package delivery.                                        |

| T5 | DELIVERING        | Delivery Successful       | RETURNING         | R7                  | Begin navigation back to the warehouse.                        |

| T6 | NAVIGATING        | Critical Battery          | RETURNING         | R8                  | Stop the current delivery journey and return to the warehouse. |

| T7 | RETURNING         | Warehouse Reached         | IDLE              | R9                  | Wait for the next delivery request.                            |

| T8 | IDLE              | Power-On / Initial State  | IDLE              | R1                  | Remain idle until a delivery request is received.              |



\## 3.2 Invalid Transitions



The following transitions are invalid because they conflict with explicit requirements in the specification.



| #  | Current State     | Event / Condition         | Proposed Next State | Requirement Violated | Reason                                                                                                                                 |

| -- | ----------------- | ------------------------- | ------------------- | -------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |

| I1 | IDLE              | Delivery Request Received | DELIVERING          | R2, R10              | A delivery request must first cause the robot to enter NAVIGATING; it cannot directly enter DELIVERING.                                |

| I2 | IDLE              | Power-On / Initial State  | DELIVERING          | R1, R10              | The robot must remain IDLE after power-on until a delivery request is received.                                                        |

| I3 | IDLE              | Delivery Request Received | AVOIDING\_OBSTACLE   | R2                   | A delivery request causes the robot to begin NAVIGATING; obstacle avoidance is entered when an obstacle is detected during navigation. |

| I4 | AVOIDING\_OBSTACLE | Obstacle Avoided          | DELIVERING          | R5, R10              | After avoiding the obstacle, the robot must return to NAVIGATING before it can reach the destination and enter DELIVERING.             |

| I5 | AVOIDING\_OBSTACLE | Destination Reached       | DELIVERING          | R5, R10              | The robot must resume NAVIGATING after obstacle avoidance; delivery begins only after the destination is reached from NAVIGATING.      |



\## 3.3 Notes on Unspecified Transitions



Not every possible state/event combination is explicitly defined by the assignment. Therefore, combinations such as:



\* DELIVERING + Obstacle Detected

\* RETURNING + Delivery Request Received

\* RETURNING + Obstacle Detected

\* DELIVERING + Critical Battery



are treated as \*\*unspecified\*\*, rather than being labeled invalid, because the requirements do not explicitly define those behaviors.



This distinction prevents the transition model from introducing requirements that are not present in the specification.



\## 3.4 Traceability



The valid transitions trace back to the requirements as follows:



\* \*\*T1 → R2\*\*

\* \*\*T2 → R4\*\*

\* \*\*T3 → R5\*\*

\* \*\*T4 → R6\*\*

\* \*\*T5 → R7\*\*

\* \*\*T6 → R8\*\*

\* \*\*T7 → R9\*\*

\* \*\*T8 → R1\*\*



The invalid transitions demonstrate compliance with \*\*R1, R2, R5, and R10\*\*.



