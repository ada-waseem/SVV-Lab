\# 4. Verification Activity \& Analysis



\## 4.1 Verification Objective



The purpose of this verification is to check whether the delivery robot state model and transition table represent the specified requirements correctly.



The verification focuses on valid transitions, prohibited transitions, requirement coverage, and detection of an intentionally introduced defect.



\---



\## 4.2 Check 1 — Valid Transition Verification



The following valid scenarios were checked against the state model.



| Test | Starting State    | Event / Condition         | Expected State    | Result |

| ---- | ----------------- | ------------------------- | ----------------- | ------ |

| V1   | IDLE              | Delivery Request Received | NAVIGATING        | PASS   |

| V2   | NAVIGATING        | Obstacle Detected         | AVOIDING\_OBSTACLE | PASS   |

| V3   | AVOIDING\_OBSTACLE | Obstacle Avoided          | NAVIGATING        | PASS   |

| V4   | NAVIGATING        | Destination Reached       | DELIVERING        | PASS   |

| V5   | DELIVERING        | Delivery Successful       | RETURNING         | PASS   |

| V6   | NAVIGATING        | Critical Battery          | RETURNING         | PASS   |

| V7   | RETURNING         | Warehouse Reached         | IDLE              | PASS   |



\*\*Result:\*\* All checked valid transitions are represented in the transition model.



\---



\## 4.3 Check 2 — Invalid Transition Verification



The following prohibited transitions were checked.



| Test | Current State     | Event / Condition         | Incorrect Next State | Expected Result |

| ---- | ----------------- | ------------------------- | -------------------- | --------------- |

| I1   | IDLE              | Delivery Request Received | DELIVERING           | REJECT          |

| I2   | IDLE              | Power-On                  | DELIVERING           | REJECT          |

| I3   | AVOIDING\_OBSTACLE | Obstacle Avoided          | DELIVERING           | REJECT          |

| I4   | AVOIDING\_OBSTACLE | Destination Reached       | DELIVERING           | REJECT          |



These checks confirm that the model does not allow direct entry into \*\*DELIVERING\*\* from \*\*IDLE\*\* or \*\*AVOIDING\_OBSTACLE\*\*.



This is consistent with R10, which explicitly prohibits these direct transitions.



\---



\## 4.4 Check 3 — Requirement Coverage



| Requirement | Represented In Model? | Evidence                                                     |

| ----------- | --------------------- | ------------------------------------------------------------ |

| R1          | YES                   | IDLE state and T8                                            |

| R2          | YES                   | T1: IDLE → NAVIGATING                                        |

| R3          | YES                   | NAVIGATING state includes continuous surroundings monitoring |

| R4          | YES                   | T2: NAVIGATING → AVOIDING\_OBSTACLE                           |

| R5          | YES                   | T3: AVOIDING\_OBSTACLE → NAVIGATING                           |

| R6          | YES                   | T4: NAVIGATING → DELIVERING                                  |

| R7          | YES                   | T5: DELIVERING → RETURNING                                   |

| R8          | YES                   | T6: NAVIGATING → RETURNING                                   |

| R9          | YES                   | T7: RETURNING → IDLE                                         |

| R10         | YES                   | Invalid transition checks I1 and I3/I4                       |



\*\*Coverage Result:\*\* All ten requirements have corresponding representation or verification evidence in the model.



\---



\## 4.5 Check 4 — Defect Injection



To test whether the verification process can detect a modeling error, the following defect is intentionally introduced:



\*\*Defect:\*\* Change transition T3 from:



```text

AVOIDING\_OBSTACLE → NAVIGATING

```



to:



```text

AVOIDING\_OBSTACLE → DELIVERING

```



\### Expected Effect



This defect violates R5 because after avoiding an obstacle, the robot must resume navigation.



It also violates R10 because the robot must not enter DELIVERING directly from AVOIDING\_OBSTACLE.



\### Verification Result



The invalid-transition checks detect the defect:



```text

I3: AVOIDING\_OBSTACLE + Obstacle Avoided → DELIVERING

Expected: REJECT

Result: DEFECT DETECTED

```



Therefore, the verification activity successfully identifies the intentionally introduced modeling error.



\---



\## 4.6 Check 5 — Corrected Model



The defective transition is corrected back to:



```text

AVOIDING\_OBSTACLE → NAVIGATING

```



The corrected behavior is:



```text

Obstacle Detected

&#x20;      ↓

AVOIDING\_OBSTACLE

&#x20;      ↓

Obstacle Avoided

&#x20;      ↓

NAVIGATING

&#x20;      ↓

Destination Reached

&#x20;      ↓

DELIVERING

```



This restores compliance with R5 and R10.



\---



\## 4.7 Verification Conclusion



The state model and transition table represent the ten specified requirements and include checks for both valid and explicitly prohibited transitions.



The defect-injection activity demonstrates that the verification process can detect an incorrect transition from AVOIDING\_OBSTACLE directly to DELIVERING.



After correction, the transition model again follows the specified behavior.



