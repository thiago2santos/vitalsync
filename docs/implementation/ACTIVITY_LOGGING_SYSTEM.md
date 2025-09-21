# 📊 VitalSync - Activity Logging System

## 🎯 **Recent Activities Dashboard Feature**

The Recent Activities section shows users their latest interactions with the app, creating engagement and providing a quick overview of their health tracking behavior.

## 📋 **What Activities to Track**

### **High Priority Activities:**
1. **Vital Signs Added** - "Added blood pressure reading: 120/80"
2. **Medications Taken** - "Took Aspirin 100mg at 8:00 AM"
3. **Medications Added** - "Added new medication: Vitamin D"
4. **Symptoms Recorded** - "Recorded headache with pain level 3"
5. **Profile Updated** - "Updated personal information"

### **Medium Priority Activities:**
6. **Login Events** - "Logged in to VitalSync"
7. **Data Exported** - "Exported health data"
8. **Settings Changed** - "Updated notification preferences"
9. **Reports Viewed** - "Viewed monthly health report"

## 🛠️ **TinyDB Structure for Activities**

### **TinyDB Tag: "recent_activities"**
```
Value: List of activity records
Format: [timestamp, activity_type, description, details]

Example:
[
  ["2024-09-21 18:30", "vital_added", "Added blood pressure", "120/80 mmHg"],
  ["2024-09-21 17:45", "med_taken", "Took medication", "Aspirin 100mg"],
  ["2024-09-21 16:20", "med_added", "Added medication", "Vitamin D 1000IU"],
  ["2024-09-21 15:10", "symptom_recorded", "Recorded symptom", "Headache - Level 3"],
  ["2024-09-21 14:00", "login", "Logged in", "Welcome back!"]
]
```

## 🔄 **Activity Logging Procedures**

### **Core Logging Procedure:**
```blocks
Procedure: LogActivity
Parameters: 
├── activity_type (text): Type of activity
├── description (text): Human-readable description  
└── details (text): Additional details

Actions:
1. Get current activities from TinyDB:
   - set activities to get value from TinyDB tag "recent_activities"
   - if activities is empty, set activities to create empty list

2. Create new activity record:
   - set timestamp to format current date/time as "YYYY-MM-DD HH:MM"
   - set new_activity to make a list(timestamp, activity_type, description, details)

3. Add to beginning of list:
   - set activities to insert list item new_activity at 1 of activities

4. Keep only last 20 activities (prevent TinyDB from getting too large):
   - if length of activities > 20:
     * set activities to sublist of activities from 1 to 20

5. Save back to TinyDB:
   - store activities in TinyDB tag "recent_activities"
```

## 📱 **When to Call LogActivity**

### **In scr_vital_signs:**
```blocks
When btn_save_vital.Click:
  # After saving vital data successfully
  if vital_type = "Blood Pressure":
    call LogActivity("vital_added", "Added blood pressure", txt_systolic.Text + "/" + txt_diastolic.Text + " mmHg")
  
  if vital_type = "Heart Rate":
    call LogActivity("vital_added", "Added heart rate", txt_heart_rate.Text + " bpm")
  
  if vital_type = "Weight":
    call LogActivity("vital_added", "Added weight", txt_weight.Text + " kg")
  
  if vital_type = "Temperature":
    call LogActivity("vital_added", "Added temperature", txt_temperature.Text + "°C")
```

### **In scr_medications:**
```blocks
When btn_save_medication.Click:
  # After saving medication successfully
  call LogActivity("med_added", "Added medication", txt_med_name.Text + " " + txt_dosage.Text)

When btn_take_med.Click:
  # When user marks medication as taken
  call LogActivity("med_taken", "Took medication", selected_med_name + " at " + current_time)

When btn_skip_med.Click:
  # When user skips a medication
  call LogActivity("med_skipped", "Skipped medication", selected_med_name + " at " + current_time)
```

### **In Screen1 (Authentication):**
```blocks
When login_successful:
  # After successful login
  call LogActivity("login", "Logged in", "Welcome back to VitalSync!")

When registration_successful:
  # After successful registration
  call LogActivity("registration", "Account created", "Welcome to VitalSync!")
```

### **In scr_profile:**
```blocks
When btn_save_profile.Click:
  # After updating profile
  call LogActivity("profile_updated", "Updated profile", "Personal information updated")

When btn_export_data.Click:
  # After exporting data
  call LogActivity("data_exported", "Exported data", "Health data exported successfully")
```

## 📊 **Displaying Activities in Dashboard**

### **Load Activities Procedure:**
```blocks
Procedure: LoadRecentActivities
Actions:
1. Get activities from TinyDB:
   - set activities to get value from TinyDB tag "recent_activities"
   - if activities is empty, set activities to create empty list

2. Clear current display:
   - set lst_recent_activities.Elements to create empty list

3. Format activities for display:
   - set display_list to create empty list
   - for each activity in activities:
     * set timestamp to select list item 1 of activity
     * set description to select list item 3 of activity
     * set details to select list item 4 of activity
     * set formatted_text to timestamp + " - " + description + ": " + details
     * add formatted_text to display_list

4. Update ListView:
   - set lst_recent_activities.Elements to display_list
```

### **In scr_dashboard When Screen.Initialize:**
```blocks
When scr_dashboard.Initialize:
  # Load and display recent activities
  call LoadRecentActivities
  
  # Also refresh activities when returning from other screens
  call RefreshHealthScore
  call UpdateQuickStats
```

## 🎨 **Activity Display Formatting**

### **Activity Icons and Colors:**
```blocks
Procedure: FormatActivityDisplay
Parameter: activity_type (text)
Returns: formatted_string (text)

Actions:
if activity_type = "vital_added":
  return "❤️ " + description
if activity_type = "med_taken":
  return "💊 " + description  
if activity_type = "med_added":
  return "➕ " + description
if activity_type = "symptom_recorded":
  return "🌡️ " + description
if activity_type = "login":
  return "🔐 " + description
else:
  return "📝 " + description
```

### **Enhanced Display with Icons:**
```blocks
In LoadRecentActivities procedure, modify formatting:
- set icon to call FormatActivityDisplay(activity_type)
- set formatted_text to icon + " " + description + " (" + time_only + ")"
```

## ⏰ **Time Formatting Helper**

### **Format Timestamp Procedure:**
```blocks
Procedure: FormatTimestamp
Parameter: timestamp (text)
Returns: formatted_time (text)

Actions:
1. Extract time from timestamp:
   - if timestamp contains current date:
     * return "Today at " + time_part
   - else if timestamp contains yesterday's date:
     * return "Yesterday at " + time_part
   - else:
     * return date_part + " at " + time_part

Example outputs:
- "Today at 18:30"
- "Yesterday at 14:20"  
- "Sep 19 at 09:15"
```

## 🔄 **Activity Management**

### **Clear Old Activities:**
```blocks
Procedure: ClearOldActivities
Actions:
1. Get current activities
2. Keep only activities from last 7 days
3. Save filtered list back to TinyDB

Call this procedure:
- When app starts (in Screen1.Initialize)
- Once per day (check last cleanup date)
```

### **Activity Statistics:**
```blocks
Procedure: GetActivityStats
Returns: stats_text (text)

Actions:
1. Count activities by type in last 7 days
2. Return summary like:
   "This week: 5 vital readings, 12 medications taken, 2 symptoms recorded"
```

## 📋 **Implementation Checklist**

### **Step 1: Create Core Procedures (30 minutes)**
- [ ] Add LogActivity procedure
- [ ] Add LoadRecentActivities procedure
- [ ] Add FormatTimestamp procedure

### **Step 2: Add Activity Logging (45 minutes)**
- [ ] Add to vital signs save events
- [ ] Add to medication events
- [ ] Add to login/registration events
- [ ] Add to profile update events

### **Step 3: Update Dashboard Display (15 minutes)**
- [ ] Add LoadRecentActivities call to dashboard initialize
- [ ] Format ListView display with icons
- [ ] Test activity display

### **Step 4: Test Complete Flow (15 minutes)**
- [ ] Add vital sign → check dashboard shows activity
- [ ] Take medication → check dashboard shows activity
- [ ] Login → check dashboard shows activity
- [ ] Verify activities persist after app restart

## 🎯 **Expected Results**

After implementation, your dashboard will show:
```
Recent Activities:
❤️ Added blood pressure: 120/80 mmHg (Today at 18:30)
💊 Took Aspirin 100mg (Today at 17:45)
➕ Added medication: Vitamin D 1000IU (Today at 16:20)
🌡️ Recorded headache - Level 3 (Today at 15:10)
🔐 Logged in: Welcome back! (Today at 14:00)
```

This creates a **living, dynamic dashboard** that shows users their health tracking progress and encourages continued engagement! 📊✨
