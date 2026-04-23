# Kundali Software Calculative Spine

Building a Kundali software spine requires a layered approach: integrating high-precision astronomical datasets, trusted open-source tooling, and Vedic equations that bridge astronomy to astrology.

---

## 1) Core Infrastructure (Libraries & Datasets)

You do **not** need to compute raw planetary positions from scratch. Most production-grade systems rely on established ephemeris engines and timezone/location datasets.

### 1.1 Calculative Library
- **Swiss Ephemeris** (definitive standard for high-precision planetary calculations).
- Coverage: roughly **8,000 BC to 12,000 AD**.
- Commonly paired with JPL ephemerides for accuracy.

### 1.2 Ephemeris Data
- Use **JPL DE405/DE431** files (commonly bundled/used with Swiss Ephemeris) for planetary coordinates.

### 1.3 Time & Place Data
- **GeoNames** or **Google Maps Platform** for latitude/longitude resolution.
- **IANA Time Zone Database (tzdb)** for historical DST and timezone changes.

> [!NOTE]
> Accurate timezone conversion is non-negotiable for Kundali generation. Birth time errors (even small ones) can shift Lagna and divisional placements.

> [!TIP] Reference Box
> Infra references: **[1], [2], [3], [4], [5]**.

---

## 2) Conversion Equations (Tropical → Sidereal)

Vedic astrology uses the **Sidereal Zodiac**. Convert tropical longitudes by subtracting an Ayanamsa value.

### 2.1 Ayanamsa (Lahiri / Chitrapaksha)

Lahiri is the most widely adopted practical standard.

- Approximate formula:

\[
\text{Ayanamsa} = 23.85 + 0.0139 \times (\text{Year} - 2000)
\]

- Example reference value (2025): approximately **24.31°**.
- Final conversion:

\[
\text{Sidereal Longitude} = \text{Tropical Longitude} - \text{Ayanamsa}
\]

> [!TIP] Reference Box
> Ayanamsa references: **[6], [7], [8], [9]**.

---

## 3) Panchang Computation Spine

Use Sun longitude \(L_s\) and Moon longitude \(L_m\):

- **Tithi**: \((L_m - L_s) \div 12^\circ\) → integer result **0–29**.
- **Nakshatra**: \(L_m \div 13^\circ20'\) → integer result **0–26**.
- **Yoga**: \((L_s + L_m) \div 13^\circ20'\).
- **Karana**: half of Tithi span = **6°**.
- **Vara**: weekday from Julian Day.

> [!TIP] Reference Box
> Panchang references: **[1]**.

---

## 4) Harmonic Spine (Divisional Charts / Vargas)

Each Varga is a mathematical subdivision of a **30° Rashi**.

- General rule for chart **D-X**:

\[
\text{Division Span} = 30^\circ / X
\]

### Key examples
- **Navamsa (D-9)**: 9 parts, each **3°20'**.
- **Dasamsa (D-10)**: 10 parts, each **3°**.
- **Shastiamsa (D-60)**: 60 parts, each **0.5°**.

> [!TIP] Reference Box
> Divisional-chart references: **[4], [5], [10], [11], [12]**.

---

## 5) Predictive Spine (Vimshottari Dasha)

- Total cycle length: **120 years**.
- Basis: Moon’s longitude within birth Nakshatra.

If a planet’s Mahadasha length is \(Y\) years and the Moon has already traversed fraction \(P\) of the Nakshatra at birth:

\[
\text{Dasha Balance at Birth} = Y \times (1 - P)
\]

> [!TIP] Reference Box
> Dasha references: **[5], [13]**.

---

## 6) Sample Python Script (pyswisseph)

This script provides the calculative baseline for generating Sidereal planetary positions and Lagna.

### 6.1 Prerequisite

```bash
pip install pyswisseph
```

### 6.2 Core Script

```python
import swisseph as swe

# 1) Set Ayanamsa to Lahiri (Chitrapaksha)
swe.set_sid_mode(swe.SIDM_LAHIRI)


def get_sidereal_positions(year, month, day, hour_utc, lat, lon):
    # Convert time to Julian Day
    jd_ut = swe.julday(year, month, day, hour_utc)

    # Define planets (Swiss Ephemeris IDs)
    planet_list = {
        "Sun": swe.SUN,
        "Moon": swe.MOON,
        "Mars": swe.MARS,
        "Mercury": swe.MERCURY,
        "Jupiter": swe.JUPITER,
        "Venus": swe.VENUS,
        "Saturn": swe.SATURN,
        "Rahu": swe.MEAN_NODE,
    }

    zodiac = [
        "Aries", "Taurus", "Gemini", "Cancer", "Leo", "Virgo",
        "Libra", "Scorpio", "Sagittarius", "Capricorn", "Aquarius", "Pisces"
    ]

    output = {}

    # 2) Calculate planetary positions
    for name, p_id in planet_list.items():
        # FLG_SIDEREAL is key for Vedic output
        res, _ret = swe.calc_ut(jd_ut, p_id, swe.FLG_SIDEREAL)
        longitude = res[0]

        output[name] = {
            "sign": zodiac[int(longitude / 30)],
            "degree": longitude % 30,
            "is_retrograde": res[3] < 0,
        }

    # 3) Calculate Lagna (Ascendant)
    # 'W' = Whole Sign Houses (commonly used in Vedic workflows)
    _cusps, ascmc = swe.houses_ex(jd_ut, lat, lon, b"W", swe.FLG_SIDEREAL)
    output["Lagna"] = {
        "sign": zodiac[int(ascmc[0] / 30)],
        "degree": ascmc[0] % 30,
    }

    return output


# Usage Example:
# Delhi, April 23, 2026, 10:00 AM UTC
# (Always convert local birth time to UTC before input.)
data = get_sidereal_positions(2026, 4, 23, 10.0, 28.6139, 77.2090)
print(data)
```

---

## 7) Critical Extensions

To evolve this into production-grade Kundali software:

- **Timezone Engine**: use `pytz`/`zoneinfo` to convert local birth time to UTC correctly.
- **Nakshatra Mapper**: lookup index = \(\text{Longitude} \div 13^\circ20'\), range **0–26**.
- **Dasha Engine**: compute Nakshatra fraction traversed and remaining Dasha balance.

> [!TIP]
> Next practical step: wire this script to city resolution + tzdb conversion, then validate against known charts from a trusted Panchang baseline.

---

## References

1. https://www.linkedin.com/pulse/master-time-vedic-math-calculate-panchang-birth-charts-joshi-qiclc
2. https://asthatechnologies.org/develop-an-astrology-app-like-astrotalk/
3. https://www.quora.com/How-do-I-write-a-software-for-astrology
4. https://vedika.io/blog/kundli-software-development
5. https://vedika.io/blog/kundli-software-development
6. https://storage.yandexcloud.net/j108/forum/post/5ktb6ijlcfjz/Ayanamsa_-_A_Statistical_Study.pdf
7. https://www.astroaura.net/ayanamsa-calculation.html
8. https://roxyapi.com/blogs/ayanamsa-lahiri-raman-kp-developers
9. https://vastuguruji.com/ayanamsa/
10. https://www.scribd.com/doc/213840297/Divisional-Charts
11. https://sciphilconf.berkeley.edu/sites/mL5BCB/602676/Vedic%20Astrology%20Divisional%20Charts.pdf
12. https://www.quora.com/How-do-we-calculate-all-divisional-charts-Varga-Charts-from-Rasi-Chart-D1
13. https://archive.org/download/astronomy-and-mathematical-astrology/%5BDeepak%20Kapoor%5D%20Astronomy%20and%20Mathematical%20Astrology_text.pdf
