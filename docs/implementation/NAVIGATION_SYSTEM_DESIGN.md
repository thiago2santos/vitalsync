# 🧭 VitalSync - Navigation System Design

## 🎯 Overview

This document details the navigation system design for VitalSync's component-based architecture, allowing seamless navigation between screens and components while maintaining state and providing excellent user experience.

## 🏗️ **Navigation Architecture**

### **Three-Level Navigation Hierarchy:**

```
Level 1: App Level
├── Screen1 (Entry Point)
├── scr_login (Authentication)
├── scr_dashboard (Main Hub)
├── scr_vital_signs (Health Monitoring)
├── scr_medications (Medication Management)
├── scr_reports (Analytics)
└── scr_profile (User & Settings)

Level 2: Screen Level
├── Main Component (Default view)
├── Secondary Components (Feature-specific)
└── Modal Components (Forms, dialogs)

Level 3: Component Level
├── Sub-components
├── Form sections
└── Interactive elements
```

## 📊 **Navigation State Management**

### **Global State Variables:**
```blocks
Global Variables:
├── nav_current_screen (text): Current screen identifier
├── nav_current_component (text): Current component identifier
├── nav_history (list): Navigation history stack
├── nav_component_data (list): Component-specific data
└── nav_breadcrumb (text): Current breadcrumb path
```

### **TinyDB Navigation Schema:**
```
TinyDB Tags:
├── "nav_state": {
│   ├── "current_screen": "vital_signs"
│   ├── "current_component": "add_vital"
│   ├── "history": ["dashboard", "vital_signs"]
│   └── "timestamp": "2024-09-20T10:30:00Z"
│   }
├── "nav_component_data": {
│   ├── "vital_form": {...}
│   ├── "med_form": {...}
│   └── "scanner_result": {...}
│   }
└── "nav_preferences": {
    ├── "default_screen": "dashboard"
    ├── "remember_position": true
    └── "animation_enabled": true
    }
```

## 🔄 **Navigation Flow Patterns**

### **Pattern 1: Screen-to-Screen Navigation**
```
User Action: Dashboard → Vital Signs
Flow:
1. Save current state to nav_history
2. Update nav_current_screen to "vital_signs"
3. Set nav_current_component to "main"
4. Store state in TinyDB
5. Open scr_vital_signs
6. Initialize with "main" component visible
```

### **Pattern 2: Component-to-Component Navigation**
```
User Action: Vital Signs Main → Add Vital Form
Flow:
1. Save current component state
2. Hide all components in current screen
3. Show target component
4. Update nav_current_component
5. Update breadcrumb navigation
6. Store state in TinyDB
```

### **Pattern 3: Back Navigation**
```
User Action: Back button pressed
Flow:
1. Check nav_current_component
2. If not "main":
   - Return to main component
   - Update navigation state
3. If "main":
   - Check nav_history
   - Navigate to previous screen
   - Restore previous state
```

### **Pattern 4: Deep Link Navigation**
```
User Action: Notification → Specific Component
Flow:
1. Parse deep link parameters
2. Navigate to target screen
3. Set target component
4. Pass context data
5. Update navigation state
```

## 🎨 **Visual Navigation Elements**

### **Breadcrumb Navigation Component:**
```blocks
lay_breadcrumb (HorizontalArrangement):
├── btn_home (Button)
│   ├── Icon: House symbol
│   ├── Action: Navigate to dashboard
│   └── Always visible
├── lbl_separator_1 (Label): ">"
├── lbl_screen_name (Label)
│   ├── Text: Current screen name
│   ├── Clickable: true
│   └── Action: Return to main component
├── lbl_separator_2 (Label): ">" [Conditional]
└── lbl_component_name (Label) [Conditional]
    ├── Text: Current component name
    └── Style: Different color/weight
```

### **Navigation Bar Component:**
```blocks
lay_navigation_bar (HorizontalArrangement):
├── btn_nav_dashboard (Button)
│   ├── Icon: Home
│   ├── Text: "Início"
│   └── State: Active/Inactive
├── btn_nav_vitals (Button)
│   ├── Icon: Heart
│   ├── Text: "Vitais"
│   └── Badge: New readings count
├── btn_nav_medications (Button)
│   ├── Icon: Pill
│   ├── Text: "Remédios"
│   └── Badge: Pending doses
├── btn_nav_reports (Button)
│   ├── Icon: Chart
│   ├── Text: "Relatórios"
│   └── Badge: None
└── btn_nav_profile (Button)
    ├── Icon: User
    ├── Text: "Perfil"
    └── Badge: Settings notification
```

### **Component Tab Navigation:**
```blocks
lay_component_tabs (HorizontalArrangement):
├── btn_tab_main (Button)
│   ├── Text: "Principal"
│   ├── State: Active/Inactive
│   └── Underline: When active
├── btn_tab_add (Button)
│   ├── Text: "Adicionar"
│   ├── State: Active/Inactive
│   └── Icon: Plus symbol
└── btn_tab_history (Button)
    ├── Text: "Histórico"
    ├── State: Active/Inactive
    └── Icon: Clock symbol
```

## 🛠️ **Navigation Procedures**

### **Core Navigation Manager:**
```blocks
Procedure: NavigationManager
├── InitializeNavigation()
├── NavigateToScreen(screen_name, component_name, data)
├── NavigateToComponent(component_name, data)
├── HandleBackNavigation()
├── UpdateBreadcrumb()
├── SaveNavigationState()
└── RestoreNavigationState()
```

### **Screen Navigation Procedures:**
```blocks
Procedure: NavigateToScreen(screen_name, component_name, data)
Parameters:
├── screen_name (text): Target screen identifier
├── component_name (text): Target component (default: "main")
└── data (list): Optional context data

Actions:
1. Validate screen_name exists
2. Save current navigation state to history
3. Update global navigation variables
4. Store context data if provided
5. Save state to TinyDB
6. Open target screen
7. Initialize target screen with component
8. Update breadcrumb navigation
9. Log navigation event
```

### **Component Navigation Procedures:**
```blocks
Procedure: NavigateToComponent(component_name, data)
Parameters:
├── component_name (text): Target component identifier
└── data (list): Optional context data

Actions:
1. Validate component exists in current screen
2. Save current component state
3. Hide all components in current screen
4. Show target component
5. Update global nav_current_component
6. Pass context data to component
7. Update breadcrumb navigation
8. Store state in TinyDB
9. Trigger component initialization
```

### **Back Navigation Handler:**
```blocks
Procedure: HandleBackNavigation()
Actions:
1. Get current navigation state
2. If nav_current_component ≠ "main":
   ├── Save current component data
   ├── Navigate to main component
   ├── Update navigation state
   └── Return true (handled)
3. Else if nav_history is not empty:
   ├── Get previous navigation state
   ├── Navigate to previous screen
   ├── Restore previous component
   ├── Update navigation state
   └── Return true (handled)
4. Else:
   └── Return false (let system handle)
```

## 📱 **Screen-Specific Navigation**

### **scr_vital_signs Navigation Map:**
```
Components:
├── "main" (Default)
│   ├── Navigation to: "add_vital", "symptoms", "history"
│   ├── Quick actions: Add readings, view trends
│   └── External: Dashboard, Reports
├── "add_vital"
│   ├── Navigation to: "main", "camera"
│   ├── Form actions: Save, cancel, validate
│   └── Context: Vital type, pre-filled data
├── "symptoms"
│   ├── Navigation to: "main", "camera"
│   ├── Form actions: Save, cancel, pain scale
│   └── Context: Symptom type, severity
└── "history"
    ├── Navigation to: "main", "detail_view"
    ├── Filter actions: Date range, vital type
    └── Context: Selected readings
```

### **scr_medications Navigation Map:**
```
Components:
├── "main" (Default)
│   ├── Navigation to: "add_med", "scanner", "schedule"
│   ├── Quick actions: Take medication, view schedule
│   └── External: Dashboard, Vital Signs
├── "add_med"
│   ├── Navigation to: "main", "scanner", "camera"
│   ├── Form actions: Save, cancel, validate
│   └── Context: Scanned data, prescription photo
├── "scanner"
│   ├── Navigation to: "main", "add_med"
│   ├── Scanner actions: Scan, use result, retry
│   └── Context: Barcode result, product info
└── "schedule"
    ├── Navigation to: "main", "edit_schedule"
    ├── Schedule actions: Add time, remove time
    └── Context: Medication ID, current schedule
```

## 🔔 **Navigation Events and Triggers**

### **User-Initiated Navigation:**
```blocks
Navigation Triggers:
├── Button clicks (primary navigation)
├── List item selections (contextual navigation)
├── Gesture swipes (component switching)
├── Back button presses (reverse navigation)
├── Menu selections (quick navigation)
└── Notification taps (deep link navigation)
```

### **System-Initiated Navigation:**
```blocks
Automatic Navigation:
├── Session timeout → Login screen
├── First app launch → Welcome/Tutorial
├── Form completion → Previous screen
├── Error states → Error recovery screen
├── Permission requests → Settings screen
└── Data sync completion → Refresh current view
```

## 🎯 **Navigation State Persistence**

### **Session Persistence:**
```blocks
On App Pause:
1. Save current navigation state
2. Save component form data
3. Save scroll positions
4. Store timestamp

On App Resume:
1. Restore navigation state
2. Validate state consistency
3. Restore component data
4. Resume from last position
```

### **Cross-Session Persistence:**
```blocks
On App Close:
1. Save navigation preferences
2. Save user's preferred starting screen
3. Save component customizations
4. Clear sensitive temporary data

On App Launch:
1. Check for saved preferences
2. Restore user's preferred flow
3. Initialize with last known good state
4. Clear expired temporary data
```

## 🚀 **Performance Optimizations**

### **Lazy Component Loading:**
```blocks
Component Loading Strategy:
├── Load main components immediately
├── Load secondary components on first access
├── Cache frequently used components
├── Unload unused components after timeout
└── Preload next likely components
```

### **Navigation Caching:**
```blocks
Cache Strategy:
├── Cache navigation state in memory
├── Batch TinyDB writes for performance
├── Use local variables for frequent access
├── Minimize cross-screen data transfer
└── Implement navigation state compression
```

## 🧪 **Testing Navigation Flows**

### **Test Scenarios:**
```
Navigation Testing Checklist:
├── [ ] Forward navigation works correctly
├── [ ] Back navigation maintains state
├── [ ] Deep links navigate properly
├── [ ] Component switching is smooth
├── [ ] State persists across app restarts
├── [ ] Error handling doesn't break navigation
├── [ ] Memory usage remains stable
├── [ ] Performance is acceptable on slow devices
├── [ ] Breadcrumb navigation updates correctly
└── [ ] Navigation history is accurate
```

### **Edge Cases:**
```
Edge Case Testing:
├── [ ] Rapid navigation clicks
├── [ ] Navigation during data loading
├── [ ] Navigation with unsaved form data
├── [ ] Navigation with network errors
├── [ ] Navigation with low memory
├── [ ] Navigation with interrupted flows
├── [ ] Navigation with invalid states
└── [ ] Navigation with corrupted data
```

This navigation system design ensures VitalSync provides a smooth, intuitive user experience while efficiently managing the complexity of a component-based architecture within MIT App Inventor's constraints.
