# 17 - Database - SQL (Projects & Practice)

[Course Link](https://programmingadvices.com/courses/17-database-sql-practice)

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
