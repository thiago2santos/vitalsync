# 🧩 VitalSync - Páginas de Especificações de Componentes

Este diretório contém páginas HTML interativas que mostram lado a lado o wireframe de cada tela do VitalSync e as especificações detalhadas dos componentes para implementação no MIT App Inventor.

## 📁 Estrutura dos Arquivos

### 🏠 Página Principal
- **`index.html`** - Página índice com links para todas as telas

### 📱 Páginas das Telas
1. **`welcome.html`** - Tela de Boas-vindas (scr_welcome)
2. **`login.html`** - Tela de Login (scr_login)
3. **`dashboard.html`** - Dashboard Principal (scr_dashboard)
4. **`vital-signs.html`** - Monitoramento de Sinais Vitais (scr_vital_signs)
5. **`add-vital.html`** - Adicionar Sinal Vital (scr_add_vital)
6. **`medications.html`** - Gerenciamento de Medicamentos (scr_medications)
7. **`add-medication.html`** - Adicionar Medicamento (scr_add_med)
8. **`scanner.html`** - Scanner de Códigos (scr_scanner)
9. **`symptoms.html`** - Registro de Sintomas (scr_symptoms)
10. **`reports.html`** - Relatórios de Saúde (scr_reports)
11. **`appointments.html`** - Consultas Médicas (scr_appointments)
12. **`profile.html`** - Perfil do Usuário (scr_profile)
13. **`settings.html`** - Configurações (scr_settings)

## 🎯 Como Usar

### 1. Visualização Local
```bash
# Navegue até o diretório
cd screens

# Abra a página principal no navegador
open index.html
# ou no Windows/Linux
start index.html
```

### 2. Navegação
- Comece pela página `index.html` para ter uma visão geral
- Clique em qualquer tela para ver suas especificações detalhadas
- Use o botão "← Voltar ao Índice" para retornar à página principal

### 3. O que Cada Página Contém

#### 📱 Lado Esquerdo - Wireframe
- **Mockup da tela** em formato mobile
- **Visualização realista** dos componentes
- **Layout responsivo** que se adapta ao tamanho da tela

#### 🧩 Lado Direito - Especificações
- **Estrutura de layout** em formato de árvore
- **Componentes detalhados** com propriedades específicas
- **Funcionalidades** de cada tela
- **Informações técnicas** para implementação no MIT App Inventor

## 🛠️ Informações Técnicas

### Componentes Especificados
Cada página inclui especificações completas para:
- **Layouts** (VerticalArrangement, HorizontalArrangement, etc.)
- **Componentes de UI** (Button, Label, TextBox, etc.)
- **Sensores** (Pedometer, Camera, BarcodeScannerSensor, etc.)
- **Armazenamento** (TinyDB, CloudDB)
- **Conectividade** (Bluetooth, Web, etc.)

### Propriedades Detalhadas
- **Textos** e hints dos componentes
- **Cores** e estilos visuais
- **Tamanhos** e dimensões
- **Ações** e navegação entre telas
- **Validações** e regras de negócio

### Funcionalidades por Tela
- **Recursos específicos** de cada funcionalidade
- **Integrações** com sensores e APIs
- **Fluxos de dados** e armazenamento
- **Validações** e tratamento de erros

## 🎨 Design e UX

### Características Visuais
- **Design moderno** com gradientes e sombras
- **Cores consistentes** seguindo a paleta do VitalSync
- **Tipografia clara** e hierarquia visual
- **Responsividade** para diferentes tamanhos de tela

### Experiência do Usuário
- **Navegação intuitiva** entre as páginas
- **Visualização lado a lado** para fácil comparação
- **Informações organizadas** em seções claras
- **Mockups realistas** para melhor compreensão

## 📋 Ordem de Implementação Sugerida

1. **Autenticação** - `welcome.html` + `login.html`
2. **Base Principal** - `dashboard.html`
3. **Core Functionality** - `vital-signs.html` + `add-vital.html`
4. **Medicamentos** - `medications.html` + `add-medication.html`
5. **Configurações** - `profile.html` + `settings.html`
6. **Funcionalidades Avançadas** - `symptoms.html` + `reports.html`
7. **Recursos Premium** - `appointments.html` + `scanner.html`

## 🔧 Componentes Críticos

### Essenciais para o Funcionamento
- **TinyDB** - Armazenamento local de dados
- **Canvas** - Gráficos personalizados e visualizações
- **Camera** - Captura de imagens
- **Clock** - Lembretes e timers

### Opcionais mas Recomendados
- **BarcodeScannerSensor** - Scanner de códigos
- **Pedometer** - Contagem automática de passos
- **BluetoothClient** - Dispositivos médicos
- **CloudDB** - Backup na nuvem

## 📱 Compatibilidade

- **MIT App Inventor 2** - Totalmente compatível
- **Navegadores modernos** - Chrome, Firefox, Safari, Edge
- **Dispositivos móveis** - Layout responsivo
- **Tablets** - Visualização otimizada

## 💡 Dicas de Implementação

1. **Comece simples** - Implemente primeiro as telas básicas
2. **Use as especificações** - Siga exatamente os nomes dos componentes
3. **Teste frequentemente** - Valide cada tela antes de avançar
4. **Mantenha consistência** - Use as cores e estilos especificados
5. **Documente mudanças** - Anote qualquer alteração nas especificações

---

**Desenvolvido para o VitalSync** - App móvel de saúde para MIT App Inventor
**Versão:** 1.0.0 | **Data:** Setembro 2024
