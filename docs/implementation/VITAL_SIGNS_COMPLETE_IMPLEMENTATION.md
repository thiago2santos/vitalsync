# 🫀 VitalSync - Complete Vital Signs Implementation

## 🎯 **Complete Action Plan for All Vital Signs**

This document provides step-by-step procedures for implementing all vital signs with TinyDB storage and activity logging.

---

## 📊 **TinyDB Structure & Namespaces**

### **TinyDB Tags (Namespaces):**
```
"vital_readings_bp"        - Blood pressure readings
"vital_readings_hr"        - Heart rate readings  
"vital_readings_temp"      - Temperature readings
"vital_readings_weight"    - Weight readings
"vital_readings_glucose"   - Blood glucose readings
"vital_readings_steps"     - Daily steps (from Pedometer)
"recent_activities"        - Activity log for dashboard
"user_profile"            - User personal data
"app_settings"            - App preferences
```

### **Data Structure for Each Vital Type:**
```
Blood Pressure: [timestamp, systolic, diastolic, pulse, notes, status]
Heart Rate    : [timestamp, bpm, notes, status]
Temperature   : [timestamp, celsius, notes, status]
Weight        : [timestamp, kg, notes, status]
Glucose       : [timestamp, mg_dl, notes, status]
Steps         : [timestamp, steps, notes, status]
```

---

## 🩺 **1. BLOOD PRESSURE Implementation**

### **Procedure: SaveBloodPressure**
```blocks
Procedure: SaveBloodPressure
Parameters: systolic (number), diastolic (number), pulse (number), notes (text)

Actions:
1. Validate input data:
   - if systolic < 70 OR systolic > 250:
     * show notifier "Invalid systolic pressure (70-250)"
     * return false
   - if diastolic < 40 OR diastolic > 150:
     * show notifier "Invalid diastolic pressure (40-150)"
     * return false

2. Create timestamp:
   - set current_timestamp to format as text (clock now) pattern "yyyy-MM-dd HH:mm:ss"

3. Determine status based on values:
   - call DetermineBPStatus(systolic, diastolic)
   - set bp_status to result

4. Create blood pressure record:
   - set bp_record to make a list(
       current_timestamp,
       systolic,
       diastolic, 
       pulse,
       notes,
       bp_status
     )

5. Get existing BP readings:
   - set bp_readings to get value from TinyDB tag "vital_readings_bp"
   - if bp_readings is empty: set bp_readings to create empty list

6. Add new reading to beginning:
   - set bp_readings to insert list item bp_record at 1 of bp_readings

7. Keep only last 100 readings (storage management):
   - if length of bp_readings > 100:
     * set bp_readings to sublist of bp_readings from 1 to 100

8. Save to TinyDB:
   - store bp_readings in TinyDB tag "vital_readings_bp"

9. Log activity:
   - set activity_text to systolic + "/" + diastolic + " mmHg (" + bp_status + ")"
   - call LogActivity("vital_added", "Added blood pressure", activity_text)

10. Update health score:
    - call UpdateHealthScore

11. Show success message:
    - show notifier "Blood pressure saved successfully!"

12. Return success:
    - return true
```

### **Procedure: DetermineBPStatus**
```blocks
Procedure: DetermineBPStatus
Parameters: systolic (number), diastolic (number)
Returns: status (text)

Actions:
if systolic < 90 AND diastolic < 60:
  return "Low"
else if systolic <= 120 AND diastolic <= 80:
  return "Normal"
else if systolic <= 129 AND diastolic <= 80:
  return "Elevated"
else if systolic <= 139 OR diastolic <= 89:
  return "High Stage 1"
else if systolic <= 179 OR diastolic <= 119:
  return "High Stage 2"
else:
  return "Crisis - Seek Medical Attention"
```

### **Event Handler: Blood Pressure Save Button**
```blocks
When btn_save_bp.Click:
1. Get form values:
   - set systolic_val to txt_systolic.Text
   - set diastolic_val to txt_diastolic.Text
   - set pulse_val to txt_pulse.Text (if empty, use 0)
   - set notes_val to txt_bp_notes.Text

2. Validate required fields:
   - if systolic_val is empty OR diastolic_val is empty:
     * show notifier "Please enter both systolic and diastolic values"
     * return

3. Call save procedure:
   - set save_success to call SaveBloodPressure(systolic_val, diastolic_val, pulse_val, notes_val)

4. If successful:
   - if save_success:
     * clear all form fields
     * call ShowVitalComponent("main")
     * call RefreshVitalsList
```

---

## ❤️ **2. HEART RATE Implementation**

### **Procedure: SaveHeartRate**
```blocks
Procedure: SaveHeartRate
Parameters: bpm (number), notes (text)

Actions:
1. Validate input:
   - if bpm < 30 OR bpm > 220:
     * show notifier "Invalid heart rate (30-220 bpm)"
     * return false

2. Create timestamp:
   - set current_timestamp to format as text (clock now) pattern "yyyy-MM-dd HH:mm:ss"

3. Determine HR status:
   - call DetermineHRStatus(bpm)
   - set hr_status to result

4. Create heart rate record:
   - set hr_record to make a list(
       current_timestamp,
       bpm,
       notes,
       hr_status
     )

5. Get existing HR readings:
   - set hr_readings to get value from TinyDB tag "vital_readings_hr"
   - if hr_readings is empty: set hr_readings to create empty list

6. Add and manage storage:
   - set hr_readings to insert list item hr_record at 1 of hr_readings
   - if length of hr_readings > 100:
     * set hr_readings to sublist of hr_readings from 1 to 100

7. Save to TinyDB:
   - store hr_readings in TinyDB tag "vital_readings_hr"

8. Log activity:
   - set activity_text to bpm + " bpm (" + hr_status + ")"
   - call LogActivity("vital_added", "Added heart rate", activity_text)

9. Update health score and show success:
   - call UpdateHealthScore
   - show notifier "Heart rate saved successfully!"
   - return true
```

### **Procedure: DetermineHRStatus**
```blocks
Procedure: DetermineHRStatus
Parameters: bpm (number)
Returns: status (text)

Actions:
if bmp < 60:
  return "Bradycardia (Low)"
else if bpm >= 60 AND bpm <= 100:
  return "Normal"
else if bpm > 100 AND bpm <= 150:
  return "Tachycardia (High)"
else:
  return "Very High - Monitor Closely"
```

---

## 🌡️ **3. TEMPERATURE Implementation**

### **Procedure: SaveTemperature**
```blocks
Procedure: SaveTemperature
Parameters: celsius (number), notes (text)

Actions:
1. Validate temperature:
   - if celsius < 30 OR celsius > 45:
     * show notifier "Invalid temperature (30-45°C)"
     * return false

2. Create timestamp:
   - set current_timestamp to format as text (clock now) pattern "yyyy-MM-dd HH:mm:ss"

3. Determine temperature status:
   - call DetermineTempStatus(celsius)
   - set temp_status to result

4. Create temperature record:
   - set temp_record to make a list(
       current_timestamp,
       celsius,
       notes,
       temp_status
     )

5. Manage TinyDB storage:
   - set temp_readings to get value from TinyDB tag "vital_readings_temp"
   - if temp_readings is empty: set temp_readings to create empty list
   - set temp_readings to insert list item temp_record at 1 of temp_readings
   - if length of temp_readings > 100:
     * set temp_readings to sublist of temp_readings from 1 to 100

7. Save and log:
   - store temp_readings in TinyDB tag "vital_readings_temp"
   - set activity_text to celsius + "°C (" + temp_status + ")"
   - call LogActivity("vital_added", "Added temperature", activity_text)
   - call UpdateHealthScore
   - show notifier "Temperature saved successfully!"
   - return true
```

### **Procedure: DetermineTempStatus**
```blocks
Procedure: DetermineTempStatus
Parameters: celsius (number)
Returns: status (text)

Actions:
if celsius < 36.1:
  return "Low (Hypothermia Risk)"
else if celsius >= 36.1 AND celsius <= 37.2:
  return "Normal"
else if celsius > 37.2 AND celsius <= 38.0:
  return "Slightly Elevated"
else if celsius > 38.0 AND celsius <= 39.0:
  return "Fever"
else:
  return "High Fever - Seek Medical Care"
```

---

## ⚖️ **4. WEIGHT Implementation**

### **Procedure: SaveWeight**
```blocks
Procedure: SaveWeight
Parameters: kg (number), notes (text)

Actions:
1. Validate weight:
   - if kg < 20 OR kg > 300:
     * show notifier "Invalid weight (20-300 kg)"
     * return false

2. Calculate additional metrics:
   - set pounds to kg * 2.20462
   - set user_height to get value from TinyDB tag "user_height" (default 170 cm)
   - set bmi to kg / ((user_height/100) * (user_height/100))

3. Create timestamp:
   - set current_timestamp to format as text (clock now) pattern "yyyy-MM-dd HH:mm:ss"

4. Determine weight status:
   - call DetermineWeightStatus(bmi)
   - set weight_status to result

5. Create weight record:
   - set weight_record to make a list(
       current_timestamp,
       kg,
       pounds,
       bmi,
       notes,
       weight_status
     )

6. Manage TinyDB storage:
   - set weight_readings to get value from TinyDB tag "vital_readings_weight"
   - if weight_readings is empty: set weight_readings to create empty list
   - set weight_readings to insert list item weight_record at 1 of weight_readings
   - if length of weight_readings > 100:
     * set weight_readings to sublist of weight_readings from 1 to 100

7. Save and log:
   - store weight_readings in TinyDB tag "vital_readings_weight"
   - set activity_text to kg + " kg (BMI: " + round(bmi, 1) + ")"
   - call LogActivity("vital_added", "Added weight", activity_text)
   - call UpdateHealthScore
   - show notifier "Weight saved successfully!"
   - return true
```

### **Procedure: DetermineWeightStatus**
```blocks
Procedure: DetermineWeightStatus
Parameters: bmi (number)
Returns: status (text)

Actions:
if bmi < 18.5:
  return "Underweight"
else if bmi >= 18.5 AND bmi < 25:
  return "Normal"
else if bmi >= 25 AND bmi < 30:
  return "Overweight"
else:
  return "Obese"
```

---

## 🩸 **5. BLOOD GLUCOSE Implementation**

### **Procedure: SaveGlucose**
```blocks
Procedure: SaveGlucose
Parameters: mg_dl (number), meal_context (text), notes (text)

Actions:
1. Validate glucose:
   - if mg_dl < 40 OR mg_dl > 600:
     * show notifier "Invalid glucose level (40-600 mg/dL)"
     * return false

2. Convert to mmol/L:
   - set mmol_l to mg_dl / 18.0

3. Create timestamp:
   - set current_timestamp to format as text (clock now) pattern "yyyy-MM-dd HH:mm:ss"

4. Determine glucose status:
   - call DetermineGlucoseStatus(mg_dl, meal_context)
   - set glucose_status to result

5. Create glucose record:
   - set glucose_record to make a list(
       current_timestamp,
       mg_dl,
       mmol_l,
       meal_context,
       notes,
       glucose_status
     )

6. Manage TinyDB storage:
   - set glucose_readings to get value from TinyDB tag "vital_readings_glucose"
   - if glucose_readings is empty: set glucose_readings to create empty list
   - set glucose_readings to insert list item glucose_record at 1 of glucose_readings
   - if length of glucose_readings > 100:
     * set glucose_readings to sublist of glucose_readings from 1 to 100

7. Save and log:
   - store glucose_readings in TinyDB tag "vital_readings_glucose"
   - set activity_text to mg_dl + " mg/dL (" + meal_context + " - " + glucose_status + ")"
   - call LogActivity("vital_added", "Added glucose reading", activity_text)
   - call UpdateHealthScore
   - show notifier "Glucose level saved successfully!"
   - return true
```

### **Procedure: DetermineGlucoseStatus**
```blocks
Procedure: DetermineGlucoseStatus
Parameters: mg_dl (number), meal_context (text)
Returns: status (text)

Actions:
if meal_context = "Fasting":
  if mg_dl < 70:
    return "Low (Hypoglycemia)"
  else if mg_dl >= 70 AND mg_dl <= 100:
    return "Normal"
  else if mg_dl > 100 AND mg_dl <= 125:
    return "Pre-diabetic"
  else:
    return "Diabetic Range"

else if meal_context = "After Meal":
  if mg_dl < 70:
    return "Low"
  else if mg_dl >= 70 AND mg_dl <= 140:
    return "Normal"
  else if mg_dl > 140 AND mg_dl <= 199:
    return "Pre-diabetic"
  else:
    return "Diabetic Range"

else: # Random/Other
  if mg_dl < 70:
    return "Low"
  else if mg_dl >= 70 AND mg_dl <= 200:
    return "Normal Range"
  else:
    return "High - Monitor"
```

---

## 👟 **6. STEPS (Pedometer) Implementation**

### **Procedure: SaveDailySteps**
```blocks
Procedure: SaveDailySteps
Parameters: steps (number), distance_km (number), calories (number)

Actions:
1. Get current date:
   - set current_date to format as text (clock now) pattern "yyyy-MM-dd"

2. Calculate additional metrics:
   - set active_minutes to steps / 100 (rough estimate)
   - if distance_km = 0: set distance_km to steps * 0.0008 (average step length)
   - if calories = 0: set calories to steps * 0.04 (rough estimate)

3. Create steps record:
   - set steps_record to make a list(
       current_date,
       steps,
       distance_km,
       calories,
       active_minutes
     )

4. Manage daily steps storage:
   - set daily_steps to get value from TinyDB tag "vital_readings_steps"
   - if daily_steps is empty: set daily_steps to create empty list

5. Check if today's record exists:
   - set today_exists to false
   - set record_index to 0
   - for each record in daily_steps:
     * set record_index to record_index + 1
     * if select list item 1 of record = current_date:
       - set today_exists to true
       - break

6. Update or add record:
   - if today_exists:
     * replace list item record_index of daily_steps with steps_record
   - else:
     * set daily_steps to insert list item steps_record at 1 of daily_steps

7. Keep only last 365 days:
   - if length of daily_steps > 365:
     * set daily_steps to sublist of daily_steps from 1 to 365

8. Save and log:
   - store daily_steps in TinyDB tag "vital_readings_steps"
   - set activity_text to steps + " steps (" + round(distance_km, 2) + " km)"
   - call LogActivity("vital_added", "Updated daily steps", activity_text)
   - return true
```

---

## 🔄 **Global Support Procedures**

### **Procedure: LogActivity**
```blocks
Procedure: LogActivity
Parameters: activity_type (text), description (text), details (text)

Actions:
1. Get current activities:
   - set activities to get value from TinyDB tag "recent_activities"
   - if activities is empty: set activities to create empty list

2. Create activity record:
   - set timestamp to format as text (clock now) pattern "MM-dd HH:mm"
   - set activity_record to make a list(timestamp, activity_type, description, details)

3. Add to activities:
   - set activities to insert list item activity_record at 1 of activities
   - if length of activities > 20: set activities to sublist of activities from 1 to 20

4. Save to TinyDB:
   - store activities in TinyDB tag "recent_activities"
```

### **Procedure: CalculateBPScore**
```blocks
Procedure: CalculateBPScore
Returns: score (number, 0-25 points)

Actions:
1. Get latest BP reading:
   - set bp_readings to get value from TinyDB tag "vital_readings_bp"
   - if bp_readings is empty OR length of bp_readings = 0:
     * return 15 (neutral score for no data)

2. Get most recent reading:
   - set latest_bp to select list item 1 of bp_readings
   - set bp_status to select list item 6 of latest_bp

3. Score based on status:
   - if bp_status = "Normal":
     * return 25
   - else if bp_status = "Elevated":
     * return 20
   - else if bp_status = "High Stage 1":
     * return 15
   - else if bp_status = "High Stage 2":
     * return 10
   - else if bp_status = "Low":
     * return 12
   - else: # Crisis
     * return 5
```

### **Procedure: CalculateHRScore**
```blocks
Procedure: CalculateHRScore
Returns: score (number, 0-25 points)

Actions:
1. Get latest HR reading:
   - set hr_readings to get value from TinyDB tag "vital_readings_hr"
   - if hr_readings is empty OR length of hr_readings = 0:
     * return 15 (neutral score for no data)

2. Get most recent reading:
   - set latest_hr to select list item 1 of hr_readings
   - set hr_status to select list item 5 of latest_hr

3. Score based on status:
   - if hr_status = "Normal":
     * return 25
   - else if hr_status = "Bradycardia (Low)":
     * return 18
   - else if hr_status = "Tachycardia (High)":
     * return 15
   - else: # Very High
     * return 8
```

### **Procedure: CalculateWeightScore**
```blocks
Procedure: CalculateWeightScore
Returns: score (number, 0-25 points)

Actions:
1. Get latest weight reading:
   - set weight_readings to get value from TinyDB tag "vital_readings_weight"
   - if weight_readings is empty OR length of weight_readings = 0:
     * return 15 (neutral score for no data)

2. Get most recent reading:
   - set latest_weight to select list item 1 of weight_readings
   - set weight_status to select list item 6 of latest_weight

3. Score based on BMI status:
   - if weight_status = "Normal":
     * return 25
   - else if weight_status = "Overweight":
     * return 18
   - else if weight_status = "Underweight":
     * return 15
   - else: # Obese
     * return 10
```

### **Procedure: CalculateActivityScore**
```blocks
Procedure: CalculateActivityScore
Returns: score (number, 0-25 points)

Actions:
1. Get today's steps:
   - set current_date to format as text (clock now) pattern "yyyy-MM-dd"
   - set daily_steps to get value from TinyDB tag "vital_readings_steps"
   - set today_steps to 0

2. Find today's step count:
   - if daily_steps is not empty:
     * for each record in daily_steps:
       - if select list item 1 of record = current_date:
         * set today_steps to select list item 2 of record
         * break

3. Score based on step count (WHO recommendations):
   - if today_steps >= 10000:
     * return 25 (excellent)
   - else if today_steps >= 7500:
     * return 22 (very good)
   - else if today_steps >= 5000:
     * return 18 (good)
   - else if today_steps >= 2500:
     * return 12 (fair)
   - else if today_steps > 0:
     * return 8 (poor)
   - else:
     * return 5 (no activity recorded)
```

### **Procedure: UpdateHealthScore**
```blocks
Procedure: UpdateHealthScore
Actions:
1. Calculate individual component scores:
   - set bp_score to call CalculateBPScore
   - set hr_score to call CalculateHRScore
   - set weight_score to call CalculateWeightScore
   - set activity_score to call CalculateActivityScore

2. Calculate total health score:
   - set total_score to bp_score + hr_score + weight_score + activity_score

3. Determine health grade:
   - if total_score >= 90:
     * set health_grade to "Excellent"
   - else if total_score >= 75:
     * set health_grade to "Good"
   - else if total_score >= 60:
     * set health_grade to "Fair"
   - else if total_score >= 45:
     * set health_grade to "Poor"
   - else:
     * set health_grade to "Needs Attention"

4. Store health score data:
   - set score_data to make a list(total_score, health_grade, bp_score, hr_score, weight_score, activity_score)
   - store score_data in TinyDB tag "current_health_score"
   - return total_score
```

---

## 🏆 **Health Scoring System Explained**

### **Scoring Logic (Total: 0-100 points)**

The health score is calculated from four equally weighted components:

#### **1. Blood Pressure Score (0-25 points)**
- **Normal (120/80 or below)**: 25 points
- **Elevated (121-129 systolic)**: 20 points  
- **High Stage 1 (130-139/80-89)**: 15 points
- **High Stage 2 (140-179/90-119)**: 10 points
- **Low (under 90/60)**: 12 points
- **Crisis (180+/120+)**: 5 points
- **No data**: 15 points (neutral)

#### **2. Heart Rate Score (0-25 points)**
- **Normal (60-100 bpm)**: 25 points
- **Bradycardia (under 60)**: 18 points
- **Tachycardia (101-150)**: 15 points
- **Very High (over 150)**: 8 points
- **No data**: 15 points (neutral)

#### **3. Weight/BMI Score (0-25 points)**
- **Normal BMI (18.5-24.9)**: 25 points
- **Overweight (25-29.9)**: 18 points
- **Underweight (under 18.5)**: 15 points
- **Obese (30+)**: 10 points
- **No data**: 15 points (neutral)

#### **4. Activity Score (0-25 points)**
- **10,000+ steps**: 25 points (excellent)
- **7,500-9,999 steps**: 22 points (very good)
- **5,000-7,499 steps**: 18 points (good)
- **2,500-4,999 steps**: 12 points (fair)
- **1-2,499 steps**: 8 points (poor)
- **0 steps**: 5 points (no activity)

### **Health Grade Interpretation**
- **90-100 points**: Excellent Health 🟢
- **75-89 points**: Good Health 🟡
- **60-74 points**: Fair Health 🟠
- **45-59 points**: Poor Health 🔴
- **0-44 points**: Needs Attention ⚠️

---

## 📋 **Implementation Timeline (Tonight)**

### **Hour 1: Core Procedures (60 minutes)**
- [ ] Create LogActivity procedure (15 min)
- [ ] Create SaveBloodPressure + DetermineBPStatus (20 min)
- [ ] Create SaveHeartRate + DetermineHRStatus (15 min)
- [ ] Test BP and HR procedures (10 min)

### **Hour 2: Additional Vitals (60 minutes)**
- [ ] Create SaveTemperature + DetermineTempStatus (20 min)
- [ ] Create SaveWeight + DetermineWeightStatus (20 min)
- [ ] Create SaveGlucose + DetermineGlucoseStatus (20 min)

### **Hour 3: Health Scoring System (60 minutes)**
- [ ] Create CalculateBPScore procedure (10 min)
- [ ] Create CalculateHRScore procedure (10 min)
- [ ] Create CalculateWeightScore procedure (10 min)
- [ ] Create CalculateActivityScore procedure (15 min)
- [ ] Create UpdateHealthScore procedure (15 min)

### **Hour 4: Integration & Testing (60 minutes)**
- [ ] Connect all procedures to UI buttons (20 min)
- [ ] Test complete vital signs flow (20 min)
- [ ] Test health scoring system (20 min)

### **Success Criteria:**
- ✅ All 5 vital types can be saved with validation
- ✅ Data persists in TinyDB with proper structure
- ✅ Activities appear in dashboard
- ✅ Health score updates automatically
- ✅ Status determination works for all vitals

This gives you a **complete, production-ready vital signs system** with proper data management, validation, and user feedback! 🚀
