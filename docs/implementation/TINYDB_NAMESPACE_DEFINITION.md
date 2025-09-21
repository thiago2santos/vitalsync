# 📊 VitalSync - Complete TinyDB Namespace Definition

## 🎯 **Overview**

This document defines **all TinyDB tags (namespaces)** used across VitalSync, their data structures, and usage patterns. This is the **single source of truth** for data storage.

---

## 📋 **Complete TinyDB Tag Registry**

### **🏥 Health Data Tags**

#### **1. "vital_readings_bp" - Blood Pressure Readings**
```
Data Type: List of Lists
Structure: [timestamp, systolic, diastolic, pulse, notes, status]
Example: [
  ["2024-09-21 18:30:00", 120, 80, 72, "Normal reading", "Normal"],
  ["2024-09-21 14:20:00", 135, 88, 75, "After exercise", "High Stage 1"]
]
Usage: SaveBloodPressure(), LoadBPReadings()
Max Records: 100 (auto-cleanup)
```

#### **2. "vital_readings_hr" - Heart Rate Readings**
```
Data Type: List of Lists
Structure: [timestamp, bpm, resting_hr, notes, status]
Example: [
  ["2024-09-21 18:30:00", 72, 65, "At rest", "Normal"],
  ["2024-09-21 16:45:00", 145, 65, "After workout", "High"]
]
Usage: SaveHeartRate(), LoadHRReadings()
Max Records: 100 (auto-cleanup)
```

#### **3. "vital_readings_temp" - Temperature Readings**
```
Data Type: List of Lists
Structure: [timestamp, celsius, fahrenheit, location, notes, status]
Example: [
  ["2024-09-21 18:30:00", 36.5, 97.7, "Oral", "Normal check", "Normal"],
  ["2024-09-21 12:15:00", 37.8, 100.0, "Forehead", "Feeling unwell", "Fever"]
]
Usage: SaveTemperature(), LoadTempReadings()
Max Records: 100 (auto-cleanup)
```

#### **4. "vital_readings_weight" - Weight Readings**
```
Data Type: List of Lists
Structure: [timestamp, kg, pounds, bmi, notes, status]
Example: [
  ["2024-09-21 08:00:00", 70.5, 155.4, 23.2, "Morning weight", "Normal"],
  ["2024-09-20 08:00:00", 71.0, 156.5, 23.4, "After weekend", "Normal"]
]
Usage: SaveWeight(), LoadWeightReadings()
Max Records: 100 (auto-cleanup)
```

#### **5. "vital_readings_glucose" - Blood Glucose Readings**
```
Data Type: List of Lists
Structure: [timestamp, mg_dl, mmol_l, meal_context, notes, status]
Example: [
  ["2024-09-21 08:00:00", 95, 5.3, "Fasting", "Morning check", "Normal"],
  ["2024-09-21 20:30:00", 140, 7.8, "After Meal", "2h post dinner", "Normal"]
]
Usage: SaveGlucose(), LoadGlucoseReadings()
Max Records: 100 (auto-cleanup)
```

#### **6. "vital_readings_steps" - Daily Steps Data**
```
Data Type: List of Lists
Structure: [date, steps, distance_km, calories, active_minutes]
Example: [
  ["2024-09-21", 8500, 6.8, 340, 85],
  ["2024-09-20", 12000, 9.6, 480, 120]
]
Usage: SaveDailySteps(), LoadStepsData()
Max Records: 365 days (auto-cleanup)
```

---

### **📱 App Activity & Navigation Tags**

#### **7. "recent_activities" - Dashboard Activity Feed**
```
Data Type: List of Lists
Structure: [timestamp, activity_type, description, details]
Example: [
  ["09-21 18:30", "vital_added", "Added blood pressure", "120/80 mmHg (Normal)"],
  ["09-21 17:45", "med_taken", "Took medication", "Aspirin 100mg"],
  ["09-21 16:20", "med_added", "Added medication", "Vitamin D 1000IU"],
  ["09-21 15:10", "symptom_recorded", "Recorded symptom", "Headache - Level 3"],
  ["09-21 14:00", "login", "Logged in", "Welcome back!"]
]
Usage: LogActivity(), LoadRecentActivities()
Max Records: 20 (auto-cleanup)
Activity Types: "vital_added", "med_taken", "med_added", "symptom_recorded", "login", "profile_updated", "data_exported"
```

#### **8. "nav_current_screen" - Current Screen State**
```
Data Type: Text
Values: "Screen1", "scr_dashboard", "scr_vital_signs", "scr_medications", "scr_profile"
Example: "scr_vital_signs"
Usage: Navigation system, screen state persistence
```

#### **9. "nav_current_component" - Current Component State**
```
Data Type: Text
Values: "main", "add_vital", "add_med", "symptoms", "scanner", "settings"
Example: "add_vital"
Usage: Component navigation, state restoration
```

#### **10. "nav_history" - Navigation History Stack**
```
Data Type: List of Texts
Structure: [previous_screen, previous_component, ...]
Example: ["scr_dashboard", "main", "scr_vital_signs", "main"]
Usage: Back button navigation, breadcrumb display
Max Records: 10 (auto-cleanup)
```

---

### **👤 User & Profile Tags**

#### **11. "user_profile" - User Personal Information**
```
Data Type: List
Structure: [username, email, full_name, birth_date, height_cm, gender, blood_type]
Example: ["joao_silva", "joao@email.com", "João Silva", "1990-05-15", 175, "Masculino", "O+"]
Usage: Profile management, BMI calculations, personalization
```

#### **12. "user_health_info" - Health Profile Data**
```
Data Type: List
Structure: [allergies, medical_conditions, medications_list, emergency_contact]
Example: ["Penicilina", "Hipertensão", "Losartana 50mg", "Maria Silva - (11) 99999-9999"]
Usage: Medical profile, emergency information
```

#### **13. "logged_in" - Authentication State**
```
Data Type: Logic (Boolean)
Values: true, false
Example: true
Usage: Session management, auto-login
```

#### **14. "username" - Current User Identifier**
```
Data Type: Text
Example: "joao_silva"
Usage: Personalization, activity logging, data ownership
```

---

### **💊 Medication Management Tags**

#### **15. "medications_list" - Active Medications**
```
Data Type: List of Lists
Structure: [med_name, dosage, schedule_times, frequency, active_status, notes]
Example: [
  ["Losartana", "50mg", "08:00,20:00", "daily", true, "For blood pressure"],
  ["Vitamina D", "1000IU", "08:00", "daily", true, "Morning with breakfast"]
]
Usage: Medication management, reminder system
Max Records: 50 medications
```

#### **16. "medication_history" - Medication Taking History**
```
Data Type: List of Lists
Structure: [timestamp, med_name, dosage, status, notes]
Example: [
  ["2024-09-21 08:00:00", "Losartana", "50mg", "taken", "On time"],
  ["2024-09-21 20:00:00", "Losartana", "50mg", "skipped", "Forgot"]
]
Usage: Adherence tracking, medication reports
Max Records: 1000 entries (auto-cleanup after 90 days)
```

---

### **⚙️ App Settings & Configuration Tags**

#### **17. "app_settings" - Application Preferences**
```
Data Type: List
Structure: [notifications_enabled, reminder_sound, dark_mode, language, units_metric]
Example: [true, true, false, "pt-BR", true]
Usage: App behavior customization, user preferences
```

#### **18. "notification_settings" - Notification Preferences**
```
Data Type: List
Structure: [med_reminders, appointment_alerts, health_tips, vital_reminders]
Example: [true, true, false, true]
Usage: Notification system configuration
```

#### **19. "health_score_data" - Health Score Calculation**
```
Data Type: List
Structure: [current_score, bp_score, hr_score, weight_score, activity_score, last_updated]
Example: [85, 22, 25, 20, 18, "2024-09-21 18:30:00"]
Usage: Dashboard health score display, trend analysis
```

---

### **📊 Analytics & Reports Tags**

#### **20. "app_usage_stats" - Usage Analytics**
```
Data Type: List
Structure: [total_logins, vitals_recorded, meds_taken, last_active_date]
Example: [45, 120, 89, "2024-09-21"]
Usage: User engagement tracking, feature usage analysis
```

#### **21. "data_export_history" - Export History**
```
Data Type: List of Lists
Structure: [export_date, export_type, file_format, record_count]
Example: [
  ["2024-09-21", "vitals", "CSV", 45],
  ["2024-09-15", "medications", "TXT", 12]
]
Usage: Export tracking, data management
Max Records: 20 exports
```

---

## 🔧 **TinyDB Management Procedures**

### **Data Cleanup Procedure:**
```blocks
Procedure: CleanupTinyDB
Actions:
1. For each vital readings tag:
   - Keep only last 100 records
   - Remove records older than 1 year

2. For recent_activities:
   - Keep only last 20 activities
   - Remove activities older than 30 days

3. For medication_history:
   - Remove records older than 90 days
   - Keep minimum 100 most recent records

4. For navigation history:
   - Keep only last 10 navigation states

Call this procedure:
- On app startup (once per day)
- When storage exceeds limits
```

### **Data Validation Procedure:**
```blocks
Procedure: ValidateTinyDBData
Actions:
1. Check data structure integrity
2. Validate required fields exist
3. Convert old data formats if needed
4. Set default values for missing data
5. Log validation results

Call this procedure:
- On app startup
- After app updates
- When data corruption detected
```

---

## 📱 **Usage Guidelines**

### **Reading Data:**
```blocks
Standard Pattern:
1. Get data from TinyDB with default value
2. Validate data structure
3. Process data for display/calculation
4. Handle empty/corrupted data gracefully

Example:
set bp_readings to get value from TinyDB tag "vital_readings_bp"
if bp_readings is empty:
  set bp_readings to create empty list
```

### **Writing Data:**
```blocks
Standard Pattern:
1. Get existing data
2. Validate new data
3. Add new record to beginning of list
4. Apply size limits (cleanup old records)
5. Save back to TinyDB
6. Log activity if applicable

Example:
set bp_readings to get value from TinyDB tag "vital_readings_bp"
set bp_readings to insert list item new_record at 1 of bp_readings
if length of bp_readings > 100:
  set bp_readings to sublist of bp_readings from 1 to 100
store bp_readings in TinyDB tag "vital_readings_bp"
```

---

## 🎯 **Implementation Priority**

### **Phase 1 (Tonight) - Essential Tags:**
- [ ] "recent_activities"
- [ ] "vital_readings_bp"
- [ ] "vital_readings_hr"
- [ ] "vital_readings_temp"
- [ ] "vital_readings_weight"
- [ ] "user_profile"
- [ ] "logged_in"

### **Phase 2 (Sunday) - Extended Tags:**
- [ ] "medications_list"
- [ ] "medication_history"
- [ ] "app_settings"
- [ ] "health_score_data"

### **Phase 3 (Later) - Advanced Tags:**
- [ ] "vital_readings_glucose"
- [ ] "vital_readings_steps"
- [ ] "app_usage_stats"
- [ ] "data_export_history"

This comprehensive TinyDB namespace definition ensures **consistent data storage** across all VitalSync screens and components! 📊✨
