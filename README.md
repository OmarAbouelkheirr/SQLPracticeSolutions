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

