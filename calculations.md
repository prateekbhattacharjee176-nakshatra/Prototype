# Chariot Studio — Vehicle Specification and Showroom Calculation Spine

This document outlines practical calculations and structured data needed for an automotive showroom website. It replaces legacy domain logic with vehicle-focused measurements, estimates, and presentation rules.

---

## 1. Core Vehicle Data

Each vehicle profile should track the measurements and attributes customers expect in a showroom.

### 1.1 Dimensions

- Length
- Width
- Height
- Wheelbase
- Ground clearance
- Cargo capacity
- Seating capacity

### 1.2 Electric Vehicle Metrics

- Battery capacity in kWh
- Estimated range
- Charging time
- Peak charging rate
- Energy consumption

### 1.3 Performance Metrics

- Power output
- Torque
- 0–100 km/h time
- Top speed
- Drive layout

---

## 2. Range Estimate Formula

For electric vehicles, a simple planning estimate can be calculated as:

\[
\text{Estimated Range} = \frac{\text{Battery Capacity (kWh)}}{\text{Energy Use (kWh/100 km)}} \times 100
\]

Example:

\[
\frac{82}{14.5} \times 100 = 565.5 \text{ km}
\]

This estimate should be marked as indicative until validated by official testing cycles.

---

## 3. Charging Time Estimate

A simplified DC charging estimate:

\[
\text{Charging Time (hours)} = \frac{\text{Battery Capacity Added (kWh)}}{\text{Average Charging Power (kW)}}
\]

Example:

- Battery added: 57 kWh
- Average charging power: 115 kW

\[
\frac{57}{115} = 0.49 \text{ hours} \approx 29 \text{ minutes}
\]

---

## 4. Showroom Card Data Structure

```json
{
  "model": "Chariot Arc GT",
  "type": "Electric Grand Tourer",
  "batteryKWh": 82,
  "estimatedRangeKm": 565,
  "zeroToHundredKmh": 3.9,
  "seats": 4,
  "highlights": [
    "long-range electric platform",
    "panoramic glass roof",
    "driver-focused cockpit"
  ]
}
```

---

## 5. Comparison Rules

When comparing models, keep categories consistent:

- Range compared with range
- Charging time compared with charging time
- Seating and cargo compared by real customer use case
- Performance compared within the same vehicle class
- Concept vehicles clearly labeled as concept or pre-production

---

## 6. Gallery Prioritization

Vehicle detail pages should order visuals by customer impact:

1. Exterior hero angle
2. Front lighting signature
3. Side profile and wheel design
4. Cabin and cockpit
5. Rear design
6. Storage and practicality
7. Color and material options

---

## 7. Future Extensions

- Total cost of ownership calculator
- Charging route planner
- Trim comparison engine
- Color and wheel configurator
- Reservation deposit calculator
- Fleet savings estimator

---

## 8. Summary

Chariot Studio’s calculation spine should support clear automotive decision-making. The website needs structured vehicle data, transparent estimates, and consistent comparison logic so visitors can move from admiration to action.
