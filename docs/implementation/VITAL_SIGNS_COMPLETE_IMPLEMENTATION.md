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

3. Get user age and determine HR status:
   - set user_profile to get value from TinyDB tag "user_profile"
   - set user_age to CalculateAge(user_profile)
   - call DetermineHRStatus(bpm, user_age)
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
Parameters: bpm (number), user_age (number)
Returns: status (text)

Actions:
1. Get heart rate validation ranges from TinyDB:
   - set hr_ranges to get value from TinyDB tag "hr_validation_ranges"
   - if hr_ranges is empty: call InitializeHRRanges

2. Find appropriate age group:
   - set found_range to false
   - for each range in hr_ranges:
     * set min_age to select list item 2 of range
     * set max_age to select list item 3 of range
     * if user_age >= min_age AND user_age <= max_age:
       - set min_normal to select list item 4 of range
       - set max_normal to select list item 5 of range
       - set age_group to select list item 6 of range
       - set found_range to true
       - break loop

3. Use default adult range if no match found:
   - if NOT found_range:
     * set min_normal to 60
     * set max_normal to 100
     * set age_group to "Adulto"

4. Evaluate heart rate based on ranges:
   - if bpm < min_normal:
     return "Baixo para " + age_group
   - else if bpm >= min_normal AND bpm <= max_normal:
     return "Normal para " + age_group
   - else if bpm > max_normal AND bpm <= (max_normal + 50):
     return "Alto para " + age_group
   - else:
     return "Muito Alto - Consulte Médico"
```

### **Procedure: CalculateAge**
```blocks
Procedure: CalculateAge
Parameters: user_profile (list)
Returns: age (number)

Actions:
1. Get birth date from profile:
   - if user_profile is empty OR length of user_profile < 3:
     * return 30 (default adult age)
   - set birth_date to select list item 3 of user_profile (birth_date field)

2. Calculate age:
   - set current_year to format as text (clock now) pattern "yyyy"
   - set birth_year to substring of birth_date from 1 to 4
   - set age to current_year - birth_year
   - return age
```

### **Procedure: InitializeHRRanges**
```blocks
Procedure: InitializeHRRanges
Parameters: none
Returns: none

Actions:
1. Create default heart rate ranges:
   - set hr_ranges to make a list(
       make a list("baby", 0, 2, 120, 160, "Bebê"),
       make a list("toddler", 3, 7, 95, 140, "Criança Pequena"),
       make a list("child", 8, 17, 75, 115, "Criança/Adolescente"),
       make a list("adult", 18, 65, 60, 100, "Adulto"),
       make a list("elderly", 66, 120, 60, 100, "Idoso")
     )

2. Save to TinyDB:
   - store hr_ranges in TinyDB tag "hr_validation_ranges"
```

### **Exemplos de Validação por Idade (Valores Atualizados):**
```
Bebê (1 ano): 140 bpm → "Normal para Bebê"
Criança Pequena (5 anos): 115 bpm → "Normal para Criança Pequena"
Criança (10 anos): 90 bpm → "Normal para Criança/Adolescente"  
Adulto (30 anos): 75 bpm → "Normal para Adulto"
Idoso (70 anos): 80 bpm → "Normal para Idoso"

Bebê (2 anos): 110 bpm → "Baixo para Bebê"
Criança Pequena (4 anos): 85 bpm → "Baixo para Criança Pequena"
Adulto (30 anos): 45 bpm → "Baixo para Adulto"
Criança (12 anos): 125 bpm → "Alto para Criança/Adolescente"
```

### **📊 Faixas Etárias Definidas (Baseadas em Referências Médicas Internacionais):**
- **Bebês (0-2 anos)**: 120-160 bpm - Frequência alta devido ao desenvolvimento cardíaco
- **Crianças Pequenas (3-7 anos)**: 95-140 bpm - Transição gradual, alta atividade física
- **Crianças/Adolescentes (8-17 anos)**: 75-115 bpm - Aproximação dos valores adultos
- **Adultos (18-65 anos)**: 60-100 bpm - Faixa padrão AHA (American Heart Association)
- **Idosos (>65 anos)**: 60-100 bpm - Mantém faixa adulta, variações individuais

### **🏥 Justificativas Médicas dos Ajustes:**
- **Bebês**: Ampliado para 120-160 bpm (era 120-140) - Recém-nascidos podem ter FC até 180 bpm
- **Crianças Pequenas**: Ajustado para 95-140 bpm (era 100-120) - Maior variabilidade por atividade física
- **Crianças/Adolescentes**: Expandido para 75-115 bpm (era 80-100) - Melhor cobertura da variação natural
- **Idosos**: Corrigido para 60-100 bpm (era 50-60) - Idosos saudáveis mantêm faixa adulta normal

### **✅ Vantagens da Validação Configurável:**
- **Flexibilidade**: Ranges podem ser ajustados sem alterar código
- **Personalização**: Médicos podem configurar faixas específicas
- **Atualizações**: Novos padrões médicos podem ser facilmente implementados
- **Manutenibilidade**: Lógica de validação separada dos valores
- **Extensibilidade**: Fácil adicionar novas faixas etárias

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

2. Create timestamp:
   - set current_timestamp to format as text (clock now) pattern "yyyy-MM-dd HH:mm:ss"

3. Determine weight status:
   - call DetermineWeightStatus(kg)
   - set weight_status to result

4. Create weight record:
   - set weight_record to make a list(
       current_timestamp,
       kg,
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
   - set activity_text to kg + " kg (" + weight_status + ")"
   - call LogActivity("vital_added", "Added weight", activity_text)
   - call UpdateHealthScore
   - show notifier "Weight saved successfully!"
   - return true
```

### **Procedure: DetermineWeightStatus**
```blocks
Procedure: DetermineWeightStatus
Parameters: kg (number)
Returns: status (text)

Actions:
if kg < 50:
  return "Baixo Peso"
else if kg >= 50 AND kg <= 90:
  return "Peso Normal"
else if kg > 90 AND kg <= 120:
  return "Sobrepeso"
else:
  return "Peso Alto"
```

---

## 🩸 **5. BLOOD GLUCOSE Implementation**

### **Procedure: SaveGlucose**
```blocks
Procedure: SaveGlucose
Parameters: mg_dl (number), notes (text)

Actions:
1. Validate glucose:
   - if mg_dl < 40 OR mg_dl > 600:
     * show notifier "Invalid glucose level (40-600 mg/dL)"
     * return false

2. Create timestamp:
   - set current_timestamp to format as text (clock now) pattern "yyyy-MM-dd HH:mm:ss"

3. Determine glucose status:
   - call DetermineGlucoseStatus(mg_dl)
   - set glucose_status to result

4. Create glucose record:
   - set glucose_record to make a list(
       current_timestamp,
       mg_dl,
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
   - set activity_text to mg_dl + " mg/dL (" + glucose_status + ")"
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

### **Procedure: SaveSteps**
```blocks
Procedure: SaveSteps
Parameters: steps (number), notes (text)

Actions:
1. Validate steps:
   - if steps < 0 OR steps > 50000:
     * show notifier "Invalid steps count (0-50000)"
     * return false

2. Create timestamp:
   - set current_timestamp to format as text (clock now) pattern "yyyy-MM-dd HH:mm:ss"

3. Determine steps status:
   - call DetermineStepsStatus(steps)
   - set steps_status to result

4. Create steps record:
   - set steps_record to make a list(
       current_timestamp,
       steps,
       notes,
       steps_status
     )

5. Get existing steps readings:
   - set steps_readings to get value from TinyDB tag "vital_readings_steps"
   - if steps_readings is empty: set steps_readings to create empty list

6. Add and manage storage:
   - set steps_readings to insert list item steps_record at 1 of steps_readings
   - if length of steps_readings > 100:
     * set steps_readings to sublist of steps_readings from 1 to 100

7. Save to TinyDB:
   - store steps_readings in TinyDB tag "vital_readings_steps"

8. Log activity:
   - set activity_text to steps + " steps (" + steps_status + ")"
   - call LogActivity("vital_added", "Added steps", activity_text)

9. Update health score and show success:
   - call UpdateHealthScore
   - show notifier "Steps saved successfully!"
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

---

## 🔐 **6. SESSION MANAGEMENT Implementation**

### **Procedure: Login**
```blocks
Procedure: Login
Parameters: email (text), password (text), remember_me (boolean)
Returns: success (boolean)

Actions:
1. Validate credentials:
   - set stored_credentials to get value from TinyDB tag "user_credentials"
   - if stored_credentials is empty: return false
   - set stored_email to select list item 1 of stored_credentials
   - set stored_password to select list item 2 of stored_credentials
   - if email ≠ stored_email OR password ≠ stored_password: return false

2. Calculate expiry date:
   - set current_date to format as text (clock now) pattern "yyyy-MM-dd"
   - if remember_me = true:
     * set expiry_date to add days to current_date: 30
   - else:
     * set expiry_date to add days to current_date: 1

3. Create session:
   - set session_data to make a list(true, expiry_date, remember_me)
   - store session_data in TinyDB tag "user_session"
   - return true
```

### **Procedure: CreateUserProfile**
```blocks
Procedure: CreateUserProfile
Parameters: email (text), full_name (text), birth_date (text), gender (text), password (text)
Returns: success (boolean)

Actions:
1. Create user profile:
   - set user_profile to make a list(
       email,           // email
       full_name,       // full_name
       birth_date,      // birth_date (yyyy-MM-dd)
       gender,          // gender
       0,               // height_cm (to be filled later)
       "",              // blood_type (to be filled later)
       ""               // allergies (to be filled later)
     )

2. Create credentials:
   - set user_credentials to make a list(email, password)

3. Save to TinyDB:
   - store user_profile in TinyDB tag "user_profile"
   - store user_credentials in TinyDB tag "user_credentials"
   - return true
```

### **Procedure: CheckSession**
```blocks
Procedure: CheckSession
Parameters: none
Returns: is_valid (boolean)

Actions:
1. Get session:
   - set session_data to get value from TinyDB tag "user_session"
   - if session_data is empty: return false

2. Check expiry:
   - set is_logged_in to select list item 1 of session_data
   - set expiry_date to select list item 2 of session_data
   - set current_date to format as text (clock now) pattern "yyyy-MM-dd"
   
3. Validate:
   - if NOT is_logged_in OR current_date > expiry_date:
     * store make a list(false, "", false) in TinyDB tag "user_session"
     * return false
   - else:
     * return true
```

---
