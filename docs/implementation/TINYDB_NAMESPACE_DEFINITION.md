# 📊 VitalSync - Complete TinyDB Data Structure

## 🎯 **Overview**

This document defines **all TinyDB tags and data structures** used across VitalSync. This is the **single source of truth** for data storage.

## 🔧 **TinyDB Component Setup**

### **Namespace vs Tags (Important Distinction):**
```
TinyDB Component:
├── Namespace: "VitalSync" (set once - isolates app data)
└── Tags: Individual data storage keys within the namespace
    ├── "vital_readings_bp" → Blood pressure data
    ├── "vital_readings_hr" → Heart rate data
    ├── "recent_activities" → Activity log
    └── etc.
```

### **Correct Implementation:**
```blocks
TinyDB Component Properties:
├── Name: db_vitalsync (or TinyDB1)
├── Namespace: "VitalSync" 
└── Usage: store/get value using tags
```

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
Structure: [timestamp, bpm, notes, status]
Example: [
  ["2024-09-21 18:30:00", 72, "At rest", "Normal"],
  ["2024-09-21 16:45:00", 145, "After workout", "Tachycardia (High)"]
]
Usage: SaveHeartRate(), LoadHRReadings()
Max Records: 100 (auto-cleanup)
```

#### **3. "vital_readings_temp" - Temperature Readings**
```
Data Type: List of Lists
Structure: [timestamp, celsius, notes, status]
Example: [
  ["2024-09-21 18:30:00", 36.5, "Normal check", "Normal"],
  ["2024-09-21 12:15:00", 37.8, "Feeling unwell", "Fever"]
]
Usage: SaveTemperature(), LoadTempReadings()
Max Records: 100 (auto-cleanup)
```

#### **4. "vital_readings_weight" - Weight Readings**
```
Data Type: List of Lists
Structure: [timestamp, kg, notes, status]
Example: [
  ["2024-09-21 08:00:00", 70.5, "Morning weight", "Normal"],
  ["2024-09-20 08:00:00", 71.0, "After weekend", "Normal"]
]
Usage: SaveWeight(), LoadWeightReadings()
Max Records: 100 (auto-cleanup)
```

#### **5. "vital_readings_glucose" - Blood Glucose Readings**
```
Data Type: List of Lists
Structure: [timestamp, mg_dl, notes, status]
Example: [
  ["2024-09-21 08:00:00", 95, "Fasting glucose", "Normal"],
  ["2024-09-21 20:30:00", 140, "2h post dinner", "Elevated"]
]
Usage: SaveGlucose(), LoadGlucoseReadings()
Max Records: 100 (auto-cleanup)
```

#### **6. "vital_readings_steps" - Daily Steps Data**
```
Data Type: List of Lists
Structure: [timestamp, steps, notes, status]
Example: [
  ["2024-09-21 23:59:00", 8500, "Good walking day", "Good"],
  ["2024-09-20 23:59:00", 12000, "Very active day", "Excellent"]
]
Usage: SaveSteps(), LoadStepsReadings()
Max Records: 100 (auto-cleanup)
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
Structure: [email, full_name, birth_date, gender, height_cm, blood_type, allergies]
Example: ["joao@email.com", "João Silva", "1990-05-15", "Masculino", 175, "O+", "Penicilina"]
Usage: Profile management, BMI calculations, medical safety
```

#### **12. "user_credentials" - Authentication Data**
```
Data Type: List
Structure: [email, password_hash]
Example: ["joao@email.com", "hashed_password_string"]
Usage: Login authentication, security
```

#### **13. "user_session" - Session Management**
```
Data Type: List
Structure: [is_logged_in, expiry_date, remember_me]
Example: [true, "2024-10-26", true]
Usage: Simple session with expiry date
Logic: 
  - remember_me = true: 30 days
  - remember_me = false: 1 day
  - Check: current_date > expiry_date → logout
```

#### **14. "password_recovery" - Password Recovery Data**
```
Data Type: List
Structure: [recovery_code, expiry_timestamp, email, is_active]
Example: ["123456", "2024-09-26 15:30:00", "joao@email.com", true]
Usage: Temporary password recovery via email
Logic:
  - Code expires in 15 minutes
  - Only one active recovery per time
  - Code is 6-digit random number
```


---

### **⚙️ Medical Validation Configuration Tags**

#### **15. "hr_validation_ranges" - Heart Rate Validation Ranges by Age**
```
Data Type: List of Lists
Structure: [age_group, min_age, max_age, min_bpm, max_bpm, group_name]
Example: [
  ["baby", 0, 2, 120, 160, "Bebê"],
  ["toddler", 3, 7, 95, 140, "Criança Pequena"],
  ["child", 8, 17, 75, 115, "Criança/Adolescente"],
  ["adult", 18, 65, 60, 100, "Adulto"],
  ["elderly", 66, 120, 60, 100, "Idoso"]
]
Usage: DetermineHRStatus(), age-based validation
Max Records: 10 age groups
```

#### **16. "bp_validation_ranges" - Blood Pressure Validation Ranges**
```
Data Type: List of Lists
Structure: [category, min_systolic, max_systolic, min_diastolic, max_diastolic, status_name]
Example: [
  ["normal", 90, 120, 60, 80, "Normal"],
  ["elevated", 121, 129, 60, 80, "Elevada"],
  ["high_stage1", 130, 139, 80, 89, "Alta Estágio 1"],
  ["high_stage2", 140, 179, 90, 119, "Alta Estágio 2"],
  ["crisis", 180, 250, 120, 150, "Crise Hipertensiva"]
]
Usage: DetermineBPStatus(), blood pressure classification
Max Records: 10 categories
```

#### **17. "temp_validation_ranges" - Temperature Validation Ranges**
```
Data Type: List of Lists
Structure: [category, min_celsius, max_celsius, status_name]
Example: [
  ["hypothermia", 30.0, 35.0, "Hipotermia"],
  ["normal", 35.1, 37.2, "Normal"],
  ["fever_low", 37.3, 38.0, "Febre Baixa"],
  ["fever_high", 38.1, 40.0, "Febre Alta"],
  ["hyperthermia", 40.1, 45.0, "Hipertermia"]
]
Usage: DetermineTempStatus(), temperature classification
Max Records: 10 categories
```

---

### **💊 Medication Management Tags**

#### **18. "medications_list" - Active Medications**
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

#### **19. "medication_history" - Medication Taking History**
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

---

## 🛠️ **Practical Usage Examples**

### **Correct Block Syntax:**
```blocks
✅ Store Data:
store activities in TinyDB1 tag "recent_activities"
store bp_readings in TinyDB1 tag "vital_readings_bp"

✅ Get Data:
set activities to get value from TinyDB1 tag "recent_activities"
set bp_readings to get value from TinyDB1 tag "vital_readings_bp"
```

### **Key Points:**
- **One TinyDB component** per app (db_vitalsync)
- **Namespace set once** to "VitalSync" (isolates app data)
- **Multiple tags** for different data types
- **Tags are storage keys** - not namespaces!

This comprehensive TinyDB structure ensures **consistent data storage** across all VitalSync screens and components! 📊✨
