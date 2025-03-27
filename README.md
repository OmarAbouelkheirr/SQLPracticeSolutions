# 17 - Database - SQL (Projects & Practice)

**[Course Link](https://programmingadvices.com/courses/17-database-sql-practice)**

---

## 🚀 Problem 1: Create Master View

### 💡 Solution:
```sql
CREATE VIEW VehicleMasterDetails AS
SELECT 
    VehicleDetails.ID, 
    VehicleDetails.MakeID, 
    Makes.Make, 
    VehicleDetails.ModelID, 
    MakeModels.ModelName, 
    VehicleDetails.SubModelID, 
    SubModels.SubModelName,
    VehicleDetails.Vehicle_Display_Name, 
    VehicleDetails.Year, 
    VehicleDetails.DriveTypeID, 
    DriveTypes.DriveTypeName, 
    VehicleDetails.Engine, 
    VehicleDetails.Engine_CC,
    VehicleDetails.Engine_Cylinders, 
    VehicleDetails.Engine_Liter_Display, 
    VehicleDetails.FuelTypeID, 
    FuelTypes.FuelTypeName, 
    VehicleDetails.NumDoors
FROM VehicleDetails 
JOIN Makes ON VehicleDetails.MakeID = Makes.MakeID
JOIN MakeModels ON VehicleDetails.ModelID = MakeModels.ModelID
JOIN SubModels ON VehicleDetails.SubModelID = SubModels.SubModelID
JOIN DriveTypes ON VehicleDetails.DriveTypeID = DriveTypes.DriveTypeID
JOIN FuelTypes ON VehicleDetails.FuelTypeID = FuelTypes.FuelTypeID;
```

---

## 🚀 Problem 2: Get all vehicles made between 1950 and 2000

### 💡 Solution:
```sql
select * from VehicleDetails 
where Year between 1950 and 2000
```

---

## 🚀 Problem 3 : Get number vehicles made between 1950 and 2000

### 💡 Solution:
```sql
select count(*) from VehicleDetails 
where Year between 1950 and 2000
```

--- 

## 🚀 Problem 4 : Get number vehicles made between 1950 and 2000 per make and order them by Number Of Vehicles Descending

### 💡 Solution:
```sql
select Makes.Make, count(*) as NumberOfVehicles from VehicleDetails 
join Makes on VehicleDetails.MakeID = Makes.MakeID
where Year between 1950 and 2000
group by Makes.Make
order By NumberOfVehicles desc
```

--- 

## 🚀 Problem 5 : Get All Makes that have manufactured more than 12000 Vehicles in years 1950 to 2000

### 💡 Solution One (With having):
```sql
select Makes.Make, count(*) as NumberOfVehicles from VehicleDetails 
join Makes on VehicleDetails.MakeID = Makes.MakeID
where Year between 1950 and 2000
group by Makes.Make
having count(*) > 12000
order By NumberOfVehicles desc
```

### 💡 Solution Two (Without having):
```sql
select * from (
select Makes.Make, count(*) as NumberOfVehicles from VehicleDetails 
join Makes on VehicleDetails.MakeID = Makes.MakeID
where Year between 1950 and 2000
group by Makes.Make
) R1
where R1.NumberOfVehicles > 12000;
```

--- 

## 🚀 Problem 6: Get number of vehicles made between 1950 and 2000 per make and add total vehicles column beside

### 💡 Solution:
```sql
select Makes.Make, count(*) as NumberOfVehicles, (select count(*) from VehicleDetails) as TotalVehicles from VehicleDetails 
join Makes on VehicleDetails.MakeID = Makes.MakeID
where Year between 1950 and 2000
group by Makes.Make
order By NumberOfVehicles desc
```

--- 

## 🚀 Problem 7: Get number of vehicles made between 1950 and 2000 per make and add total vehicles column beside it, then calculate it's percentage

### 💡 Solution:
```sql
--count(*)/(select count(*) from VehicleDetails) as Perc
select *,  Cast(NumberOfVehicles as float)/Cast (TotalVehicles as float) as Perc from
(
select Makes.Make, count(*) as NumberOfVehicles, TotalVehicles=(select count(*) from VehicleDetails) from VehicleDetails 
join Makes on VehicleDetails.MakeID = Makes.MakeID
where Year between 1950 and 2000
group by Makes.Make
) T1
order By T1.NumberOfVehicles desc
```

--- 

## 🚀 Problem 8: Get Make, FuelTypeName and Number of Vehicles per FuelType per Make

### 💡 Solution:
```sql
select Makes.Make, FuelTypes.FuelTypeName, count(*) as NumberOfVehicles from VehicleDetails 
join Makes on VehicleDetails.MakeID = Makes.MakeID
join FuelTypes on FuelTypes.FuelTypeID = VehicleDetails.FuelTypeID
where Year between 1950 and 2000
group by FuelTypes.FuelTypeName, Makes.Make
order by Makes.Make
```

--- 

## 🚀 Problem 9: Get all vehicles that runs with GAS

### 💡 Solution:
```sql
SELECT VehicleDetails.*, FuelTypes.FuelTypeName 
FROM VehicleDetails 
JOIN FuelTypes ON FuelTypes.FuelTypeID = VehicleDetails.FuelTypeID
WHERE FuelTypes.FuelTypeName = 'GAS';
```

--- 

## 🚀 Problem 10: Get all Makes that runs with GAS

### 💡 Solution:
```sql
SELECT distinct Makes.Make, FuelTypes.FuelTypeName 
FROM VehicleDetails 
JOIN FuelTypes ON FuelTypes.FuelTypeID = VehicleDetails.FuelTypeID
join Makes on Makes.MakeID = VehicleDetails.MakeID
WHERE FuelTypes.FuelTypeName = 'GAS';
```

--- 


## 🚀 Problem 11: Get Total Makes that runs with GAS

### 💡 Solution:
```sql
select count(*) from 
(
SELECT distinct Makes.Make, FuelTypes.FuelTypeName 
FROM VehicleDetails 
JOIN FuelTypes ON FuelTypes.FuelTypeID = VehicleDetails.FuelTypeID
join Makes on Makes.MakeID = VehicleDetails.MakeID
WHERE FuelTypes.FuelTypeName = 'GAS'
) T1

```

--- 


## 🚀 Problem 12: Count Vehicles by make and order them by NumberOfVehicles from high to low.

### 💡 Solution:
```sql
select Makes.Make, count(*) as NumberOfVehicles from VehicleDetails
join Makes on VehicleDetails.MakeID = Makes.MakeID
group by Makes.Make
order by NumberOfVehicles desc
```

--- 

## 🚀 Problem 13: Get all Makes/Count Of Vehicles that manufactures more than 20K Vehicles

### 💡 Solution One (With having):
```sql
select Makes.Make, count(*) as NumberOfVehicles from VehicleDetails
join Makes on VehicleDetails.MakeID = Makes.MakeID
group by Makes.Make
having count(*) > 20000
order by NumberOfVehicles desc
```

### 💡 Solution Two (Without having):
```sql
select * from 
(
select Makes.Make, count(*) as NumberOfVehicles from VehicleDetails
join Makes on VehicleDetails.MakeID = Makes.MakeID
group by Makes.Make
) T1
where T1.NumberOfVehicles > 20000
order by NumberOfVehicles desc
```

--- 

## 🚀 Problem 14: Get all Makes with make starts with 'B'

### 💡 Solution:
```sql
select Makes.Make from Makes
where Make like 'B%'
```

--- 

## 🚀 Problem 14: Get all Makes with make starts with 'B'

### 💡 Solution:
```sql
select Makes.Make from Makes
where Make like 'B%'
```

--- 

## 🚀 Problem 15: Get all Makes with make ends with 'W'

### 💡 Solution:
```sql
select Makes.Make from Makes
where Make like '%W'
```

--- 

## 🚀 Problem 16: Get all Makes that manufactures DriveTypeName = FWD

### 💡 Solution:
```sql
select distinct Makes.Make, DriveTypes.DriveTypeName from VehicleDetails 
join Makes on VehicleDetails.MakeID = Makes.MakeID
join DriveTypes on VehicleDetails.DriveTypeID = DriveTypes.DriveTypeID
where DriveTypeName = 'FWD'
```

--- 

## 🚀 Problem 17: Get total Makes that Mantufactures DriveTypeName=FWD

### 💡 Solution:
```sql
select distinct Makes.Make, DriveTypes.DriveTypeName from VehicleDetails 
join Makes on VehicleDetails.MakeID = Makes.MakeID
join DriveTypes on VehicleDetails.DriveTypeID = DriveTypes.DriveTypeID
where DriveTypeName = 'FWD'
```
---

## 🚀 Problem 18: Get total vehicles per DriveTypeName Per Make and order them per make asc then per total Desc

### 💡 Solution:
```sql
select distinct Makes.Make, DriveTypes.DriveTypeName, count(*) as Total
from VehicleDetails 
join Makes on VehicleDetails.MakeID = Makes.MakeID
join DriveTypes on VehicleDetails.DriveTypeID = DriveTypes.DriveTypeID
group by Makes.Make, DriveTypes.DriveTypeName
order by make asc, total desc
```

--- 

## 🚀 Problem 19: Get total vehicles per DriveTypeName Per Make then filter only results with total > 10,000

### 💡 Solution:
```sql
select distinct Makes.Make, DriveTypes.DriveTypeName, count(*) as Total
from VehicleDetails 
join Makes on VehicleDetails.MakeID = Makes.MakeID
join DriveTypes on VehicleDetails.DriveTypeID = DriveTypes.DriveTypeID
group by Makes.Make, DriveTypes.DriveTypeName
having count(*) > 10000
order by make asc, total desc
```

---

## 🚀 Problem 20: Get all Vehicles that number of doors is not specified

### 💡 Solution:
```sql
select * from VehicleDetails 
where NumDoors is null
```

--- 

## 🚀 Problem 21: Get Total Vehicles that number of doors is not specified

### 💡 Solution:
```sql
select count(*) as Total from VehicleDetails 
where NumDoors is null
```

--- 

## 🚀 Problem 22: Get percentage of vehicles that has no doors specified

### 💡 Solution:
```sql
select 
(
Cast( (select count(*) as Total from VehicleDetails where NumDoors is null) as float) / Cast( (select count(*) from VehicleDetails) as float)
) as PercOfNoSpecifiedDoors
```

--- 

## 🚀 Problem 23: Get MakeID , Make, SubModelName for all vehicles that have SubModelName 'Elite'

### 💡 Solution:
```sql
select distinct Makes.MakeID, Makes.Make, SubModels.SubModelName from VehicleDetails
join Makes on Makes.MakeID = VehicleDetails.MakeID
join SubModels on SubModels.SubModelID = VehicleDetails.SubModelID

where SubModelName = 'Elite'
order by MakeID desc
```

--- 

## 🚀 Problem 24: Get all vehicles that have Engines > 3 Liters and have only 2 doors

### 💡 Solution:
```sql
select * from VehicleDetails 
where Engine_Liter_Display > 3 and NumDoors = 2
```

--- 

## 🚀 Problem 25: Get make and vehicles that the engine contains 'OHV' and have Cylinders = 4

### 💡 Solution:
```sql
select Makes.Make, VehicleDetails.* from Makes
join VehicleDetails on Makes.MakeID = VehicleDetails.MakeID
where Engine LIKE '%OHV%' and Engine_Cylinders = 4
```

--- 
## 🚀 Problem 26: Get all vehicles that their body is 'Sport Utility' and Year > 2020

### 💡 Solution:
```sql
select * from VehicleDetails
join Bodies on VehicleDetails.BodyID = Bodies.BodyID
where BodyName = 'Sport Utility' and Year = 2020
```

---

## 🚀 Problem 27: Get all vehicles that their Body is 'Coupe' or 'Hatchback' or 'Sedan'

### 💡 Solution:
```sql
select * from VehicleDetails
join Bodies on VehicleDetails.BodyID = Bodies.BodyID
where BodyName in ('Coupe', 'Hatchback', 'Sedan')
```

--- 

## 🚀 Problem 28: Get all vehicles that their body is 'Coupe' or 'Hatchback' or 'Sedan' and manufactured in year 2008 or 2020 or 2021

### 💡 Solution:
```sql
select * from VehicleDetails
join Bodies on VehicleDetails.BodyID = Bodies.BodyID
where BodyName in ('Coupe', 'Hatchback', 'Sedan') and Year in (2008, 2020, 2021)
```

--- 

## 🚀 Problem 29: Return found=1 if there is any vehicle made in year 1950

### 💡 Solution:
```sql
select found = 1 
where exists ( select top 1 * from VehicleDetails where Year =1950 )
```

--- 

## 🚀 Problem 30: Get all Vehicle_Display_Name, NumDoors and add extra column to describe number of doors by words, and if door is null display 'Not Set'

### 💡 Solution:
```sql
 select Vehicle_Display_Name, NumDoors, DoorDesciption =
		case 
			when NumDoors is null then 'Not Set'
			when NumDoors = 0 then 'Zero Door'
			when NumDoors = 1 then 'One Door'
			when NumDoors = 2 then 'Two Doors'
			when NumDoors = 3 then 'Three Doors'
			when NumDoors = 4 then 'Four Doors'
			when NumDoors = 5 then 'Five Doors'
			when NumDoors = 6 then 'Six Doors'
			when NumDoors = 8 then 'Eight Doors'
		end
from VehicleDetails
```

---

## 🚀 Problem 31: Get all Vehicle_Display_Name, year and add extra column to calculate the age of the car then sort the results by age desc.

### 💡 Solution:
```sql
Select VehicleDetails.Vehicle_Display_Name, Year, Age=(YEAR(GetDate()) - VehicleDetails.year)
from VehicleDetails
Order by Age Desc
```

---

## 🚀 Problem 32: Get all Vehicle_Display_Name, year, Age for vehicles that their age between 15 and 25 years old

### 💡 Solution:
```sql
select * from
( 
	select VehicleDetails.Vehicle_Display_Name, Year, Age=(YEAR(GetDate()) - VehicleDetails.year)
	from VehicleDetails
) T1

where Age between 15 and 25
```

---

## 🚀 Problem 33: Get Minimum Engine CC , Maximum Engine CC , and Average Engine CC of all Vehicles

### 💡 Solution:
```sql
select min(VehicleDetails.Engine_CC) as minEngineCC, max(VehicleDetails.Engine_CC) as maxEngineCC, avg(VehicleDetails.Engine_CC) as avgEngineCC from VehicleDetails

```

---

## 🚀 Problem 34: Get all vehicles that have the minimum Engine_CC

### 💡 Solution:
```sql
SELECT * FROM VehicleDetails
WHERE VehicleDetails.Engine_CC = (SELECT MIN(VehicleDetails.Engine_CC) FROM VehicleDetails)
```

---

## 🚀 Problem 35: Get all vehicles that have the Maximum Engine_CC

### 💡 Solution:
```sql
select * from VehicleDetails
where Engine_CC = ( select  Max(Engine_CC) as MinEngineCC  from VehicleDetails )
```

---

## 🚀 Problem 36: Get all vehicles that have Engin_CC below average

### 💡 Solution:
```sql
Select * from VehicleDetails
where Engine_CC < ( select  avg(Engine_CC) as MinEngineCC  from VehicleDetails )
```

---

## 🚀 Problem 37: Get total vehicles that have Engin_CC above average

### 💡 Solution:
```sql
select count(*) from
(
	Select * from VehicleDetails
	where Engine_CC > ( select  avg(Engine_CC) as MinEngineCC from VehicleDetails )
) T1
```

---

## 🚀 Problem 38: Get all unique Engin_CC and sort them Desc

### 💡 Solution:
```sql
select distinct Engine_CC from VehicleDetails
order by Engine_CC desc
```

---

## 🚀 Problem 39: Get the maximum 3 Engine CC

### 💡 Solution:
```sql
select distinct top 3 Engine_CC from VehicleDetails
order by Engine_CC desc
```

---

## 🚀 Problem 40: Get all vehicles that has one of the Max 3 Engine CC

### 💡 Solution:
```sql
select Vehicle_Display_Name from VehicleDetails
where Engine_CC in 
(
	select  distinct top 3 Engine_CC from VehicleDetails
	order By Engine_CC desc
)
```

---

## 🚀 Problem 41: Get all Makes that manufactures one of the Max 3 Engine CC

### 💡 Solution:
```sql
select distinct Makes.Make from VehicleDetails
JOIN Makes ON VehicleDetails.MakeID = Makes.MakeID
where (VehicleDetails.Engine_CC IN (
select distinct top (3) Engine_CC from VehicleDetails 
order by Engine_CC desc
) )
order By Make
```

---

## 🚀 Problem 42: Get a table of unique Engine_CC and calculate tax per Engine CC

### 💡 Solution:
```sql
select Engine_CC,
	CASE
		WHEN Engine_CC between 0 and 1000 THEN 100
		 WHEN Engine_CC between 1001 and 2000 THEN 200
		 WHEN Engine_CC between 2001 and 4000 THEN 300
		 WHEN Engine_CC between 4001 and 6000 THEN 400
		 WHEN Engine_CC between 6001 and 8000 THEN 500
		 WHEN Engine_CC > 8000 THEN 600	
		ELSE 0

	END as Tax

from 
(
	select distinct Engine_CC from VehicleDetails
) T1
order by Engine_CC
```

---

## 🚀 Problem 43: Get Make and Total Number Of Doors Manufactured Per Make

### 💡 Solution:
```sql
select Makes.Make, Sum(VehicleDetails.NumDoors) as TotalNumberOfDoors
from VehicleDetails
join Makes on VehicleDetails.MakeID = Makes.MakeID
Group By Make
Order By TotalNumberOfDoors desc
```

---

## 🚀 Problem 44: Get Total Number Of Doors Manufactured by 'Ford'

### 💡 Solution:
```sql
SELECT Makes.Make, Sum(VehicleDetails.NumDoors) AS TotalNumberOfDoors
FROM VehicleDetails
JOIN Makes ON VehicleDetails.MakeID = Makes.MakeID
Group By Make
Having Make='Ford'
```

---

## 🚀 Problem 45: Get Number of Models Per Make

### 💡 Solution:
```sql
SELECT Makes.Make, COUNT(*) AS NumberOfModels
FROM Makes
JOIN MakeModels ON Makes.MakeID = MakeModels.MakeID
GROUP BY Makes.Make
Order By NumberOfModels Desc
```

---
