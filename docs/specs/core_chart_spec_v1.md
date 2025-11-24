# core_chart_spec_v1

## Overview
The `core_chart` extractor is the anchor for Refraction Engine V1. The extractor MUST accept a validated birth payload (date/time, timezone, latitude, longitude) and emit a structured response describing the ascendant and planetary positions. All downstream consumers (dashas, strengths, UI) depend on this schema.

## Output schema

### ascendant

```yaml
ascendant:
  longitude_deg: float         # Sidereal zodiac longitude in degrees (0 °.. <360 °)
  degree_in_sign: float        # Progress within the current sign (0 °.. <30 °)
  sign_index: integer          # 1..12 (1=Aries,...,12=Pisces)
  sign_name: string            # Uppercase sign name ("ARIES","TAURUS",...)
  house_index: integer         # Always 1 for the Ascendant
  nakshatra_index: integer     # 1..27 (Ashwini=1,...,Revati=27)
  nakshatra_name: string       # Nakshatra name matching `docs/pyjhora_knowledge/primitives/CorePrimitives.json`
  nakshatra_pada: integer      # 1..4 (quarter within the Nakshatra)
```

### planets

```yaml
planets:
  - id: string                 # "SUN","MOON","MERCURY","VENUS","MARS","JUPITER","SATURN","RAHU","KETU","URANUS","NEPTUNE","PLUTO"
    name: string
    longitude_deg: float       # 0.. <360
    degree_in_sign: float      # 0.. <30
    sign_index: integer        # 1..12
    sign_name: string
    house_index: integer       # 1..12
    retrograde: boolean
    speed_deg_per_day?: float  # optional raw speed data
    nakshatra_index: integer   # 1..27
    nakshatra_name: string
    nakshatra_pada: integer     # 1..4
```

### metadata (optional)

```yaml
metadata:
  ayanamsa_deg: float
  jd_utc: float
  location_name: string
```

### conventions note

- `nakshatra_index`/`nakshatra_pada` are provided for every celestial body even though only the Moon’s Nakshatra is central to Panchanga.  
- These Nakshatra fields follow the standard ordering and quartering used in `CorePrimitives.json`.  
- Longitude/degree fields are sidereal and compatible with PyJHora’s output (via `charts.divisional_chart`/`drik`).  
