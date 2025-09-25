# 🎨 Missing Vital Signs Wireframes - VitalSync

## 🚨 **Issue Identified**

The `add-vital.html` wireframe currently only shows **Blood Pressure** form, but we need wireframes for all vital signs that will be implemented as components.

## 📊 **Current Status**

### ✅ **Has Wireframe:**
- **Blood Pressure** (Pressão Arterial) - Complete in `add-vital.html`

### ❌ **Missing Wireframes:**
1. **Heart Rate** (Frequência Cardíaca)
2. **Temperature** (Temperatura Corporal)
3. **Weight** (Peso Corporal)
4. **Blood Glucose** (Glicemia)
5. **Steps** (Passos Diários) - Optional, might be automatic

---

## 🎯 **Solution Options**

### **Option 1: Extend Current add-vital.html (Recommended)**
Update the existing `add-vital.html` to show **multiple wireframe variations** for different vital types, similar to how you'd switch between them in the actual component.

### **Option 2: Create Individual Wireframe Files**
Create separate wireframe files for each vital type (not recommended due to maintenance overhead).

### **Option 3: Create Multi-Tab Wireframe**
Create a single wireframe with tabs showing different vital sign forms.

---

## 🎨 **Wireframe Specifications Needed**

### **1. Heart Rate (Frequência Cardíaca)**
```
Form Elements:
├── Title: "Adicionar Frequência Cardíaca"
├── BPM Input: Slider (30-220 bpm) or TextBox
├── Measurement Context: ListPicker
│   └── Options: "Em repouso", "Após exercício", "Durante exercício"
├── Notes: TextBox (multiline)
├── Date/Time: DatePicker + TimePicker
└── Actions: Save/Cancel buttons
```

### **2. Temperature (Temperatura)**
```
Form Elements:
├── Title: "Adicionar Temperatura"
├── Temperature Input: Slider (35-42°C) or TextBox
├── Unit Toggle: Celsius/Fahrenheit
├── Measurement Location: ListPicker
│   └── Options: "Oral", "Axilar", "Testa", "Ouvido"
├── Notes: TextBox (multiline)
├── Date/Time: DatePicker + TimePicker
└── Actions: Save/Cancel buttons
```

### **3. Weight (Peso)**
```
Form Elements:
├── Title: "Adicionar Peso"
├── Weight Input: Slider (20-200 kg) or TextBox
├── Unit Toggle: kg/lbs
├── BMI Display: Calculated automatically (read-only)
├── Measurement Context: ListPicker
│   └── Options: "Manhã", "Tarde", "Noite", "Após exercício"
├── Notes: TextBox (multiline)
├── Date/Time: DatePicker + TimePicker
└── Actions: Save/Cancel buttons
```

### **4. Blood Glucose (Glicemia)**
```
Form Elements:
├── Title: "Adicionar Glicemia"
├── Glucose Input: Slider (50-400 mg/dL) or TextBox
├── Unit Toggle: mg/dL / mmol/L
├── Meal Context: ListPicker
│   └── Options: "Jejum", "Pré-refeição", "Pós-refeição", "Antes de dormir"
├── Notes: TextBox (multiline)
├── Date/Time: DatePicker + TimePicker
└── Actions: Save/Cancel buttons
```

### **5. Steps (Passos) - Optional**
```
Form Elements:
├── Title: "Passos Diários"
├── Steps Display: Auto from Pedometer (read-only)
├── Manual Steps Input: TextBox (if pedometer unavailable)
├── Distance Display: Calculated (read-only)
├── Calories Display: Calculated (read-only)
├── Date: DatePicker (default today)
└── Actions: Save/Refresh buttons
```

---

## 🛠️ **Implementation Plan**

### **Quick Solution for Tonight (30 minutes):**

#### **Update add-vital.html with Vital Type Selector**
Add a dropdown/tabs at the top of the wireframe showing different vital types, then show the appropriate form below based on selection.

```html
<!-- Add this to the wireframe -->
<div class="vital-type-selector">
    <button class="vital-tab active">Pressão Arterial</button>
    <button class="vital-tab">Frequência Cardíaca</button>
    <button class="vital-tab">Temperatura</button>
    <button class="vital-tab">Peso</button>
    <button class="vital-tab">Glicemia</button>
</div>

<!-- Then show different forms based on selection -->
<div class="vital-form bp-form active">
    <!-- Current BP form -->
</div>

<div class="vital-form hr-form">
    <!-- Heart Rate form -->
</div>

<!-- etc. -->
```

### **Complete Solution (2-3 hours):**

#### **Create Comprehensive Multi-Vital Wireframe**
1. **Update existing add-vital.html** with tab navigation
2. **Add all 5 vital sign forms** with proper styling
3. **Include form validation indicators**
4. **Add status indicators** for each vital type
5. **Update component specifications** section

---

## 📱 **MIT App Inventor Component Mapping**

### **For Dynamic Vital Type Selection:**
```blocks
Components Needed:
├── pkr_vital_type (ListPicker)
│   └── Elements: "Pressão Arterial", "Frequência Cardíaca", "Temperatura", "Peso", "Glicemia"
├── lay_bp_form (VerticalArrangement) [Visible when BP selected]
├── lay_hr_form (VerticalArrangement) [Visible when HR selected]
├── lay_temp_form (VerticalArrangement) [Visible when Temp selected]
├── lay_weight_form (VerticalArrangement) [Visible when Weight selected]
└── lay_glucose_form (VerticalArrangement) [Visible when Glucose selected]
```

### **Show/Hide Logic:**
```blocks
When pkr_vital_type.AfterPicking:
1. Hide all form layouts
2. Show selected vital form layout
3. Update form title and labels
4. Set appropriate input ranges/options
```

---

## 🎯 **Priority Action**

### **For Tonight's Implementation:**
1. **Don't worry about wireframes** - focus on the MIT App Inventor implementation
2. **Use the existing BP wireframe as reference** for the other vital types
3. **Create the component logic first**, wireframes can be updated later

### **For Later (When You Have Time):**
1. **Update add-vital.html** with multi-vital wireframe
2. **Add proper visual specifications** for each vital type
3. **Include validation and error states** in wireframes

---

## 💡 **Quick Workaround**

Since you're implementing this as a **VerticalArrangement component** anyway, you can:

1. **Use the BP wireframe as a template** for the visual design
2. **Implement all vital types** using the same layout pattern
3. **Just change the input fields** based on vital type selection
4. **Update wireframes later** when you have more time

The important thing is getting the **functionality working tonight** - the wireframes can be perfected later! 🚀

**Want me to help you focus on the MIT App Inventor implementation instead of wireframes for now?** The procedures I created will work regardless of the exact wireframe details.
