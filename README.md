#SQL Practice Solutions  

---

## 🚀 Problem 1: Get Number of Vehicles  

### 💡 Sol  
```sql
SELECT make_id, COUNT(*) AS vehicle_count
FROM vehicles
WHERE year BETWEEN 1950 AND 2000
GROUP BY make_id
ORDER BY vehicle_count DESC;
