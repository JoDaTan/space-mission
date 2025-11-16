# Maven Return to Space Challenge — Space Missions Analysis (1957–2022)

This project is my submission for the Maven Analytics [Return to Space Challenge (Oct–Nov 2025)](https://mavenanalytics.io/challenges/maven-return-to-space-challenge).
Using the global dataset of space missions from 1957 to 2022, I developed a one-page Power BI visualisation that tells the story of humanity’s journey to space.

The dashboard is designed to answer **three Big Space Questions:**
1. What is the golden era of space travel
2. Which rocket is the champion of space travel
3. What is the winning team (Country/Company) and its strategy?

# The Big Space Questions

## 1. The Golden Era of Space Travel
The 1970s stand out as the golden age of spaceflight, with **1,012 missions** and a **92.7% success rate**.
Russia led this era, with **RVSN USSR** emerging as a launch powerhouse:
- 814 missions
- 94.2% success rate
- An astonishing launch pace of one mission every 5 days

## 2. Champion of Space Travel
Using only Total Launches and Success Rate, I ranked rockets based on their operational history and reliability.
The visualisation highlights top performers like **Cosmos-3M, Voskhod, and Molniya-M,** showing how launch frequency and success rate together define rocket excellence.

## 3. The Winning Team and Its Strategy
Although the United States launched slightly more missions (1,467) than Russia (1,416), Russia emerged as the long-term leader with a **93.4% success rate** — the highest among major spacefaring nations.
Key to this dominance:
- RVSN USSR sponsored nearly **1,200 missions**
- Maintained an average mission interval of just **9 days**
- For comparison, the next fastest program averaged 57 days per mission
Their strategy? **High-frequency, high-reliability launches that sustained momentum over decades.**


# Data & Structure
Source: Provided by Maven Analytics

Files Included:
- space_mission.csv
- space_mission_data_dictionary.csv

Model Structure - Star Schema
- Fact: Missions
- Dimensions: Company, Location, Date, Rocket

# Key DAX Measures
1. Total Launch
``` DAX
Total Launch = 
COUNTROWS(
    missions
)
```
2. Success Rate
``` DAX
Success Rate =
DIVIDE(
    CALCULATE(COUNTROWS('Missions'), 'Missions'[Status] = "Success"),
    COUNTROWS('Missions')
)
```
3. Average Mission Interval (Days)
```
Avg Mission Interval =
VAR PrevMissionDate =
    CALCULATE(
        MAX('Missions'[Date]),
        FILTER(
            'Missions',
            'Missions'[Company] = EARLIER('Missions'[Company]) &&
            'Missions'[Date] < EARLIER('Missions'[Date])
        )
    )
VAR MissionInterval =
    IF(
        NOT ISBLANK(PrevMissionDate),
        DATEDIFF(PrevMissionDate, 'Missions'[Date], DAY)
    )
RETURN
    AVERAGEX(
        VALUES('Missions'[Mission ID]),
        MissionInterval
    )
```

# Visuals Included
- Total Mission by Country
- Mission Success Trend
- Rocket Launch v Success rate (rocket performance)
- Country and Company ranking
