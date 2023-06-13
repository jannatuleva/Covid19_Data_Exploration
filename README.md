
# Covid19_Data_Exploration

Identified the Covid-19 restrictions and analyzed pandemic, including updates on vaccinations. Gathered and investigated data from a trustworthy source. The data was organized and prepared using Excel, and then analyzed using Structured Query Language (SQL) to gain insights. Created an interactive Tableau dashboard that presents important details about the countries most impacted by the virus, the continents with the highest death rates, and the percentage of the global population vaccinated.


## Tech Stack

SQL, Excel, Tableau


# Covid19_Data_Exploration

Identified the Covid-19 restrictions and analyzed pandemic, including updates on vaccinations. Gathered and investigated data from a trustworthy source. The data was organized and prepared using Excel, and then analyzed using Structured Query Language (SQL) to gain insights. Created an interactive Tableau dashboard that presents important details about the countries most impacted by the virus, the continents with the highest death rates, and the percentage of the global population vaccinated.


## Running Tests

To run tests, run the following command

```bash
 ---Covid Death Data
Select *
From PortfolioProject..CovidDeaths
Where continent is not null 
order by 3,4


------Covid Vaccine Data
Select *
From PortfolioProject..CovidVaccinations
Where continent is not null 
order by 3,4

--Working data
Select Location, date, total_cases, new_cases, total_deaths, population
From PortfolioProject..CovidDeaths
Where continent is not null 
order by 1,2


--Necessary Covid Death info
SELECT location, population, date, MAX(total_cases) AS Highest_Infection_Count,
MAX((total_cases/population))* 100 AS Percent_Population_Infected
FROM PortfolioProject..CovidDeaths
GROUP BY location, population, date
ORDER BY Percent_Population_Infected DESC



--Most infected Countries

Select TOP 20 location, MAX(total_cases) AS Total_Cases
FROM PortfolioProject..CovidDeaths
WHERE continent is NOT NULL
GROUP BY location
Order by Total_Cases DESC


--Infection Count by Continent

SELECT Continent, SUM(new_cases) AS Total_Cases
FROM PortfolioProject..CovidDeaths
WHERE continent is not null
Group By continent
ORDER BY Total_Cases DESC


--Highest Death Countries

Select TOP 20 location, MAX(cast(total_deaths as int)) AS Total_Death
FROM PortfolioProject..CovidDeaths
WHERE continent is NOT NULL
GROUP BY location
Order by Total_Death DESC


--Total Cases VS Population of each country (Infection Percentage)

Select TOP 20 location, MAX(total_cases) AS Total_Cases, MAX(Population) AS Population,
(MAX(total_cases)/MAX(population))*100 as InfectionPercentage
FROM PortfolioProject..CovidDeaths
WHERE continent is NOT NULL
GROUP BY location
Order by InfectionPercentage DESC


--Total Cases VS Population of each Continent (Infection Percentage)

Select continent, MAX(total_cases) AS Total_Cases, MAX(Population) AS Population,
(MAX(total_cases)/MAX(population))*100 as InfectionPercentage
FROM PortfolioProject..CovidDeaths
WHERE continent is NOT NULL
GROUP BY continent
Order by InfectionPercentage DESC


--Death Count by Continent
Select continent, SUM(cast(total_deaths as int)) AS Total_Death
FROM PortfolioProject..CovidDeaths
WHERE continent is NOT NULL
GROUP BY continent
Order by Total_Death DESC


--Death Rate by Continent (Death Percentage)

SELECT continent, SUM(population) AS Population, SUM(new_cases) AS Total_Cases, 
SUM(CAST (new_deaths AS int)) AS Total_Death,
(SUM(CAST (new_deaths AS int))/SUM(new_cases))* 100 AS Death_Percentage
FROM PortfolioProject..CovidDeaths
WHERE continent is not null
GROUP BY continent
ORDER BY Death_Percentage DESC

 --Countries with Highest Infection Rate compared to Population

SELECT Location, Population, MAX(total_cases) AS Highest_Infection_Count, 
MAX(total_cases/ population)* 100 AS InfectionPercentage
FROM PortfolioProject..CovidDeaths
GROUP BY Location, Population 
ORDER BY InfectionPercentage DESC


--Mortality Rate of each continent

SELECT continent, MAX(total_cases) AS Cases, MAX(CAST(total_deaths AS int)) AS Total_Death_Count, 
(MAX(CAST(total_deaths AS int))/MAX(total_cases)) * 100 AS Mortality_Rate
FROM PortfolioProject..CovidDeaths
WHERE continent is not null
GROUP BY continent
ORDER BY Mortality_Rate DESC


--Continents with the highest death count per population

SELECT Continent, MAX(CAST(total_deaths AS int)) AS Total_Death_Count
FROM PortfolioProject..CovidDeaths
WHERE continent is not null
GROUP BY continent
ORDER BY Total_Death_Count DESC



--Death Percentage by Continent

SELECT continent, SUM(new_cases) AS Total_Cases, MAX(CAST(total_deaths AS int)) AS Total_deaths,  
SUM(CAST(new_deaths AS int))/SUM(New_Cases)* 100 AS DeathPercentage 
FROM PortfolioProject..CovidDeaths
WHERE continent is not null
GROUP BY continent
ORDER BY DeathPercentage DESC


SELECT continent, SUM(new_cases) AS Total_Cases, MAX(CAST(total_deaths AS int)) AS Total_deaths,  
SUM(CAST(new_deaths AS int))/SUM(new_cases)* 100 AS DeathPercentage 
FROM PortfolioProject..CovidDeaths
WHERE continent is not null
GROUP BY continent
ORDER BY DeathPercentage DESC


-- GLOBAL NUMBERS

Select SUM(new_cases) as total_cases, SUM(cast(new_deaths as int)) as total_deaths, 
SUM(cast(new_deaths as int))/SUM(New_Cases)*100 as DeathPercentage
From PortfolioProject..CovidDeaths
--Where location like '%states%'
where continent is not null 
--Group By date
order by 1,2

SELECT SUM(new_cases) AS Total_Cases, SUM(CAST(new_deaths AS int)) AS Total_Deaths, 
SUM(CAST(new_deaths AS int))/SUM(New_Cases)* 100 AS Death_Percentage 
FROM PortfolioProject..CovidDeaths
WHERE continent is not null
ORDER BY 1, 2

-- Total Population vs Vaccinations
-- Shows Percentage of Population that has recieved at least one Covid Vaccine

Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(CONVERT(int,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.Date) as RollingPeopleVaccinated
--, (RollingPeopleVaccinated/population)*100
From PortfolioProject..CovidDeaths dea
Join PortfolioProject..CovidVaccinations vac
	On dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null 
order by 2,3




```

