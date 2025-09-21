# 🧩 VitalSync - Especificação Detalhada de Componentes

## 📋 Índice de Telas
1. [Tela de Boas-vindas](#1-scr_welcome)
2. [Tela de Login](#2-scr_login)
3. [Dashboard Principal](#3-scr_dashboard)
4. [Monitoramento de Sinais Vitais](#4-scr_vital_signs)
5. [Adicionar Sinal Vital](#5-scr_add_vital)
6. [Gerenciamento de Medicamentos](#6-scr_medications)
7. [Adicionar Medicamento](#7-scr_add_med)
8. [Scanner de Códigos](#8-scr_scanner)
9. [Registro de Sintomas](#9-scr_symptoms)
10. [Relatórios de Saúde](#10-scr_reports)
11. [Consultas Médicas](#11-scr_appointments)
12. [Perfil do Usuário](#12-scr_profile)
13. [Configurações](#13-scr_settings)

---

## 1. 🚀 **scr_welcome**
*Tela de Boas-vindas e Onboarding*

### **Layout Principal:**
```
lay_main (VerticalArrangement)
├── lay_header (VerticalArrangement)
├── lay_content (VerticalScrollArrangement)
└── lay_footer (HorizontalArrangement)
```

### **Componentes Detalhados:**

#### **Header:**
- `img_logo` (Image)
  - **Função:** Logo do VitalSync
  - **Tamanho:** 120x120 pixels
  - **Posição:** Centro do header

#### **Content:**
- `lbl_title` (Label)
  - **Texto:** "Bem-vindo ao VitalSync"
  - **Fonte:** 24px, negrito
  - **Cor:** #2c3e50

- `lbl_subtitle` (Label)
  - **Texto:** "Seu companheiro pessoal de saúde"
  - **Fonte:** 16px, regular
  - **Cor:** #7f8c8d

- `lbl_description` (Label)
  - **Texto:** "Monitore sinais vitais, gerencie medicamentos e acompanhe seu bem-estar com facilidade"
  - **Fonte:** 14px, regular
  - **Cor:** #95a5a6

- `cnv_onboarding` (Canvas)
  - **Função:** Ilustração animada simples
  - **Tamanho:** 200x150 pixels
  - **Conteúdo:** Ícones de saúde desenhados

#### **Footer:**
- `btn_start` (Button)
  - **Texto:** "Começar"
  - **Cor:** #3498db (azul)
  - **Ação:** Navegar para scr_login

- `btn_skip` (Button)
  - **Texto:** "Pular"
  - **Cor:** Transparente com borda
  - **Ação:** Navegar para scr_login

### **Funcionalidades:**
- Animação de entrada suave
- Navegação por gestos (swipe)
- Indicadores de progresso (dots)

---

## 2. 🔐 **scr_login**
*Tela de Login e Cadastro*

### **Layout Principal:**
```
lay_main (VerticalArrangement)
├── lay_header (VerticalArrangement)
├── lay_form (VerticalArrangement)
├── lay_actions (VerticalArrangement)
└── lay_footer (VerticalArrangement)
```

### **Componentes Detalhados:**

#### **Header:**
- `img_logo_small` (Image)
  - **Tamanho:** 80x80 pixels
  - **Posição:** Centro

- `lbl_welcome_back` (Label)
  - **Texto:** "Bem-vindo de volta"
  - **Fonte:** 20px, negrito

#### **Form:**
- `txt_username` (TextBox)
  - **Hint:** "Nome de usuário ou email"
  - **Validação:** Obrigatório, mín. 3 caracteres

- `txt_password` (PasswordTextBox)
  - **Hint:** "Senha"
  - **Validação:** Obrigatório, mín. 6 caracteres

- `swt_remember` (Switch)
  - **Texto:** "Lembrar de mim"
  - **Função:** Salvar credenciais no TinyDB

#### **Actions:**
- `btn_login` (Button)
  - **Texto:** "Entrar"
  - **Cor:** #3498db
  - **Ação:** Validar e navegar para scr_dashboard

- `btn_forgot_password` (Button)
  - **Texto:** "Esqueci a senha"
  - **Estilo:** Link
  - **Ação:** Mostrar pergunta de segurança

#### **Footer:**
- `lbl_register_prompt` (Label)
  - **Texto:** "Não tem uma conta?"

- `btn_register` (Button)
  - **Texto:** "Cadastrar"
  - **Estilo:** Link
  - **Ação:** Alternar para modo cadastro

### **Funcionalidades:**
- Validação em tempo real
- Alternância login/cadastro
- Armazenamento seguro (TinyDB)
- Pergunta de segurança para recuperação

---

## 3. 🏠 **scr_dashboard**
*Dashboard Principal*

### **Layout Principal:**
```
lay_main (VerticalScrollArrangement)
├── lay_header (HorizontalArrangement)
├── lay_health_score (VerticalArrangement)
├── lay_quick_actions (TableArrangement 2x2)
├── lay_recent_activity (VerticalArrangement)
└── lay_navigation (HorizontalArrangement)
```

### **Componentes Detalhados:**

#### **Header:**
- `lbl_greeting` (Label)
  - **Texto:** "Bom dia, [Nome]" (dinâmico por horário)
  - **Fonte:** 18px, negrito

- `lbl_date` (Label)
  - **Texto:** Data atual formatada
  - **Fonte:** 12px, regular

- `img_user_avatar` (Image)
  - **Tamanho:** 40x40 pixels
  - **Forma:** Circular

#### **Health Score:**
- `cnv_health_chart` (Canvas)
  - **Função:** Gráfico circular de pontuação
  - **Tamanho:** 150x150 pixels
  - **Cores:** Gradiente verde-azul

- `lbl_score_value` (Label)
  - **Texto:** "85/100" (dinâmico)
  - **Fonte:** 32px, negrito

- `lbl_score_message` (Label)
  - **Texto:** "Ótimo trabalho!" (baseado na pontuação)
  - **Fonte:** 14px, regular

#### **Quick Actions (Grid 2x2):**
- `btn_vital_signs` (Button)
  - **Ícone:** Coração
  - **Texto:** "Sinais Vitais"
  - **Cor:** #e74c3c

- `btn_medications` (Button)
  - **Ícone:** Pílula
  - **Texto:** "Medicamentos"
  - **Cor:** #27ae60

- `btn_symptoms` (Button)
  - **Ícone:** Termômetro
  - **Texto:** "Sintomas"
  - **Cor:** #f39c12

- `btn_appointments` (Button)
  - **Ícone:** Calendário
  - **Texto:** "Consultas"
  - **Cor:** #9b59b6

#### **Recent Activity:**
- `lbl_activity_title` (Label)
  - **Texto:** "Atividade Recente"
  - **Fonte:** 16px, negrito

- `lst_recent_activity` (ListView)
  - **Itens:** Últimas 5 ações do usuário
  - **Formato:** "Ação - Tempo"

#### **Navigation Bar:**
- `btn_nav_home` (Button)
  - **Estado:** Ativo
  - **Ícone:** Casa

- `btn_nav_monitor` (Button)
  - **Ação:** scr_vital_signs

- `btn_nav_reports` (Button)
  - **Ação:** scr_reports

- `btn_nav_profile` (Button)
  - **Ação:** scr_profile

### **Funcionalidades:**
- Atualização automática da pontuação
- Contagem de passos via Pedometer
- Notificações de lembretes
- Refresh manual (pull-to-refresh)

---

## 4. 📊 **scr_vital_signs**
*Monitoramento de Sinais Vitais*

### **Layout Principal:**
```
lay_main (VerticalScrollArrangement)
├── lay_header (HorizontalArrangement)
├── lay_metrics_grid (TableArrangement 2x3)
├── lay_add_section (VerticalArrangement)
└── lay_navigation (HorizontalArrangement)
```

### **Componentes Detalhados:**

#### **Header:**
- `btn_back` (Button)
  - **Ícone:** Seta esquerda
  - **Ação:** Voltar ao dashboard

- `lbl_title` (Label)
  - **Texto:** "Sinais Vitais"
  - **Fonte:** 18px, negrito

- `btn_history` (Button)
  - **Ícone:** Histórico
  - **Ação:** Ver histórico completo

#### **Metrics Grid (2x3):**
- `lay_bp_card` (VerticalArrangement)
  - `lbl_bp_value` (Label): "--/--"
  - `lbl_bp_label` (Label): "Pressão Arterial"
  - `lbl_bp_status` (Label): "Toque para adicionar"

- `lay_hr_card` (VerticalArrangement)
  - `lbl_hr_value` (Label): "-- bpm"
  - `lbl_hr_label` (Label): "Freq. Cardíaca"
  - `lbl_hr_status` (Label): "Manual"

- `lay_temp_card` (VerticalArrangement)
  - `lbl_temp_value` (Label): "--°C"
  - `lbl_temp_label` (Label): "Temperatura"
  - `lbl_temp_status` (Label): "Toque para adicionar"

- `lay_weight_card` (VerticalArrangement)
  - `lbl_weight_value` (Label): "-- kg"
  - `lbl_weight_label` (Label): "Peso"
  - `lbl_weight_status` (Label): "Bluetooth disponível"

- `lay_glucose_card` (VerticalArrangement)
  - `lbl_glucose_value` (Label): "-- mg/dL"
  - `lbl_glucose_label` (Label): "Glicemia"
  - `lbl_glucose_status` (Label): "Manual"

- `lay_steps_card` (VerticalArrangement)
  - `lbl_steps_value` (Label): "0 passos" (Pedometer)
  - `lbl_steps_label` (Label): "Passos Hoje"
  - `lbl_steps_status` (Label): "Automático"

#### **Add Section:**
- `lbl_add_title` (Label)
  - **Texto:** "Adicionar Nova Leitura"

- `pkr_vital_type` (ListPicker)
  - **Opções:** Pressão, Temperatura, Peso, Glicemia, etc.

- `btn_add_vital` (Button)
  - **Texto:** "Adicionar Leitura"
  - **Ação:** scr_add_vital

- `btn_bluetooth_scan` (Button)
  - **Texto:** "Conectar Dispositivo"
  - **Função:** Bluetooth scan

### **Funcionalidades:**
- Cards clicáveis para entrada rápida
- Integração Pedometer automática
- Bluetooth para dispositivos médicos
- Validação de faixas normais
- Alertas visuais para valores críticos

---

## 5. ➕ **scr_add_vital**
*Adicionar Sinal Vital*

### **Layout Principal:**
```
lay_main (VerticalArrangement)
├── lay_header (HorizontalArrangement)
├── lay_form (VerticalArrangement)
├── lay_datetime (HorizontalArrangement)
├── lay_actions (HorizontalArrangement)
```

### **Componentes Detalhados:**

#### **Header:**
- `btn_back` (Button)
- `lbl_title` (Label): "Adicionar [Tipo]"
- `btn_camera` (Button): Fotografar (se aplicável)

#### **Form:**
- `lbl_vital_type` (Label)
  - **Texto:** Tipo selecionado

- `sld_value_input` (Slider) *OU* `txt_value_input` (TextBox)
  - **Função:** Entrada do valor
  - **Validação:** Faixas específicas por tipo

- `lbl_value_display` (Label)
  - **Texto:** Valor atual do slider

- `txt_notes` (TextBox)
  - **Hint:** "Observações (opcional)"
  - **Multiline:** true

#### **DateTime:**
- `pkr_date` (DatePicker)
  - **Default:** Data atual

- `pkr_time` (TimePicker)
  - **Default:** Hora atual

#### **Actions:**
- `btn_save` (Button)
  - **Texto:** "Salvar"
  - **Cor:** #27ae60
  - **Ação:** Validar e salvar no TinyDB

- `btn_cancel` (Button)
  - **Texto:** "Cancelar"
  - **Cor:** #95a5a6

### **Funcionalidades:**
- Validação em tempo real
- Faixas de valores por tipo
- Captura de fotos (sintomas visuais)
- Alertas para valores críticos
- Salvamento automático

---

## 6. 💊 **scr_medications**
*Gerenciamento de Medicamentos*

### **Layout Principal:**
```
lay_main (VerticalScrollArrangement)
├── lay_header (HorizontalArrangement)
├── lay_next_dose (VerticalArrangement)
├── lay_today_schedule (VerticalArrangement)
├── lay_med_list (VerticalArrangement)
```

### **Componentes Detalhados:**

#### **Header:**
- `btn_back` (Button)
- `lbl_title` (Label): "Medicamentos"
- `btn_add_med` (Button): "+" (Adicionar)

#### **Next Dose:**
- `lay_next_card` (VerticalArrangement)
  - **Background:** Verde claro
  - `lbl_next_title` (Label): "Próxima Dose"
  - `lbl_med_name` (Label): Nome do medicamento
  - `lbl_med_time` (Label): Horário
  - `btn_take_med` (Button): "Marcar como Tomado"

#### **Today's Schedule:**
- `lbl_today_title` (Label): "Programação de Hoje"
- `lst_today_meds` (ListView)
  - **Formato:** Nome | Horário | Status
  - **Status:** Tomado ✓, Pendente ⏰, Perdido ✗

#### **All Medications:**
- `lbl_all_meds_title` (Label): "Todos os Medicamentos"
- `lst_all_medications` (ListView)
  - **Itens:** Lista completa
  - **Ação:** Toque para editar

### **Funcionalidades:**
- Lembretes automáticos (Clock + Sound)
- Status visual por medicamento
- Cálculo de aderência
- Contagem de estoque
- Scanner de códigos de barras

---

## 7. ➕ **scr_add_med**
*Adicionar Medicamento*

### **Layout Principal:**
```
lay_main (VerticalScrollArrangement)
├── lay_header (HorizontalArrangement)
├── lay_scanner_section (VerticalArrangement)
├── lay_form (VerticalArrangement)
├── lay_schedule (VerticalArrangement)
├── lay_actions (HorizontalArrangement)
```

### **Componentes Detalhados:**

#### **Scanner Section:**
- `btn_scan_barcode` (Button)
  - **Texto:** "Escanear Código de Barras"
  - **Ícone:** Scanner
  - **Função:** BarcodeScannerSensor

- `btn_photo_prescription` (Button)
  - **Texto:** "Fotografar Receita"
  - **Função:** Camera

#### **Form:**
- `txt_med_name` (TextBox)
  - **Hint:** "Nome do medicamento"
  - **Obrigatório:** true

- `txt_dosage` (TextBox)
  - **Hint:** "Dosagem (ex: 10mg)"

- `txt_quantity` (TextBox)
  - **Hint:** "Quantidade em estoque"
  - **Tipo:** Numérico

- `pkr_med_type` (ListPicker)
  - **Opções:** Comprimido, Cápsula, Líquido, etc.

#### **Schedule:**
- `lbl_schedule_title` (Label): "Horários"
- `pkr_frequency` (ListPicker)
  - **Opções:** 1x, 2x, 3x ao dia, Personalizado

- `lst_times` (ListView)
  - **Função:** Lista de horários configurados

- `btn_add_time` (Button): "Adicionar Horário"

#### **Actions:**
- `btn_save_med` (Button): "Salvar Medicamento"
- `btn_cancel` (Button): "Cancelar"

### **Funcionalidades:**
- Scanner automático de códigos
- Captura de receitas médicas
- Configuração flexível de horários
- Validação de dados
- Integração com base de medicamentos

---

## 8. 📱 **scr_scanner**
*Scanner de Códigos de Barras*

### **Layout Principal:**
```
lay_main (VerticalArrangement)
├── lay_header (HorizontalArrangement)
├── lay_camera_view (VerticalArrangement)
├── lay_result (VerticalArrangement)
├── lay_actions (HorizontalArrangement)
```

### **Componentes Detalhados:**

#### **Header:**
- `btn_back` (Button)
- `lbl_title` (Label): "Scanner de Códigos"
- `btn_flashlight` (Button): Toggle flash

#### **Camera View:**
- `scan_barcode` (BarcodeScannerSensor)
  - **Função:** Scanner principal
  - **Formato:** Todos os tipos de código

- `lbl_scan_instruction` (Label)
  - **Texto:** "Aponte para o código de barras"

#### **Result:**
- `lbl_result_title` (Label): "Resultado:"
- `lbl_scanned_code` (Label): Código escaneado
- `lbl_product_info` (Label): Informações do produto

#### **Actions:**
- `btn_use_result` (Button): "Usar Este Medicamento"
- `btn_scan_again` (Button): "Escanear Novamente"

### **Funcionalidades:**
- Scanner automático contínuo
- Reconhecimento de múltiplos formatos
- Busca automática de informações
- Integração com base de medicamentos
- Feedback visual e sonoro

---

## 9. 🤒 **scr_symptoms**
*Registro de Sintomas*

### **Layout Principal:**
```
lay_main (VerticalScrollArrangement)
├── lay_header (HorizontalArrangement)
├── lay_quick_symptoms (TableArrangement 2x2)
├── lay_pain_scale (VerticalArrangement)
├── lay_description (VerticalArrangement)
├── lay_photo (VerticalArrangement)
```

### **Componentes Detalhados:**

#### **Quick Symptoms (Grid):**
- `btn_headache` (Button): "Dor de Cabeça"
- `btn_fatigue` (Button): "Fadiga"
- `btn_nausea` (Button): "Náusea"
- `btn_dizziness` (Button): "Tontura"

#### **Pain Scale:**
- `lbl_pain_title` (Label): "Escala de Dor (1-10)"
- `sld_pain_scale` (Slider)
  - **Min:** 1, **Max:** 10
  - **Default:** 1

- `cnv_pain_visual` (Canvas)
  - **Função:** Escala visual colorida
  - **Cores:** Verde → Amarelo → Vermelho

#### **Description:**
- `txt_symptom_description` (TextBox)
  - **Hint:** "Descreva seus sintomas..."
  - **Multiline:** true
  - **Linhas:** 4

#### **Photo:**
- `btn_add_photo` (Button): "Adicionar Foto"
- `img_symptom_photo` (Image): Preview da foto

### **Funcionalidades:**
- Seleção rápida de sintomas comuns
- Escala visual de dor
- Captura de fotos para sintomas visuais
- Histórico de sintomas
- Correlação com medicamentos

---

## 10. 📈 **scr_reports**
*Relatórios de Saúde*

### **Layout Principal:**
```
lay_main (VerticalScrollArrangement)
├── lay_header (HorizontalArrangement)
├── lay_period_selector (HorizontalArrangement)
├── lay_charts (VerticalArrangement)
├── lay_summary (VerticalArrangement)
```

### **Componentes Detalhados:**

#### **Period Selector:**
- `btn_week` (Button): "Semana"
- `btn_month` (Button): "Mês"
- `btn_year` (Button): "Ano"

#### **Charts:**
- `cnv_bp_trend` (Canvas)
  - **Função:** Gráfico de pressão arterial
  - **Tipo:** Linha temporal

- `cnv_weight_progress` (Canvas)
  - **Função:** Progresso do peso
  - **Tipo:** Linha com meta

- `cnv_med_adherence` (Canvas)
  - **Função:** Aderência medicamentos
  - **Tipo:** Gráfico circular

#### **Summary:**
- `lbl_summary_title` (Label): "Resumo do Período"
- `lst_summary_items` (ListView)
  - **Itens:** Estatísticas principais

### **Funcionalidades:**
- Gráficos desenhados com Canvas
- Filtros por período
- Cálculos automáticos de tendências
- Exportação de dados
- Compartilhamento de relatórios

---

## 11. 🏥 **scr_appointments**
*Consultas Médicas*

### **Layout Principal:**
```
lay_main (VerticalScrollArrangement)
├── lay_header (HorizontalArrangement)
├── lay_next_appointment (VerticalArrangement)
├── lay_upcoming (VerticalArrangement)
├── lay_actions (VerticalArrangement)
```

### **Componentes Detalhados:**

#### **Next Appointment:**
- `lay_next_card` (VerticalArrangement)
  - **Background:** Amarelo claro
  - `lbl_next_title` (Label): "Próxima Consulta"
  - `lbl_doctor_name` (Label): Nome do médico
  - `lbl_appointment_date` (Label): Data e hora
  - `lbl_location` (Label): Local
  - `btn_directions` (Button): "Direções"
  - `btn_reschedule` (Button): "Reagendar"

#### **Upcoming:**
- `lbl_upcoming_title` (Label): "Próximas Consultas"
- `lst_appointments` (ListView)
  - **Formato:** Médico | Data | Especialidade

#### **Actions:**
- `btn_add_appointment` (Button): "Nova Consulta"
- `btn_find_pharmacy` (Button): "Farmácias Próximas"

### **Funcionalidades:**
- Lembretes automáticos
- Integração com GPS para direções
- Contatos médicos integrados
- Histórico de consultas
- Localização de farmácias

---

## 12. 👤 **scr_profile**
*Perfil do Usuário*

### **Layout Principal:**
```
lay_main (VerticalScrollArrangement)
├── lay_header (VerticalArrangement)
├── lay_personal_info (VerticalArrangement)
├── lay_health_info (VerticalArrangement)
├── lay_emergency (VerticalArrangement)
```

### **Componentes Detalhados:**

#### **Header:**
- `img_profile_photo` (Image)
  - **Tamanho:** 100x100 pixels
  - **Forma:** Circular

- `btn_change_photo` (Button): "Alterar Foto"
- `lbl_user_name` (Label): Nome completo
- `lbl_user_email` (Label): Email

#### **Personal Info:**
- `lbl_personal_title` (Label): "Informações Pessoais"
- `txt_full_name` (TextBox): Nome completo
- `pkr_birth_date` (DatePicker): Data nascimento
- `txt_height` (TextBox): Altura
- `pkr_gender` (ListPicker): Gênero

#### **Health Info:**
- `lbl_health_title` (Label): "Informações de Saúde"
- `txt_allergies` (TextBox): Alergias
- `txt_conditions` (TextBox): Condições médicas
- `txt_blood_type` (TextBox): Tipo sanguíneo

#### **Emergency:**
- `lbl_emergency_title` (Label): "Contatos de Emergência"
- `lst_emergency_contacts` (ListView)
- `btn_add_contact` (Button): "Adicionar Contato"

### **Funcionalidades:**
- Upload de foto de perfil
- Validação de dados pessoais
- Integração com ContactPicker
- Backup automático de perfil
- Configurações de privacidade

---

## 13. ⚙️ **scr_settings**
*Configurações*

### **Layout Principal:**
```
lay_main (VerticalScrollArrangement)
├── lay_header (HorizontalArrangement)
├── lay_notifications (VerticalArrangement)
├── lay_data (VerticalArrangement)
├── lay_app (VerticalArrangement)
├── lay_about (VerticalArrangement)
```

### **Componentes Detalhados:**

#### **Notifications:**
- `lbl_notifications_title` (Label): "Notificações"
- `swt_med_reminders` (Switch): "Lembretes de Medicamentos"
- `swt_appointment_alerts` (Switch): "Alertas de Consultas"
- `swt_health_tips` (Switch): "Dicas de Saúde"

#### **Data:**
- `lbl_data_title` (Label): "Dados e Backup"
- `btn_backup_data` (Button): "Fazer Backup"
- `btn_restore_data` (Button): "Restaurar Dados"
- `btn_export_data` (Button): "Exportar Dados"
- `btn_clear_data` (Button): "Limpar Dados"

#### **App:**
- `lbl_app_title` (Label): "Aplicativo"
- `pkr_language` (ListPicker): "Idioma"
- `swt_dark_mode` (Switch): "Modo Escuro"
- `btn_tutorial` (Button): "Tutorial"

#### **About:**
- `lbl_about_title` (Label): "Sobre"
- `btn_help` (Button): "Ajuda e Suporte"
- `btn_privacy` (Button): "Política de Privacidade"
- `btn_about` (Button): "Sobre o VitalSync"
- `lbl_version` (Label): "Versão 1.0.0"

### **Funcionalidades:**
- Configurações persistentes (TinyDB)
- Backup na nuvem (CloudDB)
- Exportação de dados
- Sistema de ajuda integrado
- Configurações de acessibilidade

---

## 🔧 **Componentes Globais e Utilitários**

### **Banco de Dados (TinyDB):**
- `db_tiny` (TinyDB)
  - **Tags principais:**
    - `user_profile` - Dados do usuário
    - `vital_signs` - Histórico de sinais vitais
    - `medications` - Lista de medicamentos
    - `symptoms` - Registro de sintomas
    - `appointments` - Consultas agendadas
    - `settings` - Configurações do app

### **Sensores:**
- `pedometer` (Pedometer) - Contagem de passos
- `accelerometer` (AccelerometerSensor) - Detecção de movimento
- `location` (LocationSensor) - GPS para farmácias
- `barcode_scanner` (BarcodeScannerSensor) - Scanner de códigos

### **Conectividade:**
- `bluetooth` (BluetoothClient) - Dispositivos médicos
- `web` (Web) - APIs externas
- `cloud_db` (CloudDB) - Backup opcional

### **Mídia:**
- `camera` (Camera) - Captura de fotos
- `sound_alert` (Sound) - Alertas sonoros
- `sound_recorder` (SoundRecorder) - Notas de voz

### **Comunicação:**
- `phone_call` (PhoneCall) - Chamadas de emergência
- `texting` (Texting) - SMS
- `sharing` (Sharing) - Compartilhamento
- `email_picker` (EmailPicker) - Envio de relatórios

---

## 📋 **Resumo de Implementação**

### **Ordem de Desenvolvimento Sugerida:**
1. `scr_welcome` + `scr_login` (Autenticação)
2. `scr_dashboard` (Base principal)
3. `scr_vital_signs` + `scr_add_vital` (Core functionality)
4. `scr_medications` + `scr_add_med` (Medicamentos)
5. `scr_profile` + `scr_settings` (Configurações)
6. `scr_symptoms` + `scr_reports` (Funcionalidades avançadas)
7. `scr_appointments` + `scr_scanner` (Recursos premium)

### **Componentes Críticos:**
- **TinyDB** para armazenamento local
- **Canvas** para gráficos personalizados
- **BarcodeScannerSensor** para scanner
- **Camera** para captura de imagens
- **Pedometer** para contagem automática
- **Clock** para lembretes e timers

Este documento serve como guia completo para implementação no MIT App Inventor, garantindo consistência e funcionalidade em todas as telas do VitalSync.
