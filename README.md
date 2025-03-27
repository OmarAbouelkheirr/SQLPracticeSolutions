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

### 💡 Solution One:
```sql
select Makes.Make, count(*) as NumberOfVehicles from VehicleDetails 
join Makes on VehicleDetails.MakeID = Makes.MakeID
where Year between 1950 and 2000
group by Makes.Make
having count(*) > 12000
order By NumberOfVehicles desc
```

### 💡 Solution Two:
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

