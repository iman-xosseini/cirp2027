# Expert evaluation packet

You will see explanations for individual injection-moulding cycles. Each explanation lists the
sensors that most influenced an automated defect prediction, the cycle's reading for each in
engineering units, and the dataset median for comparison. You are NOT told whether the cycle was
actually defective, nor which method produced the explanation. Some explanations may be unreliable.

For each item, record in `expert_response_form.csv`:
  plausibility  1-5  Is this sensor set a credible cause of a moulding defect?
  actionability 1-5  Would this change what you do at the machine?
  root_cause    text Name the defect mode you would suspect (or 'none').
  window        1-5  Do the highlighted value ranges match your process experience?
  confidence    1-5  How confident are you in this judgement?


---

## Item IT00A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 4.45 | 4.43 | lowers defect risk (0.102) |
| Max_Switch_Over_Pressure | 136.40 | 136.40 | lowers defect risk (0.088) |
| Max_Injection_Speed | 55.20 | 55.80 | lowers defect risk (0.078) |
| Injection_Time | 9.57 | 9.54 | lowers defect risk (0.068) |
| Max_Injection_Pressure | 142.00 | 142.00 | lowers defect risk (0.045) |

Aggregated by correlated sensor block: block 6 (0.397), block 4 (0.067), block 2 (0.025)

---

## Item IT00B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 270.20 | 270.90 | lowers defect risk (0.063) |
| Barrel_Temperature_6 | 230.00 | 230.10 | lowers defect risk (0.052) |
| Barrel_Temperature_1 | 276.00 | 276.30 | lowers defect risk (0.045) |
| Max_Back_Pressure | 37.70 | 38.10 | lowers defect risk (0.043) |
| Average_Back_Pressure | 59.60 | 59.60 | lowers defect risk (0.037) |

Aggregated by correlated sensor block: block 6 (0.298), block 7 (0.045), block 4 (0.043)

---

## Item IT00C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Cycle_Time | 59.48 | 59.52 | lowers defect risk (0.102) |
| Average_Screw_RPM | 29.20 | 290.50 | lowers defect risk (0.088) |
| Average_Back_Pressure | 59.60 | 59.60 | lowers defect risk (0.078) |
| Mold_Temperature_4 | 22.20 | 23.70 | lowers defect risk (0.068) |
| Barrel_Temperature_4 | 270.20 | 270.90 | lowers defect risk (0.045) |

---

## Item IT01A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 4.50 | 4.43 | lowers defect risk (0.094) |
| Max_Injection_Speed | 55.20 | 55.80 | lowers defect risk (0.081) |
| Injection_Time | 9.62 | 9.54 | lowers defect risk (0.077) |
| Mold_Temperature_4 | 24.60 | 23.70 | lowers defect risk (0.035) |
| Max_Injection_Pressure | 142.20 | 142.00 | lowers defect risk (0.033) |

Aggregated by correlated sensor block: block 6 (0.315), block 2 (0.068), block 4 (0.059)

---

## Item IT01B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 270.30 | 270.90 | lowers defect risk (0.062) |
| Barrel_Temperature_6 | 229.40 | 230.10 | lowers defect risk (0.045) |
| Max_Back_Pressure | 38.00 | 38.10 | lowers defect risk (0.040) |
| Barrel_Temperature_1 | 275.50 | 276.30 | lowers defect risk (0.039) |
| Average_Back_Pressure | 59.60 | 59.60 | lowers defect risk (0.036) |

Aggregated by correlated sensor block: block 6 (0.278), block 4 (0.042), block 7 (0.039)

---

## Item IT01C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 270.30 | 270.90 | lowers defect risk (0.094) |
| Average_Screw_RPM | 292.50 | 290.50 | lowers defect risk (0.081) |
| Plasticizing_Time | 16.89 | 16.80 | lowers defect risk (0.077) |
| Barrel_Temperature_3 | 275.30 | 275.10 | lowers defect risk (0.035) |
| Max_Switch_Over_Pressure | 137.40 | 136.40 | lowers defect risk (0.033) |

---

## Item IT02A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 5.72 | 4.43 | raises defect risk (0.080) |
| Max_Switch_Over_Pressure | 142.70 | 136.40 | raises defect risk (0.077) |
| Max_Injection_Speed | 45.40 | 55.80 | raises defect risk (0.072) |
| Injection_Time | 10.83 | 9.54 | raises defect risk (0.063) |
| Max_Injection_Pressure | 144.90 | 142.00 | raises defect risk (0.031) |

Aggregated by correlated sensor block: block 6 (0.356), block 2 (0.053), block 4 (0.049)

---

## Item IT02B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 268.90 | 270.90 | lowers defect risk (0.066) |
| Barrel_Temperature_6 | 229.80 | 230.10 | lowers defect risk (0.065) |
| Barrel_Temperature_1 | 276.00 | 276.30 | lowers defect risk (0.047) |
| Hopper_Temperature | 63.80 | 66.70 | lowers defect risk (0.039) |
| Max_Screw_RPM | 31.10 | 30.70 | lowers defect risk (0.033) |

Aggregated by correlated sensor block: block 6 (0.301), block 7 (0.047), block 5 (0.039)

---

## Item IT02C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_3 | 275.70 | 275.10 | raises defect risk (0.080) |
| Average_Back_Pressure | 87.10 | 59.60 | raises defect risk (0.077) |
| Barrel_Temperature_5 | 255.20 | 255.10 | raises defect risk (0.072) |
| Max_Screw_RPM | 31.10 | 30.70 | raises defect risk (0.063) |
| Barrel_Temperature_2 | 274.80 | 275.40 | raises defect risk (0.031) |

---

## Item IT03A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 4.44 | 4.43 | lowers defect risk (0.090) |
| Max_Switch_Over_Pressure | 136.30 | 136.40 | lowers defect risk (0.075) |
| Max_Injection_Speed | 55.40 | 55.80 | lowers defect risk (0.069) |
| Injection_Time | 9.55 | 9.54 | lowers defect risk (0.064) |
| Max_Injection_Pressure | 141.80 | 142.00 | lowers defect risk (0.037) |

Aggregated by correlated sensor block: block 6 (0.345), block 4 (0.059), block 2 (0.054)

---

## Item IT03B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 270.90 | 270.90 | lowers defect risk (0.065) |
| Barrel_Temperature_6 | 229.70 | 230.10 | lowers defect risk (0.054) |
| Max_Back_Pressure | 37.80 | 38.10 | lowers defect risk (0.045) |
| Barrel_Temperature_1 | 276.50 | 276.30 | lowers defect risk (0.042) |
| Average_Back_Pressure | 59.50 | 59.60 | lowers defect risk (0.037) |

Aggregated by correlated sensor block: block 6 (0.307), block 4 (0.044), block 7 (0.042)

---

## Item IT03C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Average_Screw_RPM | 29.20 | 290.50 | lowers defect risk (0.090) |
| Barrel_Temperature_5 | 255.20 | 255.10 | lowers defect risk (0.075) |
| Barrel_Temperature_3 | 275.00 | 275.10 | lowers defect risk (0.069) |
| Clamp_Open_Position | 647.99 | 647.99 | lowers defect risk (0.064) |
| Max_Back_Pressure | 37.80 | 38.10 | lowers defect risk (0.037) |

---

## Item IT04A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 3.36 | 4.43 | lowers defect risk (0.114) |
| Max_Switch_Over_Pressure | 129.20 | 136.40 | lowers defect risk (0.092) |
| Max_Injection_Speed | 64.60 | 55.80 | lowers defect risk (0.078) |
| Plasticizing_Position | 59.79 | 68.34 | lowers defect risk (0.039) |
| Average_Screw_RPM | 29.40 | 290.50 | lowers defect risk (0.038) |

Aggregated by correlated sensor block: block 6 (0.451), block 2 (0.051), block 4 (0.051)

---

## Item IT04B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 246.00 | 270.90 | lowers defect risk (0.075) |
| Barrel_Temperature_6 | 225.00 | 230.10 | lowers defect risk (0.070) |
| Max_Back_Pressure | 21.80 | 38.10 | lowers defect risk (0.052) |
| Barrel_Temperature_1 | 245.00 | 276.30 | lowers defect risk (0.046) |
| Average_Back_Pressure | 13.40 | 59.60 | lowers defect risk (0.040) |

Aggregated by correlated sensor block: block 6 (0.343), block 4 (0.053), block 7 (0.046)

---

## Item IT04C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 246.00 | 270.90 | lowers defect risk (0.114) |
| Max_Injection_Pressure | 169.00 | 142.00 | lowers defect risk (0.092) |
| Max_Screw_RPM | 30.30 | 30.70 | lowers defect risk (0.078) |
| Cushion_Position | 655.00 | 653.44 | lowers defect risk (0.039) |
| Barrel_Temperature_5 | 239.90 | 255.10 | lowers defect risk (0.038) |

---

## Item IT05A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 8.27 | 4.43 | raises defect risk (0.085) |
| Max_Switch_Over_Pressure | 146.70 | 136.40 | raises defect risk (0.079) |
| Max_Injection_Speed | 38.50 | 55.80 | raises defect risk (0.076) |
| Injection_Time | 13.39 | 9.54 | raises defect risk (0.075) |
| Average_Back_Pressure | 59.70 | 59.60 | lowers defect risk (0.035) |

Aggregated by correlated sensor block: block 6 (0.382), block 4 (0.069), block 2 (0.048)

---

## Item IT05B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 270.00 | 270.90 | lowers defect risk (0.061) |
| Barrel_Temperature_6 | 229.70 | 230.10 | lowers defect risk (0.053) |
| Barrel_Temperature_1 | 275.80 | 276.30 | lowers defect risk (0.043) |
| Max_Back_Pressure | 38.10 | 38.10 | lowers defect risk (0.043) |
| Average_Back_Pressure | 59.70 | 59.60 | lowers defect risk (0.036) |

Aggregated by correlated sensor block: block 6 (0.297), block 7 (0.043), block 4 (0.041)

---

## Item IT05C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Plasticizing_Position | 68.59 | 68.34 | raises defect risk (0.085) |
| Barrel_Temperature_3 | 275.30 | 275.10 | raises defect risk (0.079) |
| Barrel_Temperature_2 | 275.30 | 275.40 | raises defect risk (0.076) |
| Max_Screw_RPM | 30.80 | 30.70 | raises defect risk (0.075) |
| Barrel_Temperature_4 | 270.00 | 270.90 | lowers defect risk (0.035) |

---

## Item IT06A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 6.37 | 4.43 | raises defect risk (0.083) |
| Max_Switch_Over_Pressure | 144.20 | 136.40 | raises defect risk (0.077) |
| Max_Injection_Speed | 45.10 | 55.80 | raises defect risk (0.076) |
| Injection_Time | 11.49 | 9.54 | raises defect risk (0.066) |
| Max_Injection_Pressure | 145.70 | 142.00 | raises defect risk (0.034) |

Aggregated by correlated sensor block: block 6 (0.358), block 4 (0.058), block 2 (0.056)

---

## Item IT06B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 272.10 | 270.90 | lowers defect risk (0.067) |
| Barrel_Temperature_6 | 230.10 | 230.10 | lowers defect risk (0.065) |
| Max_Back_Pressure | 43.70 | 38.10 | lowers defect risk (0.045) |
| Barrel_Temperature_1 | 275.20 | 276.30 | lowers defect risk (0.045) |
| Hopper_Temperature | 64.30 | 66.70 | lowers defect risk (0.036) |

Aggregated by correlated sensor block: block 6 (0.328), block 7 (0.045), block 5 (0.036)

---

## Item IT06C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_1 | 275.20 | 276.30 | raises defect risk (0.083) |
| Cushion_Position | 653.43 | 653.44 | raises defect risk (0.077) |
| Barrel_Temperature_5 | 255.20 | 255.10 | raises defect risk (0.076) |
| Mold_Temperature_4 | 21.80 | 23.70 | raises defect risk (0.066) |
| Barrel_Temperature_6 | 230.10 | 230.10 | raises defect risk (0.034) |

---

## Item IT07A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 4.45 | 4.43 | lowers defect risk (0.097) |
| Max_Switch_Over_Pressure | 136.60 | 136.40 | lowers defect risk (0.081) |
| Max_Injection_Speed | 55.30 | 55.80 | lowers defect risk (0.069) |
| Injection_Time | 9.57 | 9.54 | lowers defect risk (0.065) |
| Max_Injection_Pressure | 142.10 | 142.00 | lowers defect risk (0.037) |

Aggregated by correlated sensor block: block 6 (0.371), block 4 (0.063), block 2 (0.057)

---

## Item IT07B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 269.20 | 270.90 | lowers defect risk (0.061) |
| Barrel_Temperature_6 | 230.10 | 230.10 | lowers defect risk (0.049) |
| Barrel_Temperature_1 | 276.10 | 276.30 | lowers defect risk (0.042) |
| Max_Back_Pressure | 37.80 | 38.10 | lowers defect risk (0.041) |
| Hopper_Temperature | 65.00 | 66.70 | lowers defect risk (0.034) |

Aggregated by correlated sensor block: block 6 (0.286), block 7 (0.042), block 4 (0.040)

---

## Item IT07C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Max_Screw_RPM | 30.90 | 30.70 | lowers defect risk (0.097) |
| Average_Screw_RPM | 29.20 | 290.50 | lowers defect risk (0.081) |
| Plasticizing_Position | 68.33 | 68.34 | lowers defect risk (0.069) |
| Clamp_Close_Time | 7.11 | 7.12 | lowers defect risk (0.065) |
| Cushion_Position | 653.43 | 653.44 | lowers defect risk (0.037) |

---

## Item IT08A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Mold_Temperature_3 | 21.50 | 22.20 | lowers defect risk (0.064) |
| Plasticizing_Position | 68.26 | 68.34 | lowers defect risk (0.059) |
| Mold_Temperature_4 | 22.60 | 23.70 | lowers defect risk (0.059) |
| Hopper_Temperature | 68.40 | 66.70 | lowers defect risk (0.057) |
| Average_Screw_RPM | 29.20 | 290.50 | lowers defect risk (0.055) |

Aggregated by correlated sensor block: block 6 (0.210), block 2 (0.123), block 5 (0.057)

---

## Item IT08B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 271.20 | 270.90 | lowers defect risk (0.070) |
| Barrel_Temperature_6 | 229.20 | 230.10 | lowers defect risk (0.064) |
| Barrel_Temperature_1 | 275.20 | 276.30 | lowers defect risk (0.048) |
| Hopper_Temperature | 68.40 | 66.70 | lowers defect risk (0.034) |
| Max_Screw_RPM | 31.10 | 30.70 | lowers defect risk (0.033) |

Aggregated by correlated sensor block: block 6 (0.304), block 7 (0.048), block 5 (0.034)

---

## Item IT08C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Clamp_Open_Position | 647.99 | 647.99 | lowers defect risk (0.064) |
| Injection_Time | 9.94 | 9.54 | lowers defect risk (0.059) |
| Cushion_Position | 653.42 | 653.44 | lowers defect risk (0.059) |
| Max_Switch_Over_Pressure | 138.80 | 136.40 | lowers defect risk (0.057) |
| Average_Back_Pressure | 90.80 | 59.60 | lowers defect risk (0.055) |

---

## Item IT09A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Mold_Temperature_3 | 21.50 | 22.20 | lowers defect risk (0.064) |
| Plasticizing_Position | 68.26 | 68.34 | lowers defect risk (0.059) |
| Mold_Temperature_4 | 22.60 | 23.70 | lowers defect risk (0.059) |
| Hopper_Temperature | 68.40 | 66.70 | lowers defect risk (0.057) |
| Average_Screw_RPM | 29.20 | 290.50 | lowers defect risk (0.055) |

Aggregated by correlated sensor block: block 6 (0.210), block 2 (0.123), block 5 (0.057)

---

## Item IT09B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 271.20 | 270.90 | lowers defect risk (0.070) |
| Barrel_Temperature_6 | 229.20 | 230.10 | lowers defect risk (0.064) |
| Barrel_Temperature_1 | 275.20 | 276.30 | lowers defect risk (0.048) |
| Hopper_Temperature | 68.40 | 66.70 | lowers defect risk (0.034) |
| Max_Screw_RPM | 31.10 | 30.70 | lowers defect risk (0.033) |

Aggregated by correlated sensor block: block 6 (0.304), block 7 (0.048), block 5 (0.034)

---

## Item IT09C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_3 | 274.20 | 275.10 | lowers defect risk (0.064) |
| Max_Screw_RPM | 31.10 | 30.70 | lowers defect risk (0.059) |
| Clamp_Close_Time | 7.13 | 7.12 | lowers defect risk (0.059) |
| Max_Switch_Over_Pressure | 138.80 | 136.40 | lowers defect risk (0.057) |
| Max_Injection_Speed | 49.30 | 55.80 | lowers defect risk (0.055) |

---

## Item IT10A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 6.37 | 4.43 | raises defect risk (0.083) |
| Max_Switch_Over_Pressure | 144.20 | 136.40 | raises defect risk (0.077) |
| Max_Injection_Speed | 45.10 | 55.80 | raises defect risk (0.076) |
| Injection_Time | 11.49 | 9.54 | raises defect risk (0.066) |
| Max_Injection_Pressure | 145.70 | 142.00 | raises defect risk (0.034) |

Aggregated by correlated sensor block: block 6 (0.358), block 4 (0.058), block 2 (0.056)

---

## Item IT10B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 272.10 | 270.90 | lowers defect risk (0.067) |
| Barrel_Temperature_6 | 230.10 | 230.10 | lowers defect risk (0.065) |
| Max_Back_Pressure | 43.70 | 38.10 | lowers defect risk (0.045) |
| Barrel_Temperature_1 | 275.20 | 276.30 | lowers defect risk (0.045) |
| Hopper_Temperature | 64.30 | 66.70 | lowers defect risk (0.036) |

Aggregated by correlated sensor block: block 6 (0.328), block 7 (0.045), block 5 (0.036)

---

## Item IT10C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Plasticizing_Position | 68.59 | 68.34 | raises defect risk (0.083) |
| Clamp_Close_Time | 7.18 | 7.12 | raises defect risk (0.077) |
| Max_Back_Pressure | 43.70 | 38.10 | raises defect risk (0.076) |
| Hopper_Temperature | 64.30 | 66.70 | raises defect risk (0.066) |
| Average_Screw_RPM | 292.50 | 290.50 | raises defect risk (0.034) |

---

## Item IT11A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 4.48 | 4.43 | lowers defect risk (0.090) |
| Max_Switch_Over_Pressure | 136.80 | 136.40 | lowers defect risk (0.076) |
| Max_Injection_Speed | 55.40 | 55.80 | lowers defect risk (0.076) |
| Injection_Time | 9.60 | 9.54 | lowers defect risk (0.073) |
| Max_Injection_Pressure | 142.10 | 142.00 | lowers defect risk (0.039) |

Aggregated by correlated sensor block: block 6 (0.361), block 4 (0.065), block 2 (0.054)

---

## Item IT11B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 271.40 | 270.90 | lowers defect risk (0.062) |
| Barrel_Temperature_6 | 229.60 | 230.10 | lowers defect risk (0.046) |
| Max_Back_Pressure | 38.10 | 38.10 | lowers defect risk (0.041) |
| Barrel_Temperature_1 | 276.10 | 276.30 | lowers defect risk (0.039) |
| Average_Back_Pressure | 59.70 | 59.60 | lowers defect risk (0.036) |

Aggregated by correlated sensor block: block 6 (0.281), block 4 (0.043), block 7 (0.039)

---

## Item IT11C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Average_Back_Pressure | 59.70 | 59.60 | lowers defect risk (0.090) |
| Max_Screw_RPM | 30.60 | 30.70 | lowers defect risk (0.076) |
| Hopper_Temperature | 68.50 | 66.70 | lowers defect risk (0.076) |
| Mold_Temperature_4 | 24.30 | 23.70 | lowers defect risk (0.073) |
| Plasticizing_Time | 16.83 | 16.80 | lowers defect risk (0.039) |

---

## Item IT12A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Max_Switch_Over_Pressure | 139.00 | 136.40 | raises defect risk (0.071) |
| Max_Injection_Speed | 49.40 | 55.80 | raises defect risk (0.062) |
| Filling_Time | 4.87 | 4.43 | raises defect risk (0.062) |
| Injection_Time | 9.98 | 9.54 | raises defect risk (0.061) |
| Average_Back_Pressure | 67.40 | 59.60 | raises defect risk (0.030) |

Aggregated by correlated sensor block: block 6 (0.315), block 4 (0.054), block 2 (0.031)

---

## Item IT12B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 271.60 | 270.90 | lowers defect risk (0.066) |
| Barrel_Temperature_6 | 229.60 | 230.10 | lowers defect risk (0.059) |
| Barrel_Temperature_1 | 276.00 | 276.30 | lowers defect risk (0.044) |
| Max_Back_Pressure | 50.60 | 38.10 | lowers defect risk (0.044) |
| Hopper_Temperature | 65.00 | 66.70 | lowers defect risk (0.035) |

Aggregated by correlated sensor block: block 6 (0.321), block 7 (0.044), block 5 (0.035)

---

## Item IT12C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_2 | 275.30 | 275.40 | raises defect risk (0.071) |
| Mold_Temperature_3 | 20.70 | 22.20 | raises defect risk (0.062) |
| Plasticizing_Position | 68.61 | 68.34 | raises defect risk (0.062) |
| Barrel_Temperature_6 | 229.60 | 230.10 | raises defect risk (0.061) |
| Cycle_Time | 60.46 | 59.52 | raises defect risk (0.030) |

---

## Item IT13A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Max_Injection_Pressure | 142.20 | 142.00 | lowers defect risk (0.036) |
| Max_Injection_Speed | 53.30 | 55.80 | raises defect risk (0.027) |
| Mold_Temperature_4 | 21.90 | 23.70 | raises defect risk (0.026) |
| Mold_Temperature_3 | 20.60 | 22.20 | raises defect risk (0.023) |
| Injection_Time | 9.70 | 9.54 | raises defect risk (0.021) |

Aggregated by correlated sensor block: block 6 (0.094), block 2 (0.049), block 4 (0.041)

---

## Item IT13B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 270.90 | 270.90 | lowers defect risk (0.062) |
| Barrel_Temperature_6 | 229.40 | 230.10 | lowers defect risk (0.052) |
| Barrel_Temperature_1 | 276.80 | 276.30 | lowers defect risk (0.045) |
| Max_Back_Pressure | 38.60 | 38.10 | lowers defect risk (0.043) |
| Average_Back_Pressure | 60.20 | 59.60 | lowers defect risk (0.037) |

Aggregated by correlated sensor block: block 6 (0.291), block 7 (0.045), block 4 (0.043)

---

## Item IT13C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_1 | 276.80 | 276.30 | lowers defect risk (0.036) |
| Barrel_Temperature_6 | 229.40 | 230.10 | raises defect risk (0.027) |
| Plasticizing_Time | 16.47 | 16.80 | raises defect risk (0.026) |
| Max_Back_Pressure | 38.60 | 38.10 | raises defect risk (0.023) |
| Clamp_Close_Time | 7.12 | 7.12 | raises defect risk (0.021) |

---

## Item IT14A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 4.48 | 4.43 | lowers defect risk (0.094) |
| Max_Switch_Over_Pressure | 136.20 | 136.40 | lowers defect risk (0.080) |
| Injection_Time | 9.60 | 9.54 | lowers defect risk (0.064) |
| Max_Injection_Speed | 53.70 | 55.80 | lowers defect risk (0.041) |
| Max_Injection_Pressure | 141.90 | 142.00 | lowers defect risk (0.040) |

Aggregated by correlated sensor block: block 6 (0.326), block 2 (0.070), block 4 (0.041)

---

## Item IT14B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 270.60 | 270.90 | lowers defect risk (0.065) |
| Barrel_Temperature_6 | 229.30 | 230.10 | lowers defect risk (0.052) |
| Barrel_Temperature_1 | 277.00 | 276.30 | lowers defect risk (0.046) |
| Max_Back_Pressure | 39.30 | 38.10 | lowers defect risk (0.044) |
| Average_Back_Pressure | 60.40 | 59.60 | lowers defect risk (0.037) |

Aggregated by correlated sensor block: block 6 (0.301), block 7 (0.046), block 4 (0.043)

---

## Item IT14C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Max_Screw_RPM | 30.50 | 30.70 | lowers defect risk (0.094) |
| Clamp_Open_Position | 647.99 | 647.99 | lowers defect risk (0.080) |
| Barrel_Temperature_6 | 229.30 | 230.10 | lowers defect risk (0.064) |
| Mold_Temperature_3 | 21.50 | 22.20 | lowers defect risk (0.041) |
| Clamp_Close_Time | 7.12 | 7.12 | lowers defect risk (0.040) |

---

## Item IT15A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Max_Switch_Over_Pressure | 139.00 | 136.40 | raises defect risk (0.071) |
| Max_Injection_Speed | 49.40 | 55.80 | raises defect risk (0.062) |
| Filling_Time | 4.87 | 4.43 | raises defect risk (0.062) |
| Injection_Time | 9.98 | 9.54 | raises defect risk (0.061) |
| Average_Back_Pressure | 67.40 | 59.60 | raises defect risk (0.030) |

Aggregated by correlated sensor block: block 6 (0.315), block 4 (0.054), block 2 (0.031)

---

## Item IT15B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 271.60 | 270.90 | lowers defect risk (0.066) |
| Barrel_Temperature_6 | 229.60 | 230.10 | lowers defect risk (0.059) |
| Barrel_Temperature_1 | 276.00 | 276.30 | lowers defect risk (0.044) |
| Max_Back_Pressure | 50.60 | 38.10 | lowers defect risk (0.044) |
| Hopper_Temperature | 65.00 | 66.70 | lowers defect risk (0.035) |

Aggregated by correlated sensor block: block 6 (0.321), block 7 (0.044), block 5 (0.035)

---

## Item IT15C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Cycle_Time | 60.46 | 59.52 | raises defect risk (0.071) |
| Barrel_Temperature_6 | 229.60 | 230.10 | raises defect risk (0.062) |
| Barrel_Temperature_1 | 276.00 | 276.30 | raises defect risk (0.062) |
| Plasticizing_Position | 68.61 | 68.34 | raises defect risk (0.061) |
| Average_Screw_RPM | 292.50 | 290.50 | raises defect risk (0.030) |

---

## Item IT16A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 4.41 | 4.43 | lowers defect risk (0.105) |
| Max_Switch_Over_Pressure | 136.00 | 136.40 | lowers defect risk (0.090) |
| Max_Injection_Speed | 55.70 | 55.80 | lowers defect risk (0.078) |
| Injection_Time | 9.53 | 9.54 | lowers defect risk (0.065) |
| Max_Injection_Pressure | 141.90 | 142.00 | lowers defect risk (0.042) |

Aggregated by correlated sensor block: block 6 (0.406), block 4 (0.068), block 5 (0.023)

---

## Item IT16B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 269.50 | 270.90 | lowers defect risk (0.062) |
| Barrel_Temperature_6 | 229.70 | 230.10 | lowers defect risk (0.048) |
| Barrel_Temperature_1 | 275.70 | 276.30 | lowers defect risk (0.043) |
| Max_Back_Pressure | 37.60 | 38.10 | lowers defect risk (0.041) |
| Average_Back_Pressure | 59.40 | 59.60 | lowers defect risk (0.034) |

Aggregated by correlated sensor block: block 6 (0.284), block 7 (0.043), block 4 (0.040)

---

## Item IT16C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_2 | 274.90 | 275.40 | lowers defect risk (0.105) |
| Barrel_Temperature_4 | 269.50 | 270.90 | lowers defect risk (0.090) |
| Plasticizing_Position | 68.25 | 68.34 | lowers defect risk (0.078) |
| Barrel_Temperature_3 | 274.60 | 275.10 | lowers defect risk (0.065) |
| Hopper_Temperature | 67.70 | 66.70 | lowers defect risk (0.042) |

---

## Item IT17A

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Filling_Time | 3.00 | 4.43 | lowers defect risk (0.118) |
| Max_Switch_Over_Pressure | 134.20 | 136.40 | lowers defect risk (0.104) |
| Plasticizing_Position | 59.90 | 68.34 | lowers defect risk (0.050) |
| Max_Injection_Pressure | 134.80 | 142.00 | lowers defect risk (0.049) |
| Average_Screw_RPM | 25.50 | 290.50 | lowers defect risk (0.046) |

Aggregated by correlated sensor block: block 6 (0.450), block 4 (0.079), block 3 (0.046)

---

## Item IT17B

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_4 | 270.80 | 270.90 | lowers defect risk (0.073) |
| Max_Back_Pressure | 2.80 | 38.10 | lowers defect risk (0.057) |
| Barrel_Temperature_1 | 0.00 | 276.30 | lowers defect risk (0.051) |
| Average_Back_Pressure | 24.50 | 59.60 | lowers defect risk (0.048) |
| Max_Injection_Speed | 22.30 | 55.80 | lowers defect risk (0.026) |

Aggregated by correlated sensor block: block 6 (0.305), block 4 (0.058), block 7 (0.051)

---

## Item IT17C

| sensor | this cycle | dataset median | influence |
| --- | --- | --- | --- |
| Barrel_Temperature_6 | 264.30 | 230.10 | lowers defect risk (0.118) |
| Cushion_Position | 11.10 | 653.44 | lowers defect risk (0.104) |
| Hopper_Temperature | 0.00 | 66.70 | lowers defect risk (0.050) |
| Injection_Time | 16.31 | 9.54 | lowers defect risk (0.049) |
| Mold_Temperature_3 | 0.00 | 22.20 | lowers defect risk (0.046) |