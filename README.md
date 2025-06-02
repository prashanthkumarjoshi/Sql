# Sql
emp
## 8. List employees with irregular attendance patterns.
<details><summary>
<strong>Description</strong>: This query identifies employees with irregular attendance, including frequent absences, late arrivals, or inconsistent check-in times. It filters active employees and ranks those with more than 5 irregular days in the last 2 months.</summary>
<br><strong>SQL Code</strong>

```sql
SELECT e.EmployeeID, e.FullName, 
       COUNT(*) AS IrregularDays,
       ROUND(AVG(EXTRACT(HOUR FROM a.CheckInTime)), 2) AS AvgCheckInHour
FROM Attendance a
JOIN Employees e ON a.EmployeeID = e.EmployeeID
WHERE e.Status = 'Active'
AND (
    a.AttendanceStatus IN ('Absent', 'Late') -- Frequently absent or late
    OR EXTRACT(HOUR FROM a.CheckInTime) NOT BETWEEN 8 AND 10 -- Unusual check-in hours
)
AND a.AttendanceDate >= CURRENT_DATE - INTERVAL '2 months'
GROUP BY e.EmployeeID, e.FullName
HAVING COUNT(*) > 5 -- Employees with irregular attendance more than 5 times
ORDER BY IrregularDays DESC;
```
</details>
<details>
<summary><strong>Expected Output</strong>: The results help track attendance patterns, highlighting employees with significant irregularities for further HR review</summary>
<br><strong>Query Output</strong>
 <br> <img src="nan" height="200">
</details>

