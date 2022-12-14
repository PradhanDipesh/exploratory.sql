select * 
From portfolioproject.dbo.[Covid death]
order by 3,4


select * 
From portfolioproject.dbo.['Covid vaccination$']
order by 3,4

---select data that we are going to be using

select location, date, total_cases, new_cases, total_deaths, Population
from portfolioproject..[Covid death]
order by 1,2

----looking at total cases Vs total deaths
select location, date, total_cases,total_deaths,(total_deaths/total_cases)*100 as deathpercentage
from portfolioproject..[Covid death]
where location like '%states%'
order by 1,2


--- looking at total cases vs population
--shows what percentage of population got Covid
select location, date, total_cases, population, (total_cases/population)*100 as deathpercentage
from portfolioproject..[Covid death]
where location like '%states%'
order by 1,2

--looking at countries with highest infection rate compared to population

select location, population, max(total_cases) as highestInfectionCount,max (total_cases/population)*100 as percentpopulationinfected
from portfolioproject..[Covid death]
--where location like '%states%'
group by location, population
order by percentpopulationinfected desc

--showing countries with highest death count per population

select location, Max(cast(total_deaths as int)) as totaldeathcount
from portfolioproject..[Covid death]
where continent is not null
group by location
order by totaldeathcount desc 

---LET'S BREAK THINGS DOWN BY CONTINENT
select location, Max(cast(total_deaths as int)) as totaldeathcount
from portfolioproject..[Covid death]
where continent is null
group by location
order by totaldeathcount desc 

---showing the continent with the highest death count per population
select location, Max(cast(total_deaths as int)) as totaldeathcount
from portfolioproject..[Covid death]
where continent is not null
group by continent
order by totaldeathcount desc


---global numbers
select date, Sum(new_cases) as total_cases, Sum(cast(new_deaths as int))as total_deaths, sum(cast(new_deaths as int))/
sum(new_cases)*100 as deathpercentage
from portfolioproject..[Covid death]
--where location like '%states%'
where continent is not null
group by date
order by 1,2

select * from portfolioproject..['Covid vaccination$']


--- joining two table covid deaths and covid vaccination
Select *
from portfolioproject..[Covid death] dea
join portfolioproject..['Covid vaccination$'] vac
on dea.location = vac.location
and dea.date=vac.date

--looking at total population Vs Vaccinations
Select dea.continent, dea.location, dea.date,dea.population,vac.new_vaccinations
from portfolioproject..[Covid death]dea
join portfolioproject..['Covid vaccination$'] vac
on dea.location= vac.location
and dea.date=vac.date
where dea.continent is not null
order by 2,3

