# 🏁 F1 2021 Season Analysis

## Overview
This project analyzes the 2021 Formula 1 season through a relational database model and an interactive Power BI dashboard. The goal was to design a clean, normalized data structure and use it to uncover performance patterns across drivers, constructors, and circuits throughout the season.

## Objectives
**SMART:**
- **Specific:** Design a relational database for the 2021 F1 season with normalized tables
- **Measurable:** Deliver a clean dataset ready for statistical analysis and visualization
- **Achievable:** Built using accessible tools (SQL, Excel, Power BI)
- **Relevant:** Enable performance analysis by driver, team, and circuit

**OKRs:**
- **O1:** Build a structured and documented F1 2021 database
  - KR1: Achieve 100% consistency in normalized identifiers
  - KR2: Implement an E-R diagram accurately reflecting relationships between drivers, races, and constructors
  - KR3: Generate a consolidated dataset ready for exploratory analysis in Power BI

## Data Model
Relational model with 5 normalized tables: **Drivers**, **Constructors**, **Circuits**, **Grand Prix**, and **Results**, connected through primary/foreign keys (driverId, constructorId, circuitId, grandprixId).

![E-R Diagram] 
e-r-diagram.png

## SQL Queries
Five business questions were answered using SQL (JOINs, GROUP BY, aggregations):
1. Which driver won at each circuit?
2. Final Constructors' Championship standings
3. Youngest driver on the grid
4. Which drivers won races during the season?
5. Final Drivers' Championship standings

## DAX Measures (Power BI)

Total Puntos Piloto = SUM(resultados[points])

Victorias = 
CALCULATE(
    COUNTROWS(resultados),
    resultados[position] = 1
)

Posición Promedio = AVERAGE(resultados[position])

Campeon 2021 = 
VAR TablaPuntos = 
    SUMMARIZE(
        pilotos,
        pilotos[driverId],
        "Puntos2021", [Total Puntos Piloto]
    )
VAR CampeonRow = 
    TOPN(1, TablaPuntos, [Puntos2021], DESC)
VAR NombreCampeon = 
    CALCULATE(
        MAX(pilotos[forename]) & " " & MAX(pilotos[surname]),
        KEEPFILTERS(CampeonRow)
    )
RETURN
    NombreCampeon

Pole Positions = 
CALCULATE(
    COUNTROWS(resultados),
    resultados[grid] = 1
)


## Dashboard
Two-page interactive Power BI report:

**Page 1 — Season Overview:** championship leaders, points evolution, constructor/driver standings
**Page 2 — Driver Deep-Dive:** DNFs, podiums, pole positions, consistency breakdown per selected driver

![Season Overview](images/dashboard-overview.png)
![Driver Deep-Dive](images/dashboard-driver.png)

🔗 **[View live interactive dashboard](#)** *(Power BI Service link)*

## Key Findings
- Max Verstappen won the 2021 Drivers' Championship by just 3 points over Lewis Hamilton (388 vs. 385), driven by a higher win count (10 vs. 8)
- Mercedes secured the Constructors' Championship (604 pts) despite Red Bull's strong late-season push (578 pts)
- Yuki Tsunoda was the youngest driver on the 2021 grid at 20 years old

## Tools Used
SQL Server · Power BI · DAX · Excel

## Future Improvements
- Predictive analysis by circuit using weather and lap data
- Integration of qualifying session data (Q1/Q2/Q3)
- Reliability and pit stop efficiency KPIs
- Multi-season historical trend analysis (2018–2023)
