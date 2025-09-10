# 🎨 VitalSync Icon Library

This folder contains all the custom icons used in the VitalSync app, designed specifically for MIT App Inventor implementation.

## 📁 Available Icons

### 🏥 Core App Icons
- **`vitalsync-logo.svg`** - Main app logo with medical cross and heartbeat
- **`heart-rate.svg`** - Heart with pulse line for vital signs
- **`stethoscope.svg`** - Medical stethoscope for health monitoring

### 💊 Feature Icons
- **`pill.svg`** - Medication/pharmacy icon
- **`thermometer.svg`** - Temperature measurement
- **`chart.svg`** - Data visualization and reports
- **`calendar.svg`** - Appointments and scheduling
- **`scanner.svg`** - Barcode/QR code scanner
- **`notification-bell.svg`** - Alerts and reminders

### 👤 User Interface Icons
- **`user-profile.svg`** - User profile and account
- **`settings.svg`** - App configuration and preferences

## 🎯 Usage in MIT App Inventor

### Image Component Setup
```
1. Upload SVG files to MIT App Inventor Media
2. Set Image Component's Picture property to the icon file
3. Adjust Width/Height for proper sizing (recommended: 48x48 or 64x64)
```

### Button Icons
```
1. Use Image property of Button component
2. Set ButtonShape to "rounded" for modern look
3. Combine with appropriate background colors from the VitalSync palette
```

### Color Palette Used
- **Primary Blue**: #3498DB
- **Success Green**: #27AE60  
- **Warning Orange**: #F39C12
- **Danger Red**: #E74C3C
- **Dark Gray**: #2C3E50
- **Light Gray**: #ECF0F1

## 📱 Recommended Sizes

### For Buttons
- **Small**: 32x32px
- **Medium**: 48x48px  
- **Large**: 64x64px

### For Headers/Logos
- **App Logo**: 64x64px or 96x96px
- **Feature Icons**: 48x48px
- **Navigation Icons**: 32x32px

## 🎨 Design Principles

### Consistency
- All icons follow the same visual style
- Consistent stroke width (2px for main elements)
- Unified color palette across all icons

### Accessibility  
- High contrast colors for visibility
- Clear, recognizable shapes
- Scalable vector format (SVG)

### MIT App Inventor Compatibility
- Simple shapes that render well in the platform
- Appropriate file sizes for mobile apps
- Standard SVG format supported by the platform

## 🔄 Converting to PNG (if needed)

If SVG support is limited, you can convert to PNG:

```bash
# Using online converters or tools like Inkscape
# Recommended PNG sizes: 48x48, 96x96, 144x144 (for different screen densities)
```

## 📋 Icon Usage Map

| Screen | Primary Icons | Secondary Icons |
|--------|---------------|-----------------|
| Welcome | vitalsync-logo, stethoscope | - |
| Dashboard | heart-rate, pill, thermometer | chart, calendar |
| Vital Signs | heart-rate, chart | thermometer |
| Medications | pill, notification-bell | scanner, calendar |
| Symptoms | thermometer | chart |
| Reports | chart | calendar |
| Appointments | calendar | notification-bell |
| Profile | user-profile | settings |
| Settings | settings | notification-bell |
| Scanner | scanner | - |

## 🚀 Implementation Tips

### Performance
- Use appropriate icon sizes to avoid unnecessary scaling
- Consider using PNG for complex icons with many details
- SVG is preferred for simple, scalable icons

### User Experience
- Maintain consistent icon sizing across similar elements
- Use color coding to indicate different types of actions
- Ensure icons are intuitive and match user expectations

### Accessibility
- Provide alternative text descriptions in your app
- Ensure sufficient color contrast
- Test icon visibility on different screen sizes

---

*All icons are custom-designed for the VitalSync project and optimized for MIT App Inventor implementation.*
