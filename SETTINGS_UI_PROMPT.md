# TFC ERP Settings Page - Detailed UI Prompt for AI Agents

## Overview

The Settings page is a tabbed navigation interface with 6 main categories. Each category can be independently accessed via URL parameters (`?section=category_id`). The page is fully responsive with a mobile-first sidebar that becomes fixed desktop navigation on larger screens.

---

## Page Structure

### Main Layout

- **Left Sidebar (Fixed on Desktop, Overlay on Mobile)**
  - "Settings" header with close button (mobile only)
  - Search box with clear button
  - Category navigation menu
- **Main Content Area**
  - Breadcrumb: Settings / {Category Name}
  - Page title: "{Category Name} Settings"
  - Category description
  - Dynamic content based on selected category

### Top Header (Desktop)

- Settings icon + "Settings" text
- Forward slash separator "/"
- Active category name in bold

---

## Settings Categories (6 Total)

### 1. COMPANIES (Default Tab)

**ID:** `companies`  
**Icon:** Layers  
**Description:** Create, edit, switch, or remove business entities linked to your account.

**Components:**

- **Header Card**
  - Title: "Manage Companies"
  - Subtitle: "Create, edit, switch, or remove business entities linked to your account."
  - Button: "+ Create Company" (links to `/create-company`)

- **Company Cards Grid** (2 columns on MD+, 1 on mobile)
  - Each card contains:
    - Company Name (large bold text)
    - Location badge (if active): Blue pill "Active"
    - Location text: "{city}, {state}"
    - Border separator
    - Details section (3 items):
      - **Address:** Full address text
      - **Email:** Email or "—" if empty
      - **GSTIN:** Tax ID or "N/A"
    - Action buttons:
      - "Switch" button (secondary, only if NOT current company) - Switches active company and navigates to dashboard
      - "Edit" button (secondary) - Opens `/edit-company/{id}`
      - "Delete" button (secondary, red) - Opens destructive confirmation dialog
    - Visual indicator: Blue ring (2px) if company is current/active

**Destructive Confirmation Dialog (on delete click)**

- Title: `Delete "{companyName}"?`
- Message: "This will permanently remove the company and all associated data — invoices, parties, transactions, and settings. This action cannot be undone."
- Buttons: "Delete Company" (red/destructive) | "Keep Company" (cancel)

---

### 2. FINANCIAL

**ID:** `financial`  
**Icon:** Wallet (DollarSign)  
**Description:** Fiscal year, currency, and tax settings

**Sections:**

#### 2.1 Financial Year

- **Card Header**
  - Title: "Financial Year"
  - Subtitle: "Define your financial year for accounting and reporting purposes."
  - Button: "Auto-generate" (generates April 1 to March 31)

- **Inputs** (2-column grid on MD+)
  - Financial Year Start: Date input with Calendar icon
  - Financial Year End: Date input with Calendar icon

- **Current FY Display**
  - Read-only box showing: "Current Financial Year: [formatted dates]"
  - Background: Zinc-50 / dark: Zinc-900

#### 2.2 Currency & Tax

- **Currency Selection** (2-column grid)
  - Base Currency: Dropdown
    - Options: INR (₹), USD ($), EUR (€), GBP (£), AUD (A$), CAD (C$), SGD (S$), AED (د.إ)
    - Format: "CODE - Full Name (Symbol)"
  - Currency Symbol: Auto-populated text input (editable)

- **Tax System**
  - Select dropdown
  - Options: GST, VAT, Sales Tax, None

#### 2.3 Document Numbering Prefixes

- Three separate sections with divider "Document Prefixes"
- Each section has:
  - Prefix input (text, monospace font)
  - Starting number input (number, monospace font)
  - Live preview box showing: `{prefix}-{startingNumber_padded_to_6_digits}`

**Document Types:**

1. **Invoice**
   - Label: "Invoice Numbering"
   - Prefix input + Starting number input
   - Preview: `INV-000001` (example)

2. **Purchase Order**
   - Label: "Purchase Order Numbering"
   - Prefix input + Starting number input
   - Preview: `PO-000001` (example)

3. **Vehicle Arrival Register**
   - Label: "Vehicle Register Numbering"
   - Prefix input + Starting number input
   - Preview: `VAR-000001` (example)

#### 2.4 Format Settings

- **Date Format** (2-column grid)
  - Select dropdown with options:
    - DD/MM/YYYY (e.g., 25/12/2024)
    - MM/DD/YYYY (e.g., 12/25/2024)
    - YYYY-MM-DD (e.g., 2024-12-25)
    - DD-MM-YYYY (e.g., 25-12-2024)
    - MMMM DD, YYYY (e.g., December 25, 2024)

- **Time Format** (2-column grid)
  - Select dropdown with options:
    - 24-hour (e.g., 14:30)
    - 12-hour (e.g., 2:30 PM)

- **Timezone** (2-column grid)
  - Select dropdown with options:
    - Asia/Kolkata (IST) - UTC+5:30
    - UTC
    - America/New_York (ET) - UTC-5
    - America/Los_Angeles (PT) - UTC-8
    - Europe/London (GMT)
    - Europe/Paris (CET)
    - Asia/Dubai (GST) - UTC+4
    - Asia/Singapore (SGT) - UTC+8
    - Australia/Sydney (AET) - UTC+10

#### 2.5 Rounding Options

- **Round Off Method**
  - Select dropdown with options:
    - Round to nearest
    - Round up
    - Round down
    - Banker's rounding (default)

---

### 3. INVOICE

**ID:** `invoice`  
**Icon:** FileText  
**Description:** Invoice templates and defaults

**Sections:**

#### 3.1 Invoice Template

- **Template Style**
  - Select dropdown
  - Options: Modern, Classic, Minimal, Professional

- **Brand Color**
  - Color picker with label "Brand Color"
  - Description: "Primary color used in invoice design"
  - Shows current hex value

#### 3.2 Invoice Options (separated by divider)

- **Enable QR Code** - Toggle
  - Description: "Add QR code for quick payment verification"

- **Auto Invoice Number** - Toggle
  - Description: "Automatically generate sequential invoice numbers"

#### 3.3 Financial Defaults (2-column grid)

- **Default Tax Rate** - Number input (0-100, step 0.1)
  - Suffix: %
  - Description: "Default tax rate applied to all items"

- **Commission %** - Number input (0-100, step 0.1)
  - Description: "Default commission percentage"

- **Payment Due Days** - Number input (0-365)
  - Description: "Default payment terms in days"

#### 3.4 Invoice Content

- **Company Logo** - File upload area
  - Drag-and-drop box with upload icon
  - Can select sample logos
- **Invoice Notes** - Textarea
  - Placeholder: "Enter default notes or terms"
  - Max length: 500 characters
  - Character counter shown

- **Invoice Footer Text** - Textarea
  - Placeholder: "Enter footer text"
  - Shows in every invoice

- **Bank Details** - Textarea (optional)
  - Placeholder: "Enter bank account details for reference"

#### 3.5 Invoice Preview & Actions

- **Preview Button** - Opens modal with invoice preview
  - Shows sample invoice with all current settings applied
  - Modal actions:
    - Print button (opens browser print dialog)
    - WhatsApp Share button (pre-populated message)
    - Download PDF button (shows "coming soon" message)
    - Close button

---

### 4. BACKUP & DATA

**ID:** `backup`  
**Icon:** Database  
**Description:** Backup, restore, and database management

**Sections:**

#### 4.1 Backup Management

- **Backup Controls**
  - "Create Backup" button - Creates instant backup snapshot
  - "Last Backup: [timestamp]" - Shows last backup date
  - Backup location info

#### 4.2 Restore Data

- **Restore Options**
  - File upload for backup file selection
  - Destructive confirmation required before restore
  - Restore confirmation: "Are you sure? This will replace all current data."

#### 4.3 Backup History

- **Table/List of previous backups**
  - Columns: Date, Size, Status, Actions
  - Actions: Download, Restore, Delete
  - Sorted by date (newest first)

#### 4.4 Database Utilities

- **Database Status**
  - Current size
  - Number of records
  - Last optimization date

- **Database Actions**
  - "Optimize Database" button - Cleans and optimizes DB
  - "Repair Database" button - Runs integrity checks
  - "Export Raw Data" button - Downloads complete database export

---

### 5. APPEARANCE

**ID:** `appearance`  
**Icon:** Palette  
**Description:** Theme, colors, and display preferences

**Sections:**

#### 5.1 Theme Selection

- **3-option button group** (1 col on mobile, 3 col on MD+)
  - **Light** - Sun icon, description: "Clean, bright interface"
  - **Dark** - Moon icon, description: "Easy on the eyes"
  - **System** - Monitor icon, description: "Follows system preference"
  - Selected state: Blue border (2px), blue background (50), checkmark icon (top-right)
  - Inactive state: Gray border, hover effect on interaction

#### 5.2 Accent Color

- **Color Picker Input**
  - Label: "Primary Accent"
  - Description: "Used for buttons, links, and interactive elements"
  - Shows hex value in input
  - Opens color picker modal on click

- **Quick Select Colors** (divider: "Quick Select")
  - 12-color grid (6 cols on tablet, 12 cols on larger)
  - Predefined colors:
    - Blue (#3b82f6)
    - Violet (#8b5cf6)
    - Pink (#ec4899)
    - Red (#ef4444)
    - Orange (#f97316)
    - Yellow (#eab308)
    - Green (#22c55e)
    - Teal (#14b8a6)
    - Cyan (#06b6d4)
    - Indigo (#6366f1)
    - Rose (#f43f5e)
    - Zinc (#78716c)
  - Selected color: 2px blue ring, scale-110 transform
  - Hover: scale-105 transform

#### 5.3 Font & Size

- **Font Size Selection** (3-option button group)
  - Small
  - Medium (default)
  - Large
  - Selected state: Blue styling

- **Font Family** (if supported)
  - Select dropdown
  - Options: System, Sans-serif, Serif, Monospace

#### 5.4 Language

- **Language Selection**
  - Icon: Languages
  - Select dropdown
  - Options: English, ગુજરાતી (Gujarati)
  - Description: "Choose interface language"

#### 5.5 Accessibility

- **High Contrast Mode** - Toggle
  - Description: "Increase contrast for better visibility"

- **Animations** - Toggle
  - Description: "Enable/disable animations and transitions"

- **Reduce Motion** - Toggle
  - Description: "Respect system preference for reduced motion"

---

### 6. SECURITY

**ID:** `security`  
**Icon:** Shield  
**Description:** Password, encryption, and access controls

**Sections:**

#### 6.1 Access Control

- **Require Password** - Toggle
  - Description: "Ask for a password when opening the application"

- **Auto-lock Timeout** - Select dropdown (disabled if password not required)
  - Options: 5min, 15min, 30min, 1hr, 2hr
  - Controlled by password toggle state

- **Session Timeout** - Select dropdown
  - Options: 15min, 30min, 1hr, 2hr, 8hr
  - Default: 1 hour

- **Two-Factor Authentication** - Toggle
  - Description: "Add an extra verification step at login"

#### 6.2 Data Security

- **Allow Data Export** - Toggle
  - Description: "Enable/disable data export functionality"

- **Enable Audit Log** - Toggle
  - Description: "Track changes across the database"

- **Database Encryption** - Toggle
  - Description: "Encrypt sensitive data at rest"

#### 6.3 Change Password

- **Current Password** - Password input (masked)
- **New Password** - Password input (masked)
- **Confirm Password** - Password input (masked)
- **Password Requirements Box**
  - Shows: Minimum 8 characters, 1 uppercase, 1 number, 1 special character
  - Checkmarks ✓ or ✗ appear as user types
- **Update Password Button** - Submits change

#### 6.4 Active Sessions

- **Current Session** - Shows device, IP, last active
- **Active Sessions Table**
  - Columns: Device, IP Address, Last Active, Actions
  - Actions: Logout this session
  - "Logout All Other Sessions" button

#### 6.5 Security Alerts

- **Alert Email Settings**
  - Toggle: "Email alerts for suspicious activity"
  - Email address for alerts (pre-filled)

---

## UI Patterns & Styling

### Cards/Containers

- **Settings Card**
  - White background (dark: zinc-900)
  - Border: zinc-200 (dark: zinc-800)
  - Rounded: xl (rounded-xl)
  - Shadow: 2xs (small shadow)
  - Padding: 6 (p-6)

- **Card Header**
  - Title: text-lg font-bold text-zinc-900 (dark: white)
  - Description: text-sm text-zinc-600 (dark: zinc-400)
  - Padding: p-6

- **Card Content**
  - Padding: p-6
  - Space between sections: space-y-6

### Input Styles

- **Text Inputs, Selects, Date Inputs**
  - Border: zinc-300 (dark: zinc-700)
  - Focus: blue-500 border + blue-100 ring
  - Background: white (dark: zinc-900)
  - Rounded: lg (rounded-lg)
  - Padding: py-2.5 px-4
  - Font: text-sm

- **Error State** (if applicable)
  - Border: red-500
  - Ring: red-100
  - Text: red-600

### Toggles

- **Toggle Switch**
  - Checked: blue-600 background
  - Unchecked: zinc-300 background
  - Has label above or beside
  - Optional description below

- **Toggle with Description**
  - Label (bold, text-sm)
  - Description (text-xs, zinc-500)

### Buttons

- **Primary Button** - Blue gradient, white text
- **Secondary Button** - Zinc/gray background
- **Destructive Button** - Red background (for delete actions)
- **Outline Button** - Border only, no fill
- States:
  - Hover: Slight scale/color change
  - Disabled: Gray, cursor-not-allowed
  - Loading: Spinner animation, disabled state

### Responsive Breakpoints

- **Mobile** (< 640px)
  - Single column grids
  - Full-width buttons
  - Sidebar overlay with backdrop
  - Font sizes reduced

- **Tablet** (640px - 1024px)
  - 2-column grids (grid-cols-1 md:grid-cols-2)
  - Sidebar visible but compact
  - Standard spacing

- **Desktop** (> 1024px)
  - Full layout with fixed sidebar
  - 2-4 column grids (grid-cols-1 md:grid-cols-2 lg:grid-cols-4)
  - Maximum width: max-w-6xl

### Colors

- **Primary** - Blue (#3b82f6)
- **Text**
  - Light: zinc-900
  - Dark: white/zinc-100
- **Borders**
  - Light: zinc-200
  - Dark: zinc-800
- **Backgrounds**
  - Card light: white
  - Card dark: zinc-900
  - Secondary light: zinc-50
  - Secondary dark: zinc-900/50

### Dark Mode

- All components have dark: variant classes
- Dark mode classes prefix with `dark:`
- Triggered by Settings > Appearance > Theme selection

---

## Navigation & URL Structure

### Default Route

- `/settings` or `/settings?section=companies`

### Specific Section Routes

- `/settings?section=financial`
- `/settings?section=invoice`
- `/settings?section=backup`
- `/settings?section=appearance`
- `/settings?section=security`

### Clicking Category

- Updates URL parameter
- Loads corresponding component
- Breadcrumb updates
- Page title updates

### Search Functionality

- Filters categories by label or description
- Case-insensitive
- ESC key clears search
- Mobile: Search remains in sidebar overlay

---

## State Management

- Uses `useAppStore()` hook
- Stores: `settings`, `companies`, `currentCompany`
- Methods: `updateSettings()`, `loadSettings()`, `loadCompanies()`, `setCurrentCompany()`, `deleteCompany()`
- Changes persist to database/local storage

---

## Error Handling & Validation

- Invalid inputs: Red border + error message below field
- Required fields: Marked with red asterisk (\*)
- Confirmation dialogs: For destructive actions (delete, restore)
- Toast notifications: Success/error feedback after actions
- Disabled buttons: When loading or form invalid

---

## Accessibility

- Semantic HTML structure
- ARIA labels on interactive elements
- Keyboard navigation support
- Focus states on all interactive elements
- High contrast text (WCAG AA compliant)
- Color not sole means of conveying information

---

## Performance Considerations

- Lazy load category components on demand
- Debounce search input
- Batch settings updates to minimize re-renders
- Use React.memo() for card components
- Virtualize long lists (sessions, backup history)

---

## Mobile-Specific Behavior

- Sidebar: Overlay with backdrop
- Open/close via hamburger menu
- Swipe to close (optional)
- Single-column layout for all sections
- Touch-friendly button sizes (min 44x44px)
- Full-width inputs and buttons
