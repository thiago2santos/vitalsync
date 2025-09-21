# 🩺 Blood Pressure Test Values - DetermineBPStatus

## 🎯 **Test Cases for DetermineBPStatus Procedure**

Use these systolic/diastolic pairs to test your `DetermineBPStatus` procedure in MIT App Inventor.

---

## 📊 **Test Values Table**

| Systolic | Diastolic | Expected Status | Medical Category | Test Scenario |
|----------|-----------|----------------|------------------|---------------|
| **85** | **55** | **"Low"** | Hypotension | Low blood pressure |
| **88** | **58** | **"Low"** | Hypotension | Borderline low |
| **90** | **60** | **"Normal"** | Normal | Perfect normal reading |
| **110** | **70** | **"Normal"** | Normal | Healthy adult |
| **118** | **78** | **"Normal"** | Normal | Upper normal range |
| **120** | **80** | **"Normal"** | Normal | Textbook normal |
| **125** | **75** | **"Elevated"** | Elevated | Slightly elevated systolic |
| **128** | **79** | **"Elevated"** | Elevated | Pre-hypertension |
| **129** | **80** | **"Elevated"** | Elevated | Upper elevated range |
| **130** | **85** | **"High Stage 1"** | Stage 1 Hypertension | Early hypertension |
| **135** | **88** | **"High Stage 1"** | Stage 1 Hypertension | Mild hypertension |
| **139** | **89** | **"High Stage 1"** | Stage 1 Hypertension | Upper Stage 1 |
| **145** | **95** | **"High Stage 2"** | Stage 2 Hypertension | Moderate hypertension |
| **160** | **105** | **"High Stage 2"** | Stage 2 Hypertension | Significant hypertension |
| **179** | **119** | **"High Stage 2"** | Stage 2 Hypertension | Upper Stage 2 |
| **185** | **125** | **"Crisis - Seek Medical Attention"** | Hypertensive Crisis | Emergency level |
| **200** | **130** | **"Crisis - Seek Medical Attention"** | Hypertensive Crisis | Severe crisis |

---

## 🧪 **Edge Case Tests**

### **Boundary Value Testing:**
| Systolic | Diastolic | Expected Status | Test Purpose |
|----------|-----------|----------------|--------------|
| **89** | **59** | **"Low"** | Just below normal threshold |
| **90** | **60** | **"Normal"** | Exact normal threshold |
| **120** | **80** | **"Normal"** | Upper normal boundary |
| **121** | **80** | **"Elevated"** | Just above normal |
| **129** | **80** | **"Elevated"** | Upper elevated boundary |
| **130** | **80** | **"High Stage 1"** | Stage 1 threshold |
| **139** | **89** | **"High Stage 1"** | Upper Stage 1 boundary |
| **140** | **90** | **"High Stage 2"** | Stage 2 threshold |
| **179** | **119** | **"High Stage 2"** | Upper Stage 2 boundary |
| **180** | **120** | **"Crisis - Seek Medical Attention"** | Crisis threshold |

### **Mixed Category Tests:**
| Systolic | Diastolic | Expected Status | Notes |
|----------|-----------|----------------|-------|
| **115** | **95** | **"High Stage 1"** | Normal systolic, high diastolic |
| **145** | **75** | **"High Stage 2"** | High systolic, normal diastolic |
| **125** | **85** | **"High Stage 1"** | Elevated systolic, Stage 1 diastolic |
| **165** | **88** | **"High Stage 2"** | Stage 2 systolic, Stage 1 diastolic |

---

## 🔬 **Testing Procedure in MIT App Inventor**

### **Step 1: Create Test Button**
```blocks
Add a test button to your screen:
btn_test_bp_status (Button)
Text: "Test BP Status"
```

### **Step 2: Test Event Handler**
```blocks
When btn_test_bp_status.Click:
1. Test each value pair:
   - set test_result to call DetermineBPStatus(120, 80)
   - show notifier "120/80 = " + test_result
   
2. Run through all test cases:
   - Use "Do It" feature to test individual cases
   - Or create a loop to test multiple values
```

### **Step 3: Automated Testing Loop**
```blocks
Procedure: RunBPTests
Actions:
1. Create test data lists:
   - set systolic_tests to make a list(85, 90, 120, 125, 130, 145, 185)
   - set diastolic_tests to make a list(55, 60, 80, 75, 85, 95, 125)
   - set expected_results to make a list("Low", "Normal", "Normal", "Elevated", "High Stage 1", "High Stage 2", "Crisis - Seek Medical Attention")

2. Loop through tests:
   - set test_index to 1
   - repeat length of systolic_tests times:
     * set sys_val to select list item test_index of systolic_tests
     * set dia_val to select list item test_index of diastolic_tests
     * set expected to select list item test_index of expected_results
     * set actual to call DetermineBPStatus(sys_val, dia_val)
     * if actual = expected:
       - show notifier "✅ PASS: " + sys_val + "/" + dia_val + " = " + actual
     * else:
       - show notifier "❌ FAIL: " + sys_val + "/" + dia_val + " Expected: " + expected + " Got: " + actual
     * set test_index to test_index + 1
```

---

## 📋 **Quick Test Checklist**

### **Essential Tests (Must Pass):**
- [ ] **90/60** → "Normal" (threshold test)
- [ ] **120/80** → "Normal" (standard normal)
- [ ] **125/75** → "Elevated" (elevated category)
- [ ] **130/85** → "High Stage 1" (stage 1 hypertension)
- [ ] **145/95** → "High Stage 2" (stage 2 hypertension)
- [ ] **185/125** → "Crisis - Seek Medical Attention" (emergency)

### **Edge Cases (Should Pass):**
- [ ] **89/59** → "Low" (just below normal)
- [ ] **121/80** → "Elevated" (just above normal)
- [ ] **139/89** → "High Stage 1" (upper stage 1)
- [ ] **179/119** → "High Stage 2" (upper stage 2)

### **Real-World Scenarios:**
- [ ] **110/70** → "Normal" (healthy adult)
- [ ] **135/88** → "High Stage 1" (common mild hypertension)
- [ ] **160/105** → "High Stage 2" (moderate hypertension)

---

## 🎯 **Expected Test Results**

### **If All Tests Pass:**
```
✅ Your DetermineBPStatus procedure is working correctly!
✅ Blood pressure categorization follows medical guidelines
✅ Ready for production use
```

### **If Tests Fail:**
```
❌ Check your procedure logic
❌ Verify the conditional statements (if/else)
❌ Ensure proper comparison operators (<=, >=, AND, OR)
```

---

## 🏥 **Medical Reference**

### **American Heart Association Guidelines:**
- **Normal:** Less than 120/80 mmHg
- **Elevated:** 120-129 systolic and less than 80 diastolic
- **Stage 1:** 130-139 systolic or 80-89 diastolic
- **Stage 2:** 140/90 mmHg or higher
- **Crisis:** Higher than 180/120 mmHg

### **Additional Notes:**
- Always use the **higher category** when systolic and diastolic fall into different categories
- **Crisis readings** require immediate medical attention
- **Low readings** (hypotension) can also be concerning

---

## 🚀 **Quick Start Testing**

**Copy and paste these values directly into your MIT App Inventor testing:**

```
Test 1: 120, 80 → Should return "Normal"
Test 2: 145, 95 → Should return "High Stage 2"  
Test 3: 185, 125 → Should return "Crisis - Seek Medical Attention"
Test 4: 85, 55 → Should return "Low"
Test 5: 125, 75 → Should return "Elevated"
```

Use these test values to ensure your `DetermineBPStatus` procedure works perfectly before implementing the complete vital signs system! 🩺✅
