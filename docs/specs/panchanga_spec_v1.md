# panchanga_spec_v1

## Overview
Defines the Panchanga payload produced by Refraction Engine V1. Focuses on the Moon’s celestial data for the target date/time/location.

## Schema

```yaml
panchanga:
  reference:
    datetime_utc: string   # ISO-8601 timestamp in UTC
    timezone: string       # IANA timezone string (e.g., "Asia/Tehran")
    location:
      latitude: float
      longitude: float

  tithi:
    index: integer          # 1..30
    name: string            # e.g., "Prathamai","Amavasai"
    paksha: string          # "SHUKLA" | "KRISHNA"
    remaining_percentage: float  # 0.0..1.0

  nakshatra:
    index: integer          # 1..27  (Moon's nakshatra)
    name: string            # uppercase Nakshatra name
    pada: integer           # 1..4
    lord: string            # planetary lord (e.g., "JUPITER")
    span_deg: float         # 0.0.. <13.3333 (Moon's progress through Nakshatra)

  yoga:
    index: integer          # 1..27
    name: string            # yoga name from PyJHora resources

  karana:
    index: integer          # 1..11
    name: string

  hora_lord: string         # planet name/ID ruling the current hora

  sunrise: string           # ISO-8601 local time
  sunset: string            # ISO-8601 local time

  auspicious_windows:
    - start: string         # ISO-8601
      end: string
      tag?: string          # e.g., "ABHIJIT"

  inauspicious_windows:
    - start: string
      end: string
      tag?: string          # e.g., "RAHU_KALAM"
```

## Notes

- `panchanga.nakshatra` always refers to the **Moon’s Nakshatra at the reference moment**; use this value for dasha seed calculations and diary views.  
- Other Nakshatra data (Ascendant or planets) is provided by `core_chart_spec_v1`.  
- Sunrise/sunset are used to compute paksha/tag intervals and tie into the UI widgets listed in `docs/pyjhora_knowledge/ui_catalog/UI_Modules.md`.
