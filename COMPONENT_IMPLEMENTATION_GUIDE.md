# 🛠️ VitalSync - Component Implementation Guide

## 📋 Step-by-Step Implementation

This guide provides detailed instructions for converting VitalSync screens into dynamic components within MIT App Inventor.

## 🎯 **Phase 1: Prepare Foundation**

### **Step 1: Update Screen1 (Navigation Controller)**

#### **Add Global Variables:**
```blocks
Initialize Global Variables:
- global nav_current_screen (text) = "dashboard"
- global nav_current_component (text) = "main"
- global nav_history (list) = create empty list
- global component_data (list) = create empty list
```

#### **Add Navigation Procedures:**
```blocks
Procedure: NavigateToScreen(screen_name, component_name)
  Parameters: screen_name (text), component_name (text)
  
  Actions:
  1. Add current state to nav_history
  2. Set global nav_current_screen to screen_name
  3. Set global nav_current_component to component_name
  4. Store state in TinyDB
  5. Open screen based on screen_name
```

```blocks
Procedure: NavigateToComponent(component_name)
  Parameter: component_name (text)
  
  Actions:
  1. Call HideAllComponents
  2. Call ShowComponent with component_name
  3. Set global nav_current_component to component_name
  4. Store state in TinyDB
  5. Update breadcrumb navigation
```

### **Step 2: Create TinyDB Structure**

#### **Database Tags:**
```
Navigation State:
- "nav_current_screen": Current screen name
- "nav_current_component": Current component name
- "nav_history": List of previous navigation states

Component Data:
- "vital_form_data": Temporary form data for add vital
- "med_form_data": Temporary form data for add medication
- "scanner_result": Last scanned barcode result
- "component_state": Current component visibility states
```

## 🏥 **Phase 2: Implement Health Monitoring Hub (scr_vital_signs)**

### **Step 1: Restructure scr_vital_signs Layout**

#### **New Layout Structure:**
```
Screen Layout:
├── lay_header (HorizontalArrangement)
│   ├── btn_back (Button)
│   ├── lbl_breadcrumb (Label)
│   └── btn_menu (Button)
├── lay_main_content (VerticalScrollArrangement)
│   ├── lay_vitals_main (VerticalArrangement) [Default Visible]
│   ├── lay_add_vital (VerticalArrangement) [Hidden]
│   └── lay_symptoms (VerticalArrangement) [Hidden]
└── lay_navigation (HorizontalArrangement)
```

### **Step 2: Build Main Vitals Component (lay_vitals_main)**

#### **Components:**
```blocks
lay_vitals_main (VerticalArrangement):
├── lay_metrics_grid (TableArrangement 2x3)
│   ├── lay_bp_card (VerticalArrangement)
│   │   ├── lbl_bp_value (Label): "--/--"
│   │   ├── lbl_bp_label (Label): "Pressão Arterial"
│   │   └── btn_add_bp (Button): "Adicionar"
│   ├── lay_hr_card (VerticalArrangement)
│   │   ├── lbl_hr_value (Label): "-- bpm"
│   │   ├── lbl_hr_label (Label): "Freq. Cardíaca"
│   │   └── btn_add_hr (Button): "Adicionar"
│   └── [... other vital cards ...]
├── lay_quick_actions (HorizontalArrangement)
│   ├── btn_add_vital (Button): "Nova Leitura"
│   ├── btn_symptoms (Button): "Sintomas"
│   └── btn_history (Button): "Histórico"
└── lay_recent_readings (VerticalArrangement)
    ├── lbl_recent_title (Label): "Leituras Recentes"
    └── lst_recent_vitals (ListView)
```

### **Step 3: Build Add Vital Component (lay_add_vital)**

#### **Components:**
```blocks
lay_add_vital (VerticalArrangement) [Initially Visible = false]:
├── lay_form_header (HorizontalArrangement)
│   ├── btn_back_to_vitals (Button): "← Voltar"
│   └── lbl_form_title (Label): "Adicionar Sinal Vital"
├── lay_vital_form (VerticalArrangement)
│   ├── pkr_vital_type (ListPicker)
│   │   Options: "Pressão Arterial", "Frequência Cardíaca", "Temperatura", "Peso", "Glicemia"
│   ├── lay_value_input (VerticalArrangement)
│   │   ├── lbl_value_label (Label): "Valor:"
│   │   ├── txt_value_1 (TextBox): First value (e.g., systolic)
│   │   ├── txt_value_2 (TextBox): Second value (e.g., diastolic) [Hidden by default]
│   │   └── lbl_unit (Label): Unit display
│   ├── lay_datetime (HorizontalArrangement)
│   │   ├── pkr_date (DatePicker): Current date
│   │   └── pkr_time (TimePicker): Current time
│   ├── txt_notes (TextBox)
│   │   Hint: "Observações (opcional)"
│   │   Multiline: true
│   └── lay_photo_section (VerticalArrangement)
│       ├── btn_add_photo (Button): "Adicionar Foto"
│       └── img_vital_photo (Image) [Hidden by default]
└── lay_form_actions (HorizontalArrangement)
    ├── btn_save_vital (Button): "Salvar"
    └── btn_cancel_vital (Button): "Cancelar"
```

### **Step 4: Build Symptoms Component (lay_symptoms)**

#### **Components:**
```blocks
lay_symptoms (VerticalArrangement) [Initially Visible = false]:
├── lay_symptoms_header (HorizontalArrangement)
│   ├── btn_back_to_vitals (Button): "← Voltar"
│   └── lbl_symptoms_title (Label): "Registro de Sintomas"
├── lay_quick_symptoms (TableArrangement 2x2)
│   ├── btn_headache (Button): "Dor de Cabeça"
│   ├── btn_fatigue (Button): "Fadiga"
│   ├── btn_nausea (Button): "Náusea"
│   └── btn_dizziness (Button): "Tontura"
├── lay_pain_scale (VerticalArrangement)
│   ├── lbl_pain_title (Label): "Escala de Dor (1-10)"
│   ├── sld_pain_scale (Slider)
│   │   MinValue: 1, MaxValue: 10, ThumbPosition: 1
│   └── lbl_pain_value (Label): "1 - Sem dor"
├── lay_description (VerticalArrangement)
│   ├── lbl_description_title (Label): "Descrição:"
│   └── txt_symptom_description (TextBox)
│       Hint: "Descreva seus sintomas..."
│       Multiline: true
├── lay_symptom_photo (VerticalArrangement)
│   ├── btn_symptom_photo (Button): "Adicionar Foto"
│   └── img_symptom_photo (Image) [Hidden by default]
└── lay_symptoms_actions (HorizontalArrangement)
    ├── btn_save_symptoms (Button): "Salvar"
    └── btn_cancel_symptoms (Button): "Cancelar"
```

### **Step 5: Add Component Management Procedures**

#### **Component Visibility Manager:**
```blocks
Procedure: ShowVitalComponent(component_name)
  Parameter: component_name (text)
  
  Actions:
  1. Set lay_vitals_main.Visible to false
  2. Set lay_add_vital.Visible to false
  3. Set lay_symptoms.Visible to false
  
  4. If component_name = "main":
     - Set lay_vitals_main.Visible to true
     - Set lbl_breadcrumb.Text to "Sinais Vitais"
  
  5. If component_name = "add_vital":
     - Set lay_add_vital.Visible to true
     - Set lbl_breadcrumb.Text to "Sinais Vitais > Adicionar"
     - Call LoadVitalForm
  
  6. If component_name = "symptoms":
     - Set lay_symptoms.Visible to true
     - Set lbl_breadcrumb.Text to "Sinais Vitais > Sintomas"
     - Call LoadSymptomsForm
  
  7. Set global nav_current_component to component_name
  8. Store in TinyDB tag "nav_current_component"
```

#### **Form Data Management:**
```blocks
Procedure: LoadVitalForm
  Actions:
  1. Get saved form data from TinyDB tag "vital_form_data"
  2. If data exists:
     - Restore form field values
     - Show appropriate input fields based on vital type
  3. Set default date/time to current
  4. Clear any previous validation errors

Procedure: SaveVitalFormData
  Actions:
  1. Create list with current form values
  2. Store in TinyDB tag "vital_form_data"
  3. Add timestamp for auto-save

Procedure: ClearVitalFormData
  Actions:
  1. Remove TinyDB tag "vital_form_data"
  2. Reset all form fields to defaults
  3. Hide optional fields
```

### **Step 6: Add Event Handlers**

#### **Navigation Events:**
```blocks
When btn_add_vital.Click:
  Call ShowVitalComponent("add_vital")

When btn_symptoms.Click:
  Call ShowVitalComponent("symptoms")

When btn_back_to_vitals.Click:
  Call ShowVitalComponent("main")
```

#### **Form Events:**
```blocks
When pkr_vital_type.AfterPicking:
  1. Set lbl_value_label.Text based on selection
  2. Set lbl_unit.Text based on selection
  3. Show/hide txt_value_2 based on type
  4. Update input validation rules

When sld_pain_scale.PositionChanged:
  1. Set lbl_pain_value.Text based on position
  2. Update pain description text
  3. Change color based on severity

When btn_save_vital.Click:
  1. Validate all required fields
  2. If valid:
     - Save to TinyDB
     - Call ClearVitalFormData
     - Call ShowVitalComponent("main")
     - Show success message
  3. If invalid:
     - Show validation errors
     - Keep form open

When btn_cancel_vital.Click:
  1. Ask for confirmation if form has data
  2. Call ClearVitalFormData
  3. Call ShowVitalComponent("main")
```

## 💊 **Phase 3: Implement Medication Hub (scr_medications)**

### **Step 1: Restructure scr_medications Layout**

#### **New Layout Structure:**
```
Screen Layout:
├── lay_header (HorizontalArrangement)
├── lay_main_content (VerticalScrollArrangement)
│   ├── lay_meds_main (VerticalArrangement) [Default Visible]
│   ├── lay_add_med (VerticalArrangement) [Hidden]
│   └── lay_scanner (VerticalArrangement) [Hidden]
└── lay_navigation (HorizontalArrangement)
```

### **Step 2: Build Main Medications Component (lay_meds_main)**

#### **Components:**
```blocks
lay_meds_main (VerticalArrangement):
├── lay_next_dose (VerticalArrangement)
│   ├── lbl_next_title (Label): "Próxima Dose"
│   ├── lay_next_card (HorizontalArrangement)
│   │   ├── img_med_icon (Image)
│   │   ├── lay_next_info (VerticalArrangement)
│   │   │   ├── lbl_next_med_name (Label)
│   │   │   ├── lbl_next_time (Label)
│   │   │   └── lbl_next_dosage (Label)
│   │   └── btn_take_med (Button): "Tomado"
├── lay_today_schedule (VerticalArrangement)
│   ├── lbl_today_title (Label): "Programação de Hoje"
│   └── lst_today_meds (ListView)
├── lay_quick_actions (HorizontalArrangement)
│   ├── btn_add_medication (Button): "Novo Medicamento"
│   ├── btn_scan_med (Button): "Escanear"
│   └── btn_med_history (Button): "Histórico"
└── lay_all_medications (VerticalArrangement)
    ├── lbl_all_meds_title (Label): "Todos os Medicamentos"
    └── lst_all_medications (ListView)
```

### **Step 3: Build Add Medication Component (lay_add_med)**

#### **Components:**
```blocks
lay_add_med (VerticalArrangement) [Initially Visible = false]:
├── lay_med_header (HorizontalArrangement)
│   ├── btn_back_to_meds (Button): "← Voltar"
│   └── lbl_add_med_title (Label): "Novo Medicamento"
├── lay_scanner_section (VerticalArrangement)
│   ├── btn_scan_barcode (Button): "Escanear Código de Barras"
│   └── btn_photo_prescription (Button): "Fotografar Receita"
├── lay_med_form (VerticalArrangement)
│   ├── txt_med_name (TextBox)
│   │   Hint: "Nome do medicamento"
│   ├── txt_dosage (TextBox)
│   │   Hint: "Dosagem (ex: 10mg)"
│   ├── txt_quantity (TextBox)
│   │   Hint: "Quantidade em estoque"
│   │   InputType: NUMBER
│   ├── pkr_med_type (ListPicker)
│   │   Text: "Tipo de Medicamento"
│   │   Elements: "Comprimido", "Cápsula", "Líquido", "Injeção", "Pomada"
│   └── txt_med_notes (TextBox)
│       Hint: "Observações"
│       Multiline: true
├── lay_schedule_section (VerticalArrangement)
│   ├── lbl_schedule_title (Label): "Horários de Administração"
│   ├── pkr_frequency (ListPicker)
│   │   Text: "Frequência"
│   │   Elements: "1x ao dia", "2x ao dia", "3x ao dia", "Personalizado"
│   ├── lst_med_times (ListView)
│   └── btn_add_time (Button): "Adicionar Horário"
└── lay_med_actions (HorizontalArrangement)
    ├── btn_save_medication (Button): "Salvar Medicamento"
    └── btn_cancel_medication (Button): "Cancelar"
```

### **Step 4: Build Scanner Component (lay_scanner)**

#### **Components:**
```blocks
lay_scanner (VerticalArrangement) [Initially Visible = false]:
├── lay_scanner_header (HorizontalArrangement)
│   ├── btn_back_to_meds (Button): "← Voltar"
│   └── lbl_scanner_title (Label): "Scanner de Códigos"
├── lay_camera_view (VerticalArrangement)
│   ├── barcode_scanner (BarcodeScanner)
│   └── lbl_scan_instruction (Label): "Aponte para o código de barras"
├── lay_scan_result (VerticalArrangement) [Initially Hidden]
│   ├── lbl_result_title (Label): "Resultado:"
│   ├── lbl_scanned_code (Label)
│   ├── lbl_product_info (Label)
│   └── img_product_image (Image) [Hidden by default]
└── lay_scanner_actions (HorizontalArrangement)
    ├── btn_use_result (Button): "Usar Este Medicamento" [Hidden by default]
    └── btn_scan_again (Button): "Escanear Novamente"
```

## 📊 **Phase 4: Implement Reports Hub (New Screen: scr_reports)**

### **Step 1: Create scr_reports Screen**

#### **Screen Layout:**
```
Screen Layout:
├── lay_header (HorizontalArrangement)
├── lay_main_content (VerticalScrollArrangement)
│   ├── lay_reports_main (VerticalArrangement) [Default Visible]
│   ├── lay_detailed_chart (VerticalArrangement) [Hidden]
│   └── lay_export_data (VerticalArrangement) [Hidden]
└── lay_navigation (HorizontalArrangement)
```

### **Step 2: Build Main Reports Component (lay_reports_main)**

#### **Components:**
```blocks
lay_reports_main (VerticalArrangement):
├── lay_period_selector (HorizontalArrangement)
│   ├── btn_week (Button): "Semana"
│   ├── btn_month (Button): "Mês"
│   └── btn_year (Button): "Ano"
├── lay_health_score (VerticalArrangement)
│   ├── cnv_health_score (Canvas)
│   │   Width: 200, Height: 200
│   ├── lbl_score_value (Label): "85/100"
│   └── lbl_score_message (Label): "Ótimo trabalho!"
├── lay_chart_grid (TableArrangement 2x2)
│   ├── lay_bp_chart (VerticalArrangement)
│   │   ├── lbl_bp_chart_title (Label): "Pressão Arterial"
│   │   ├── cnv_bp_mini (Canvas): Mini chart
│   │   └── btn_view_bp_chart (Button): "Ver Detalhes"
│   ├── lay_weight_chart (VerticalArrangement)
│   │   ├── lbl_weight_chart_title (Label): "Peso"
│   │   ├── cnv_weight_mini (Canvas): Mini chart
│   │   └── btn_view_weight_chart (Button): "Ver Detalhes"
│   └── [... other chart cards ...]
├── lay_summary_stats (VerticalArrangement)
│   ├── lbl_summary_title (Label): "Resumo do Período"
│   └── lst_summary_items (ListView)
└── lay_export_section (HorizontalArrangement)
    ├── btn_export_data (Button): "Exportar Dados"
    └── btn_share_report (Button): "Compartilhar"
```

## 👤 **Phase 5: Extend Profile Hub (scr_profile)**

### **Step 1: Add Components to scr_profile**

#### **Add to existing layout:**
```blocks
Add to lay_main_content:
├── lay_profile_main (existing content)
├── lay_settings (VerticalArrangement) [Hidden]
├── lay_appointments (VerticalArrangement) [Hidden]
└── lay_welcome_tutorial (VerticalArrangement) [Hidden]
```

### **Step 2: Build Settings Component (lay_settings)**

#### **Components:**
```blocks
lay_settings (VerticalArrangement) [Initially Visible = false]:
├── lay_settings_header (HorizontalArrangement)
│   ├── btn_back_to_profile (Button): "← Voltar"
│   └── lbl_settings_title (Label): "Configurações"
├── lay_notifications_section (VerticalArrangement)
│   ├── lbl_notifications_title (Label): "Notificações"
│   ├── lay_med_reminders (HorizontalArrangement)
│   │   ├── lbl_med_reminders (Label): "Lembretes de Medicamentos"
│   │   └── swt_med_reminders (Switch): Checked = true
│   ├── lay_appointment_alerts (HorizontalArrangement)
│   │   ├── lbl_appointment_alerts (Label): "Alertas de Consultas"
│   │   └── swt_appointment_alerts (Switch): Checked = true
│   └── lay_health_tips (HorizontalArrangement)
│       ├── lbl_health_tips (Label): "Dicas de Saúde"
│       └── swt_health_tips (Switch): Checked = false
├── lay_data_section (VerticalArrangement)
│   ├── lbl_data_title (Label): "Dados e Backup"
│   ├── btn_backup_data (Button): "Fazer Backup"
│   ├── btn_restore_data (Button): "Restaurar Dados"
│   ├── btn_export_data (Button): "Exportar Dados"
│   └── btn_clear_data (Button): "Limpar Dados"
├── lay_app_section (VerticalArrangement)
│   ├── lbl_app_title (Label): "Aplicativo"
│   ├── lay_language (HorizontalArrangement)
│   │   ├── lbl_language (Label): "Idioma"
│   │   └── pkr_language (ListPicker): "Português"
│   ├── lay_dark_mode (HorizontalArrangement)
│   │   ├── lbl_dark_mode (Label): "Modo Escuro"
│   │   └── swt_dark_mode (Switch): Checked = false
│   └── btn_tutorial (Button): "Ver Tutorial"
└── lay_about_section (VerticalArrangement)
    ├── lbl_about_title (Label): "Sobre"
    ├── btn_help (Button): "Ajuda e Suporte"
    ├── btn_privacy (Button): "Política de Privacidade"
    ├── btn_about_app (Button): "Sobre o VitalSync"
    └── lbl_version (Label): "Versão 1.0.0"
```

## 🔄 **Phase 6: Global Navigation System**

### **Step 1: Update All Screens with Navigation Manager**

#### **Add to each screen's initialization:**
```blocks
When Screen.Initialize:
  1. Get navigation state from TinyDB
  2. Restore current component visibility
  3. Update breadcrumb navigation
  4. Set up back button handler
```

#### **Global Back Button Handler:**
```blocks
When Screen.BackPressed:
  1. Get current navigation state
  2. If nav_current_component ≠ "main":
     - Return to main component of current screen
     - Update navigation state
  3. Else:
     - Check navigation history
     - Navigate to previous screen
     - Update navigation state
```

### **Step 2: Add Breadcrumb Navigation**

#### **Breadcrumb Component (add to all screens):**
```blocks
lay_breadcrumb (HorizontalArrangement):
├── lbl_breadcrumb (Label)
│   Text: Dynamic based on current location
│   Examples:
│   - "Dashboard"
│   - "Sinais Vitais > Adicionar"
│   - "Medicamentos > Scanner"
│   - "Relatórios > Gráfico Detalhado"
└── btn_breadcrumb_menu (Button): "⋮"
```

## 🎨 **Phase 7: UI Polish and Transitions**

### **Step 1: Add Loading States**

#### **Loading Component (add to each screen):**
```blocks
lay_loading (VerticalArrangement) [Initially Hidden]:
├── img_loading_spinner (Image): Animated spinner
├── lbl_loading_message (Label): "Carregando..."
└── Background: Semi-transparent overlay
```

### **Step 2: Add Smooth Transitions**

#### **Component Transition Manager:**
```blocks
Procedure: TransitionToComponent(component_name)
  1. Show loading overlay
  2. Wait 200ms (Clock timer)
  3. Hide current component
  4. Show target component
  5. Hide loading overlay
  6. Update navigation state
```

## 📱 **Testing Checklist**

### **Component Navigation:**
- [ ] All components show/hide correctly
- [ ] Back button works in all states
- [ ] Breadcrumb navigation updates
- [ ] Navigation history is maintained

### **Data Persistence:**
- [ ] Form data saves automatically
- [ ] Navigation state persists across app restarts
- [ ] Component data doesn't interfere between screens

### **User Experience:**
- [ ] Smooth transitions between components
- [ ] Loading states show appropriately
- [ ] Error handling works correctly
- [ ] All buttons and forms function properly

### **Performance:**
- [ ] App loads quickly with all components
- [ ] Memory usage is acceptable
- [ ] No lag when switching components
- [ ] Database operations are efficient

This implementation guide provides a complete roadmap for converting VitalSync's screen-based architecture to a component-based system that works within MIT App Inventor's 10-screen limitation while maintaining all planned functionality.
