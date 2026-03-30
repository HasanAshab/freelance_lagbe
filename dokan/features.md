# ক্যাশ মেমো (Cash Memo) - Features Documentation

## Overview
A Bengali cash memo/invoice application with a mobile-first design for creating itemized bills with automatic calculations.

## Core Features

### 1. Location Management
- Input field for shop/location address (ঠিকানা)
- Autocomplete dropdown with predefined locations (Shop A, B, C)
- Customizable location list via datalist

### 2. Dynamic Item Table
- Four-column layout: পরিমাণ (Count), মালের বিবরণ (Details), দর (Rate), টাকা (Taka)
- Auto-expanding rows - new empty row appears when last row is filled
- Vertical column separators (no horizontal row lines)
- Responsive cell inputs with column-specific alignment

### 3. Smart Data Entry

#### Count Field (পরিমাণ)
- Accepts text input with numeric parsing
- Defaults to 1 if no number present
- Extracts first numeric value from input

#### Details Field (মালের বিবরণ)
- Autocomplete with predefined items (Rice, Sugar, Oil)
- Auto-fills Rate when matching item selected
- Auto-calculates Taka when Count contains number

#### Rate Field (দর)
- Numeric input
- Auto-populated from item datalist
- Manual override marks field as user-edited
- Auto-calculates Taka based on Count × Rate

#### Taka Field (টাকা)
- Numeric-only input with validation
- Formatted display with locale thousands separators
- Manual override back-calculates Rate when Count has number
- Raw number shown on focus, formatted on blur

### 4. Calculation Logic

#### Auto-calculation Rules
- Taka = Count × Rate (when Count contains number)
- When Count has no number: treats as 1 for calculation
- Manual Taka entry back-calculates Rate (only when Count has number)
- Manual Rate/Taka edits prevent auto-updates

#### Manual Override Tracking
- `takaManual` flag: prevents auto-calculation of Taka
- `rateManual` flag: prevents auto-fill from datalist

### 5. Keyboard Navigation

#### Enter Key Behavior
- Count → Details (always)
- Details → Rate (if Count has number) OR Taka (if Count empty/no number)
- Rate → Taka (if Rate empty) OR next row (if Rate filled)
- Taka → next row Count (always)

#### Backspace Navigation
- Double backspace on empty field moves to previous field
- Streak counter prevents accidental deletion
- Navigation blocks next backspace to prevent content deletion
- Count field: goes to previous row's Taka

### 6. Summary Section
- Car Rent (গাড়ি ভাড়া): editable numeric input
- Total (মোট): auto-calculated sum of all Taka + Car Rent
- Formatted with thousands separators
- Vertical separator between label and value

### 7. Column Click Focus
- Clicking column header or empty cell area focuses last row's input for that column
- Quick navigation without scrolling

### 8. Print Functionality
- Dedicated print button with icon
- Print-optimized styles (hides buttons, removes shadows)
- Preserves table formatting and calculations

### 9. UI/UX Design
- Mobile-first responsive layout
- Green header (ক্যাশ মেমো title)
- Back button with arrow icon
- Clean card-based table design
- Focus states with subtle background changes
- Smooth transitions and hover effects

## Data Structure

### Predefined Data
```javascript
LOCATIONS: ["Shop A", "Shop B", "Shop C"]
ITEMS: [
  { details: "Rice", rate: 60 },
  { details: "Sugar", rate: 80 },
  { details: "Oil", rate: 150 }
]
```

### Row State
```javascript
{
  id: number,
  count: string,
  details: string,
  rate: number,
  taka: number,
  takaManual: boolean,
  rateManual: boolean
}
```

## Technical Implementation
- Pure HTML/CSS/JavaScript (no frameworks)
- Dynamic DOM manipulation
- Event delegation for row management
- State management with row objects array
- Locale-aware number formatting
- Print media queries

## Browser Features Used
- HTML5 datalist for autocomplete
- CSS Grid/Flexbox for layout
- Input validation and formatting
- Print CSS media queries
- LocaleString for number formatting
