# Chariot Studio — Product and Engineering Spine

## 1. Overview

This document defines the product and technical spine for Chariot Studio, the digital showroom where Chariot Auto’s vehicle designs are presented, explored, and converted into customer interest.

The system is a web-first showroom built around brand storytelling, vehicle discovery, and fast iteration for future model launches.

---

## 2. Core Layers

1. **Showroom Interface Layer**
   - Landing page sections
   - Vehicle cards
   - Gallery blocks
   - Calls to action for studio visits and model previews

2. **Design Content Layer**
   - Model names and trims
   - Exterior and interior design notes
   - Colorway, wheel, lighting, and material details
   - Concept versus production status

3. **Asset Layer**
   - Hero images or rendered vehicle artwork
   - Detail shots for wheels, cabins, lighting, and paint
   - Brand marks and launch campaign graphics

4. **Data Layer**
   - Structured vehicle metadata
   - Inquiry forms and lead capture
   - Future CMS or API-ready model records

---

## 3. Showroom Experience

### Responsibilities

- Introduce Chariot Auto’s design language
- Give each model a clear personality and use case
- Make the portfolio easy to browse
- Encourage visitors to book previews, test drives, or studio appointments

### Recommended Sections

- Hero: flagship concept or latest launch
- Collections: electric, performance, family, urban, and concept vehicles
- Design Details: aerodynamics, lighting, cockpit, materials, wheels
- Studio Journal: short notes from the design team
- Inquiry CTA: appointment, preview, or partnership contact

---

## 4. Vehicle Metadata Structure

A future implementation can store vehicles in JSON so the gallery is easy to expand.

```json
{
  "name": "Chariot Arc GT",
  "category": "Electric Grand Tourer",
  "status": "Concept",
  "rangeEstimate": "620 km",
  "designHighlights": [
    "low-slung aerodynamic profile",
    "panoramic glass canopy",
    "signature horizon light bar"
  ],
  "cta": "Request a studio preview"
}
```

---

## 5. Interaction Flow

```text
Visitor lands on Chariot Studio
   ↓
Hero introduces the design language
   ↓
Visitor browses vehicle collections
   ↓
Design details explain materials and engineering intent
   ↓
Visitor selects a model or inquiry CTA
   ↓
Lead is captured for preview, test drive, or launch updates
```

---

## 6. Design Principles

1. **Premium but approachable**
   - Large visuals, concise copy, and generous spacing

2. **Vehicle-first storytelling**
   - Every section should support the cars, not distract from them

3. **Modular content**
   - New models, trims, and launch stories should be easy to add

4. **Performance-conscious**
   - Optimize images, keep dependencies light, and prioritize fast first paint

5. **Conversion-ready**
   - Every page should create a clear next step for interested visitors

---

## 7. Future Extensions

- CMS-backed vehicle records
- 3D model viewer
- Configurator for color, wheels, and interiors
- Appointment booking integration
- Press kit and investor showroom pages
- Analytics dashboard for vehicle interest

---

## 8. Summary

Chariot Studio is the brand and product showcase for Chariot Auto. Its technical spine should stay simple, modular, and visual: a showroom-style website that can grow from static launch pages into a full vehicle discovery platform.
