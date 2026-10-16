
# TCY Motors Brunei - Premium Digital Showroom Kit

This kit contains 3 deliverables you requested:

## 1. UI Components & Comparison Card Layouts
File: tcy_ui_components.jsx
- VehicleCard: JDM-inspired premium dark card with image 16/10, brand red tag, heart compare, spec rows (Engine, Powertrain, Transmission, Fuel, Seating), price BND + monthly pills 7Y/10Y/5Y, CTAs VIEW DETAILS + ESTIMATE PAYMENT
- ComparisonTable: Grid layout 140px + repeat(vehicles) - header with images rounded 12px, rows Price/Variant/Powertrain/Engine/Fuel/Transmission/Drivetrain/Seating/Range/Monthly - hover bg #1E1E24/50
- PaymentEstimatorCard: Gradient #1E1E24 to #141418, red top line, amount financed, monthly BND {estimated}/mo large red 28px, total payable, DBR bar green<60 amber<80 red>80, disclaimer left border red

Design tokens:
bg: #070709, #0E0E10, #141418, #1E1E24
border: #2A2A32
text: #FFFFFF, muted #9AA0A8
accent: #E30613 / #FF1230 with glow shadow 0 0 15px rgba(227,6,19,0.4)
radius: 20px cards, 12px inner, 9999 pills
font: Space Grotesk headings bold, Inter body

## 2. BAIC/FOTON Spec Dataset JSON
File: tcy_vehicles_dataset.json
15 vehicles total (13 from your price list + 2 ARCFOX T1 EV upcoming)
Structure per vehicle:
{
  id, brand, model, variant, variant_code, price_bnd, monthly_ref: {5y,7y,10y}, availability, powertrain,
  specs: {engine, displacement_cc, power_hp, torque_nm, transmission, drivetrain, fuel_type, fuel_tank_l, seating, dimensions_mm, wheelbase_mm, ground_clearance_mm, trunk_l, 0_100_s, battery_*, range_*, tyres},
  key_features: [],
  colors: [],
  image_key, image_path (official paths from image_search)
}

Sources merged:
- Your price list (X55 Fashion 25250 etc)
- BAIC_REHEV_X55e.xlsx (77kW engine + 160kW motor 310Nm, 1.83kWh ternary, 53L, 350L trunk, 4620x1886x1680, WLTC 904KM, 5.86L/100km, ADAS L2 full)
- Acfocx_T1_Specification.xlsx (ARCFOX T1 EV 4337x1860x1572 wheelbase 2770, 70kW 94HP 176Nm, LFP 42.3kWh, WLTC 310 NEDC 375, fast 25min slow 8h, V2L/V2V, 15.6" + 8.8")
- BAIC_BJ30_catalogue_revised_8_Dec_2.pdf (4.7m rugged, 1.5T 185HP 305Nm ICE, hybrid 404HP 685Nm AWD 1000km range, 14.6" + 10.25" dual screens, fighter-jet shifter, 190cm flat 1496L)
- BJ40L catalogue (2.0T gasoline/diesel 2.0TD, 8AT dual clutch, 4WD diff lock, 4645x1925x1871, removable roof, rescue hooks x4, double wishbone front / five-link rear, 265/65R17, Ocean Blue/Olive Green/Black/White/Red/Golden)
- FOTON official sites (Tunland V7 pickup, View CS2 van 9/15/17)

Ready for frontend: import json, map to VehicleCard, filter by brand, sort by price_bnd or monthly_ref.

## 3. WhatsApp Templates
File: tcy_whatsapp_templates.json
11 templates covering all routing:

- enquiry_general
- enquiry_price_only (PRICE)
- enquiry_monthly_payment (MONTHLY PAYMENT with salary/bank/DBR)
- enquiry_financing (FINANCING)
- enquiry_trade_in (TRADE-IN with make/model/year/mileage/value/outstanding)
- enquiry_test_drive (TEST DRIVE with date/time/passengers)
- enquiry_comparison (comparing 2-3 vehicles)
- enquiry_availability (AVAILABILITY/PROMOTION - BJ40L limited)
- enquiry_appointment (showroom appointment)
- ask_muez_direct (ASK MUEZ direct to Abdul Muiz)
- lead_structured_summary (for sales dashboard/CRM)

Routing logic:
- TEST DRIVE date exists -> enquiry_test_drive
- Trade-in YES -> append trade-in template
- Salary+deposit+monthly -> enquiry_monthly_payment primary
- Compare >=2 -> enquiry_comparison
- ASK MUEZ button -> ask_muez_direct -> wa.me/6738138884?text= encoded
- Appointment requested -> enquiry_appointment
- Default browsing -> price_only or general
- On SUBMIT always generate lead_structured_summary

WA link: https://wa.me/6738138884?text={encodeURIComponent(filledTemplate)}

All templates include variables like {model} {variant} {price} {customer_name} {mobile} etc for dynamic filling.

---

Integration into your premium showroom artifact:
- Use dataset as source of truth for inventory
- VehicleCard component already matches your dark theme
- ComparisonTable uses official images
- WhatsApp templates power CHAT WITH SALES and ASK MUEZ buttons with pre-filled context

Files ready for download below.
