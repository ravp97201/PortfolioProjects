# COVID-19 Data Exploration Project

## Project Overview
This SQL project explores global COVID-19 data to uncover trends in cases, deaths, and vaccinations. Using SQL Server Management Studio, I performed data analysis on two key datasets to answer questions about infection rates, death percentages, and vaccine rollouts worldwide.

## Dataset
- CovidDeaths
- CovidVaccinations


Key Analyses

 1. Total Cases vs Total Deaths
Calculated the likelihood of dying if infected with COVID-19:

SELECT location, date, total_cases, total_deaths,
(total_deaths/total_cases)*100 AS DeathPercentage
FROM portfolioProject..CovidDeaths
WHERE location LIKE '%states%'
AND continent IS NOT NULL
ORDER BY 1,2;

2. Total Cases vs Population
Determined what percentage of each country's population was infected:

SELECT location, date, population, total_cases,
(total_cases/population)*100 AS PercentPopulationInfected
FROM portfolioProject..CovidDeaths
ORDER BY 1,2;

3. Highest Infection Rates
Identified countries with the highest infection rate relative to population:

SELECT location, population,
MAX(total_cases) AS HighestInfectionCount,
MAX((total_cases/population))*100 AS PercentPopulationInfected
FROM portfolioProject..CovidDeaths
GROUP BY location, population
ORDER BY PercentPopulationInfected DESC;

4. Highest Death Counts

By Country:

SELECT location, MAX(total_deaths) AS TotalDeathCount
FROM portfolioProject..CovidDeaths
WHERE continent IS NOT NULL
GROUP BY location
ORDER BY TotalDeathCount DESC;

By Continent:

SELECT continent, MAX(total_deaths) AS TotalDeathCount
FROM portfolioProject..CovidDeaths
WHERE continent IS NOT NULL
GROUP BY continent
ORDER BY TotalDeathCount DESC;

5. Global Trends Over Time
Tracked total new cases, deaths, and death percentage by date:

SELECT date,
SUM(new_cases) AS total_cases,
SUM(new_deaths) AS total_deaths,
SUM(new_deaths)/SUM(new_cases)*100 AS DeathPercentage
FROM portfolioProject..CovidDeaths
WHERE continent IS NOT NULL
GROUP BY date
ORDER BY 1,2;

6. Vaccination Progress
Used window functions and CTEs to calculate cumulative vaccination statistics:

SELECT dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations,
SUM(vac.new_vaccinations) OVER(
PARTITION BY dea.location
ORDER BY dea.location, dea.date) AS RollingPeopleVaccinated
FROM portfolioProject..CovidDeaths dea
JOIN portfolioProject..CovidVaccinations vac
ON dea.location = vac.location
AND dea.date = vac.date
WHERE dea.continent IS NOT NULL
ORDER BY 2,3;

Created:
- CTE for population vs vaccinations
- Temp table for % vaccinated
- View for visualization-ready data: PercentPopulationVaccinated

Key Insights
- Smaller countries often had higher infection percentages despite lower total case counts
- Death rates varied widely depending on country and region
- Vaccine rollout timelines were inconsistent across continents

Next Steps
- Create data visualizations using Tableau or Power BI
- Add forecasting using time-series analysis
- Incorporate interactive dashboards

Ravi Patel
[LinkedIn - Ravi Patel](https://www.linkedin.com/in/ravi-patel-2001rp413)
