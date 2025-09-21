# 🧭 Screen1 Navigation Code - VitalSync

## 🎯 **Complete Navigation System for Screen1**

Based on your current implementation with 3 VerticalArrangements in Screen1.

## 🛠️ **Global Variables (Add to Screen1)**

```blocks
Initialize Global Variables:
├── global current_component (text) = "welcome"
├── global user_logged_in (logic) = false
└── global user_data (list) = create empty list
```

## 🔄 **Core Navigation Procedure**

### **ShowComponent Procedure:**
```blocks
Procedure: ShowComponent
Parameter: component_name (text)

Actions:
1. Hide all components:
   - set lay_welcome.Visible to false
   - set lay_login.Visible to false
   - set lay_registration.Visible to false

2. Show target component:
   - if component_name = "welcome":
     * set lay_welcome.Visible to true
   - if component_name = "login":
     * set lay_login.Visible to true
   - if component_name = "registration":
     * set lay_registration.Visible to true

3. Update global variable:
   - set global current_component to component_name

4. Store in TinyDB:
   - store component_name in TinyDB tag "current_component"
```

## 🎨 **Component Event Handlers**

### **Welcome Component Events:**
```blocks
When btn_get_started.Click:
  call ShowComponent("login")

When btn_create_account.Click:
  call ShowComponent("registration")
```

### **Login Component Events:**
```blocks
When btn_login.Click:
  if txt_username.Text is not empty AND txt_password.Text is not empty:
    # Simple validation - you can enhance this later
    set global user_logged_in to true
    store txt_username.Text in TinyDB tag "username"
    store true in TinyDB tag "logged_in"
    open another screen "scr_dashboard"
  else:
    show notifier message "Please fill in all fields"

When btn_back_to_welcome.Click:
  call ShowComponent("welcome")

When btn_go_to_register.Click:
  call ShowComponent("registration")
```

### **Registration Component Events:**
```blocks
When btn_register.Click:
  if txt_reg_username.Text is not empty AND 
     txt_reg_password.Text is not empty AND
     txt_reg_email.Text is not empty:
    # Save registration data
    set global user_data to make a list(txt_reg_username.Text, txt_reg_email.Text)
    store global user_data in TinyDB tag "user_profile"
    store true in TinyDB tag "logged_in"
    set global user_logged_in to true
    show notifier message "Account created successfully!"
    open another screen "scr_dashboard"
  else:
    show notifier message "Please fill in all fields"

When btn_back_to_login.Click:
  call ShowComponent("login")

When btn_already_have_account.Click:
  call ShowComponent("login")
```

## 🚀 **Screen Initialization**

### **When Screen1.Initialize:**
```blocks
When Screen1.Initialize:
1. Check if user is already logged in:
   - get value from TinyDB tag "logged_in" (if not there, use false)
   - if value is true:
     * open another screen "scr_dashboard"
   - else:
     * call ShowComponent("welcome")

2. Initialize component visibility:
   - call ShowComponent("welcome")
```

## 🔒 **Simple Form Validation**

### **Enhanced Login Validation:**
```blocks
Procedure: ValidateLogin
Returns: logic (true/false)

Actions:
1. Check username:
   - if length of txt_username.Text < 3:
     * show notifier message "Username must be at least 3 characters"
     * return false

2. Check password:
   - if length of txt_password.Text < 6:
     * show notifier message "Password must be at least 6 characters"  
     * return false

3. If all valid:
   - return true
```

### **Use in Login Button:**
```blocks
When btn_login.Click:
  if call ValidateLogin:
    # Proceed with login
    set global user_logged_in to true
    store txt_username.Text in TinyDB tag "username"
    store true in TinyDB tag "logged_in"
    open another screen "scr_dashboard"
```

## 💾 **Data Storage Structure**

### **TinyDB Tags for Screen1:**
```
"current_component": "welcome" | "login" | "registration"
"logged_in": true | false
"username": User's username
"user_profile": [username, email, full_name]
"remember_me": true | false (if you add this feature)
```

## 🎯 **Quick Implementation Checklist**

### **Step 1: Add Global Variables (5 minutes)**
- [ ] Add current_component variable
- [ ] Add user_logged_in variable
- [ ] Add user_data variable

### **Step 2: Create ShowComponent Procedure (10 minutes)**
- [ ] Add procedure with parameter
- [ ] Add hide all components logic
- [ ] Add show target component logic
- [ ] Add TinyDB storage

### **Step 3: Add Button Event Handlers (15 minutes)**
- [ ] Welcome component buttons
- [ ] Login component buttons
- [ ] Registration component buttons

### **Step 4: Add Screen Initialize Logic (10 minutes)**
- [ ] Check existing login status
- [ ] Initialize with welcome component

### **Step 5: Test Navigation (10 minutes)**
- [ ] Test welcome → login
- [ ] Test welcome → registration
- [ ] Test login → dashboard
- [ ] Test registration → dashboard
- [ ] Test back buttons

## 🚨 **Common Issues & Solutions**

### **Issue: Components not hiding properly**
```blocks
Solution: Make sure all components are set to Visible = false first
Double-check component names match exactly
```

### **Issue: Navigation not working**
```blocks
Solution: Check that procedure names are spelled correctly
Make sure parameters are passed correctly
Use "Do It" to test procedures
```

### **Issue: Data not saving**
```blocks
Solution: Check TinyDB tag names are consistent
Use TinyDB.GetValue with default values
Test with simple text first
```

## 🎉 **Success Test**

Your Screen1 navigation is working correctly when:
- ✅ App starts with welcome screen visible
- ✅ Can switch between welcome/login/registration
- ✅ Login creates user session and opens dashboard
- ✅ Registration creates account and opens dashboard
- ✅ Back buttons work correctly
- ✅ Data persists when app is closed/reopened

This navigation system will serve as the template for all your other component-based screens! 🚀
