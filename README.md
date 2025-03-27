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
