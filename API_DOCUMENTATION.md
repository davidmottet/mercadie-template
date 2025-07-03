# Nutrition Tracker - API Documentation

## Table of Contents

1. [Project Overview](#project-overview)
2. [Components](#components)
3. [Services & APIs](#services--apis)
4. [Utility Functions](#utility-functions)
5. [Types & Interfaces](#types--interfaces)
6. [Parse Backend Models](#parse-backend-models)
7. [Configuration](#configuration)
8. [Usage Examples](#usage-examples)

## Project Overview

The Nutrition Tracker is a React TypeScript application that allows users to track their daily nutrition intake. It uses Parse as the backend service for data persistence and user authentication.

**Key Features:**
- User authentication (login/signup)
- Daily nutrition logging with multiple nutrients
- Two modes: health and diet with different target values
- Local storage fallback with Parse backend synchronization
- Responsive design with Tailwind CSS

**Tech Stack:**
- React 18 with TypeScript
- Vite for build tooling
- Parse for backend services
- Tailwind CSS for styling
- React Router DOM for navigation

## Components

### App Component

The main application component that handles routing and authentication state.

**Location:** `src/App.tsx`

**Props:** None

**State:**
- `isAuthenticated` (boolean): Current authentication status
- `isLoading` (boolean): Loading state during authentication check

**Routes:**
- `/` - Home page (protected, requires authentication)
- `/login` - Authentication page (redirects to home if authenticated)

**Example Usage:**
```tsx
import App from './App';

// Used in main.tsx
<App />
```

**Key Methods:**
- `handleLogin()`: Sets authentication state to true
- `handleLogout()`: Logs out user via Parse and updates state

---

### Auth Component

Handles user authentication with both login and signup functionality.

**Location:** `src/components/Auth.tsx`

**Props:**
```tsx
interface AuthProps {
  onLogin: () => void;
}
```

**Features:**
- Toggle between login and signup modes
- Session validation on component mount
- Error handling with user-friendly messages
- Automatic redirect if valid session exists

**Example Usage:**
```tsx
import Auth from './components/Auth';

<Auth onLogin={() => setIsAuthenticated(true)} />
```

**State Management:**
- `isLogin` (boolean): Toggle between login/signup
- `email` (string): User email input
- `password` (string): User password input
- `error` (string): Error message display
- `isCheckingSession` (boolean): Session validation loading state

---

### Navbar Component

Navigation component with authentication-aware functionality.

**Location:** `src/components/Navbar.tsx`

**Props:**
```tsx
interface NavbarProps {
  isAuthenticated: boolean;
  onLogout: () => void;
}
```

**Features:**
- Responsive design (hidden on mobile for nav items)
- Active link highlighting
- Authentication-aware button (login/logout)
- Emoji-based icons for better UX

**Example Usage:**
```tsx
import Navbar from './components/Navbar';

<Navbar 
  isAuthenticated={true} 
  onLogout={handleLogout} 
/>
```

## Services & APIs

### Config Service

Manages application configuration and default nutrition logs.

**Location:** `src/services/configService.ts`

#### `getBaseConfig()`
Returns the base nutrition configuration from localStorage or creates default config.

**Returns:** `NutritionLog[]`

**Example:**
```typescript
import { getBaseConfig } from '../services/configService';

const config = getBaseConfig();
console.log(config); // Array of nutrition logs with default values
```

#### `updateBaseConfig(logs: NutritionLog[])`
Updates the base configuration in localStorage.

**Parameters:**
- `logs`: Array of nutrition logs to save

**Example:**
```typescript
import { updateBaseConfig } from '../services/configService';

const updatedLogs = [...existingLogs, newLog];
updateBaseConfig(updatedLogs);
```

#### `resetBaseConfig()`
Resets configuration to default values.

**Example:**
```typescript
import { resetBaseConfig } from '../services/configService';

resetBaseConfig(); // Resets to defaultLogs
```

#### `createDailyLogs(userId: string)`
Creates daily logs for a specific user based on base configuration.

**Parameters:**
- `userId`: The user ID to associate logs with

**Returns:** `NutritionLog[]`

**Example:**
```typescript
import { createDailyLogs } from '../services/configService';

const dailyLogs = createDailyLogs('user123');
// Returns logs for today with currentValue = 0
```

## Utility Functions

### Authentication Utils

**Location:** `src/utils/authUtils.ts`

#### `getCurrentUser()`
Gets the currently authenticated Parse user.

**Returns:** `Promise<Parse.User>`

**Throws:** Error if no user is logged in

**Example:**
```typescript
import { getCurrentUser } from '../utils/authUtils';

try {
  const user = await getCurrentUser();
  console.log('Current user:', user.get('email'));
} catch (error) {
  console.error('No user logged in');
}
```

---

### Date Utils

**Location:** `src/utils/dateUtils.ts`

#### `formatDate(date: Date)`
Formats a Date object to YYYY-MM-DD string.

**Parameters:**
- `date`: Date object to format

**Returns:** `string`

**Example:**
```typescript
import { formatDate } from '../utils/dateUtils';

const today = new Date();
const formatted = formatDate(today);
console.log(formatted); // "2024-01-15"
```

#### `formatReadableDate(dateString: string)`
Formats a date string to human-readable French format.

**Parameters:**
- `dateString`: Date string in YYYY-MM-DD format

**Returns:** `string`

**Example:**
```typescript
import { formatReadableDate } from '../utils/dateUtils';

const readable = formatReadableDate('2024-01-15');
console.log(readable); // "lundi 15 janvier 2024"
```

#### `getNextDay(dateString: string)`
Gets the next day from a given date string.

**Parameters:**
- `dateString`: Date string in YYYY-MM-DD format

**Returns:** `string`

**Example:**
```typescript
import { getNextDay } from '../utils/dateUtils';

const tomorrow = getNextDay('2024-01-15');
console.log(tomorrow); // "2024-01-16"
```

#### `getPrevDay(dateString: string)`
Gets the previous day from a given date string.

**Parameters:**
- `dateString`: Date string in YYYY-MM-DD format

**Returns:** `string`

**Example:**
```typescript
import { getPrevDay } from '../utils/dateUtils';

const yesterday = getPrevDay('2024-01-15');
console.log(yesterday); // "2024-01-14"
```

---

### Parse Storage Utils

**Location:** `src/utils/parseStorageUtils.ts`

Comprehensive API for managing nutrition data with Parse backend.

#### `getInitialState()`
Initializes application state with user's nutrition logs for today.

**Returns:** `Promise<{ logs: NutritionLog[] }>`

**Example:**
```typescript
import { getInitialState } from '../utils/parseStorageUtils';

const initialData = await getInitialState();
console.log(initialData.logs); // Today's nutrition logs
```

#### `getNutritionLogs()`
Fetches current user's nutrition logs for today.

**Returns:** `Promise<NutritionLog[]>`

**Example:**
```typescript
import { getNutritionLogs } from '../utils/parseStorageUtils';

const logs = await getNutritionLogs();
logs.forEach(log => {
  console.log(`${log.name}: ${log.currentValue}/${log.targetValue} ${log.unit.name}`);
});
```

#### `updateNutritionLog(logId, amount, isTarget, date)`
Updates a nutrition log's current value or target value.

**Parameters:**
- `logId` (string): ID of the log to update
- `amount` (number): Amount to add (or set if isTarget=true)
- `isTarget` (boolean): Whether to update target value instead of current
- `date` (string): Date in YYYY-MM-DD format

**Returns:** `Promise<NutritionLog[]>`

**Example:**
```typescript
import { updateNutritionLog } from '../utils/parseStorageUtils';

// Add 0.5L of water
const updatedLogs = await updateNutritionLog('water-log-id', 0.5, false, '2024-01-15');

// Set target calories to 1800
const updatedLogs2 = await updateNutritionLog('calories-log-id', 1800, true, '2024-01-15');
```

#### `getLogsForDate(date: string)`
Retrieves nutrition logs for a specific date.

**Parameters:**
- `date`: Date string in YYYY-MM-DD format

**Returns:** `Promise<NutritionLog[]>`

**Example:**
```typescript
import { getLogsForDate } from '../utils/parseStorageUtils';

const logs = await getLogsForDate('2024-01-15');
console.log(`Found ${logs.length} nutrition logs for the date`);
```

#### `toggleNutritionMode(state, date)`
Toggles between 'health' and 'diet' modes for a specific date.

**Parameters:**
- `state`: Current app state
- `date`: Date string in YYYY-MM-DD format

**Returns:** `Promise<AppState>`

**Example:**
```typescript
import { toggleNutritionMode } from '../utils/parseStorageUtils';

const newState = await toggleNutritionMode(currentState, '2024-01-15');
console.log('Mode switched to:', newState.nutritionLogs['2024-01-15'][0].mode);
```

#### `updateLogToDefault(logId, date)`
Resets a nutrition log to its default target value.

**Parameters:**
- `logId`: ID of the log to reset
- `date`: Date string in YYYY-MM-DD format

**Returns:** `Promise<NutritionLog[]>`

**Example:**
```typescript
import { updateLogToDefault } from '../utils/parseStorageUtils';

const resetLogs = await updateLogToDefault('eau', '2024-01-15');
console.log('Water log reset to default target');
```

---

### Storage Utils (Local Storage)

**Location:** `src/utils/storageUtils.ts`

Local storage utilities for offline functionality and data persistence.

#### `getInitialState()`
Gets initial app state from localStorage or creates default state.

**Returns:** `AppState`

#### `saveState(state: AppState)`
Saves application state to localStorage.

#### `getDailyLog(state, date)`
Gets daily log for a specific date from app state.

#### `updateNutritionGoal(state, date, goalId, amount)`
Updates a nutrition goal's current value.

#### `resetNutritionGoal(state, date, goalId)`
Resets a nutrition goal's current value to 0.

#### `toggleNutritionMode(state, date)`
Toggles nutrition mode between 'health' and 'diet'.

#### `updateNutritionTarget(state, date, goalId, amount)`
Updates target values for nutrition goals.

## Types & Interfaces

**Location:** `src/types/index.ts`

### `MeasurementUnit`
```typescript
interface MeasurementUnit {
  id: string;
  name: string;
}
```

**Usage:** Defines units like 'l' for liters, 'g' for grams, 'kcal' for calories.

### `NutritionLog`
```typescript
interface NutritionLog {
  id: string;
  name: string;
  date: Date;
  currentValue: number;
  targetValue: number;
  mode: 'health' | 'diet';
  unit: MeasurementUnit;
  user: string;
}
```

**Usage:** Core data structure for tracking nutrition intake.

### `AppState`
```typescript
interface AppState {
  currentDate: string;
  nutritionLogs: Record<string, NutritionLog[]>;
}
```

**Usage:** Main application state structure.

## Parse Backend Models

**Location:** `src/parseModels.ts`

### MeasurementUnit Model
Parse class for storing measurement units.

**Attributes:**
- `name` (string): Unit name (e.g., 'l', 'g', 'kcal')

### NutritionLog Model
Parse class for storing nutrition log entries.

**Attributes:**
- `name` (string): Log name (e.g., 'Eau', 'Calories')
- `date` (Date): Log date
- `currentValue` (number): Current intake value
- `targetValue` (number): Target intake value
- `mode` (string): 'health' or 'diet'
- `unit` (Pointer): Reference to MeasurementUnit
- `user` (Pointer): Reference to Parse User

## Configuration

### Parse Configuration

**Location:** `src/parseConfig.ts`

Set up Parse backend connection with environment variables:

```typescript
// Environment variables required:
// VITE_PARSE_APP_ID - Parse application ID
// VITE_PARSE_JAVASCRIPT_KEY - Parse JavaScript key
// VITE_PARSE_SERVER_URL - Parse server URL
```

### Default Nutrition Logs

**Location:** `src/data/defaultLogs.ts`

Default nutrition tracking categories:

```typescript
const defaultLogs = [
  { name: 'Eau', targetValue: 2.5, unit: 'l' },
  { name: 'Calories', targetValue: 2000, unit: 'kcal' },
  { name: 'Protéines', targetValue: 60, unit: 'g' },
  { name: 'Glucides', targetValue: 250, unit: 'g' },
  { name: 'Lipides', targetValue: 70, unit: 'g' }
];
```

## Usage Examples

### Complete Authentication Flow

```typescript
// Login user
const handleLogin = async (email: string, password: string) => {
  try {
    const user = await Parse.User.logIn(email, password);
    console.log('User logged in:', user.get('email'));
    return user;
  } catch (error) {
    console.error('Login failed:', error.message);
    throw error;
  }
};

// Check current user
const checkCurrentUser = async () => {
  try {
    const user = await getCurrentUser();
    console.log('Current user:', user.get('email'));
  } catch (error) {
    console.log('No user logged in');
  }
};
```

### Managing Nutrition Data

```typescript
// Initialize daily nutrition tracking
const initializeDay = async () => {
  try {
    const { logs } = await getInitialState();
    console.log('Today\'s nutrition logs:', logs);
    
    // Update water intake
    const updatedLogs = await updateNutritionLog(
      logs[0].id, // Water log ID
      0.25,       // Add 250ml
      false,      // Update current value
      formatDate(new Date()) // Today
    );
    
    console.log('Updated logs:', updatedLogs);
  } catch (error) {
    console.error('Error initializing day:', error);
  }
};

// Switch to diet mode
const switchToDietMode = async () => {
  const today = formatDate(new Date());
  const newState = await toggleNutritionMode(currentState, today);
  console.log('Switched to diet mode');
};
```

### Date Navigation

```typescript
// Navigate between dates
const navigateDate = (currentDate: string, direction: 'next' | 'prev') => {
  const newDate = direction === 'next' 
    ? getNextDay(currentDate) 
    : getPrevDay(currentDate);
  
  console.log(`Navigating from ${currentDate} to ${newDate}`);
  console.log(`Readable: ${formatReadableDate(newDate)}`);
  
  return newDate;
};
```

### Error Handling Best Practices

```typescript
// Robust error handling for API calls
const safeApiCall = async (apiFunction: () => Promise<any>) => {
  try {
    return await apiFunction();
  } catch (error) {
    if (error.message.includes('Please log in')) {
      // Redirect to login
      window.location.href = '/login';
    } else {
      console.error('API Error:', error.message);
      // Show user-friendly error message
    }
    throw error;
  }
};

// Usage
const logs = await safeApiCall(() => getNutritionLogs());
```

## Best Practices

1. **Authentication**: Always check authentication status before making API calls
2. **Error Handling**: Implement proper error boundaries and user feedback
3. **Date Handling**: Use the provided date utilities for consistent formatting
4. **State Management**: Keep local state in sync with Parse backend
5. **Performance**: Cache frequently accessed data to reduce API calls
6. **User Experience**: Provide loading states and offline functionality

## Environment Setup

Create a `.env` file with the following variables:

```env
VITE_PARSE_APP_ID=your_parse_app_id
VITE_PARSE_JAVASCRIPT_KEY=your_parse_javascript_key
VITE_PARSE_SERVER_URL=your_parse_server_url
```

## Getting Started

1. **Installation**:
   ```bash
   npm install
   ```

2. **Development**:
   ```bash
   npm run dev
   ```

3. **Build**:
   ```bash
   npm run build
   ```

4. **Lint**:
   ```bash
   npm run lint
   ```

This documentation covers all public APIs, functions, and components in the Nutrition Tracker application. For additional details or specific use cases, refer to the inline code comments and TypeScript type definitions.