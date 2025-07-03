# Mercadie Template Framework - Documentation

## Table of Contents

1. [Template Overview](#template-overview)
2. [Core Components](#core-components)
3. [Services & Utilities](#services--utilities)
4. [Framework Architecture](#framework-architecture)
5. [Types & Interfaces](#types--interfaces)
6. [Parse Backend Integration](#parse-backend-integration)
7. [Configuration](#configuration)
8. [Template Usage Guide](#template-usage-guide)

## Template Overview

Le framework Mercadie est un template de base réutilisable pour créer des applications React TypeScript avec une architecture cohérente. Il fournit une base solide avec authentification, gestion d'état, et intégration Parse backend.

**Fonctionnalités du Template:**
- Système d'authentification complet (login/signup)
- Intégration Parse backend prête à l'emploi
- Gestion d'état local avec fallback localStorage
- Architecture de composants réutilisables
- Système de routing avec protection des routes
- Design responsive avec Tailwind CSS

**Stack Technique:**
- React 18 avec TypeScript
- Vite pour le build et développement
- Parse Server pour le backend
- Tailwind CSS pour le styling
- React Router DOM pour la navigation
- Architecture modulaire et extensible

## Core Components

### App Component (Template Principal)

Le composant racine du template qui gère le routing et l'état d'authentification pour toutes les applications mercadie-*.

**Location:** `src/App.tsx`

**Props:** None

**State:**
- `isAuthenticated` (boolean): État d'authentification global
- `isLoading` (boolean): État de chargement pendant la vérification d'authentification

**Routes Template:**
- `/` - Page d'accueil (protégée, nécessite authentification)
- `/login` - Page d'authentification (redirige vers accueil si connecté)

**Usage dans vos Apps:**
```tsx
import App from './App';

// Point d'entrée principal pour toute app mercadie-*
<App />
```

**Méthodes Clés:**
- `handleLogin()`: Met à jour l'état d'authentification
- `handleLogout()`: Déconnecte l'utilisateur via Parse et met à jour l'état

**Personnalisation pour vos Apps:**
- Ajoutez vos routes spécifiques dans le composant Routes
- Modifiez la page d'accueil selon vos besoins métier
- Gardez la logique d'authentification intacte

---

### Auth Component (Template d'Authentification)

Composant d'authentification réutilisable avec login et inscription intégrés. Prêt à l'emploi pour toutes vos applications mercadie-*.

**Location:** `src/components/Auth.tsx`

**Props:**
```tsx
interface AuthProps {
  onLogin: () => void;
}
```

**Fonctionnalités Template:**
- Basculement automatique login/inscription
- Validation de session au montage du composant
- Gestion d'erreurs avec messages utilisateur
- Redirection automatique si session valide existe

**Usage dans vos Apps:**
```tsx
import Auth from './components/Auth';

// Composant prêt à l'emploi - aucune modification nécessaire
<Auth onLogin={() => setIsAuthenticated(true)} />
```

**État Interne:**
- `isLogin` (boolean): Basculement login/inscription
- `email` (string): Saisie email utilisateur
- `password` (string): Saisie mot de passe utilisateur
- `error` (string): Affichage des erreurs
- `isCheckingSession` (boolean): État de validation de session

**Personnalisation Possible:**
- Modifier les messages en français/autre langue
- Ajouter des champs supplémentaires (nom, prénom, etc.)
- Personnaliser le style CSS selon votre charte graphique

---

### Navbar Component (Template de Navigation)

Composant de navigation réutilisable avec gestion d'authentification intégrée. Base solide pour la navigation de vos applications mercadie-*.

**Location:** `src/components/Navbar.tsx`

**Props:**
```tsx
interface NavbarProps {
  isAuthenticated: boolean;
  onLogout: () => void;
}
```

**Fonctionnalités Template:**
- Design responsive (navigation adaptée mobile/desktop)
- Mise en évidence des liens actifs
- Bouton d'authentification contextuel (login/logout)
- Icônes emoji pour une meilleure UX

**Usage dans vos Apps:**
```tsx
import Navbar from './components/Navbar';

// Structure de base - ajoutez vos liens spécifiques
<Navbar 
  isAuthenticated={true} 
  onLogout={handleLogout} 
/>
```

**Personnalisation pour vos Apps:**
- Ajoutez vos liens de navigation dans la section `nav`
- Modifiez les icônes et textes selon votre domaine métier
- Adaptez les styles CSS à votre charte graphique
- Gardez la logique d'authentification intacte

## Services & Utilities

### Config Service (Template de Configuration)

Service de gestion de configuration réutilisable pour vos applications mercadie-*. Fournit une base pour gérer les données métier de vos applications.

**Location:** `src/services/configService.ts`

#### `getBaseConfig()`
Récupère la configuration de base depuis localStorage ou crée une configuration par défaut.

**Returns:** `NutritionLog[]` (à adapter selon vos données métier)

**Usage Template:**
```typescript
import { getBaseConfig } from '../services/configService';

const config = getBaseConfig();
console.log(config); // Configuration par défaut de votre app
```

#### `updateBaseConfig(logs: NutritionLog[])`
Met à jour la configuration de base dans localStorage.

**Parameters:**
- `logs`: Données de configuration à sauvegarder (à typer selon vos besoins)

**Usage Template:**
```typescript
import { updateBaseConfig } from '../services/configService';

const updatedConfig = [...existingConfig, newItem];
updateBaseConfig(updatedConfig);
```

#### `resetBaseConfig()`
Remet la configuration aux valeurs par défaut.

**Usage Template:**
```typescript
import { resetBaseConfig } from '../services/configService';

resetBaseConfig(); // Reset vers configuration par défaut
```

#### `createDailyLogs(userId: string)`
Crée des entrées quotidiennes pour un utilisateur basé sur la configuration.

**Parameters:**
- `userId`: ID utilisateur pour associer les données

**Returns:** `NutritionLog[]` (à adapter selon vos données métier)

**Usage Template:**
```typescript
import { createDailyLogs } from '../services/configService';

const dailyData = createDailyLogs('user123');
// Retourne les données du jour avec valeurs initiales
```

**🔧 Adaptation pour vos Apps:**
- Remplacez `NutritionLog` par vos types métier
- Modifiez les données par défaut dans `defaultLogs.ts`
- Adaptez la logique selon vos besoins spécifiques

## Framework Architecture

### Authentication Utils (Template d'Auth)

**Location:** `src/utils/authUtils.ts`

Utilitaires d'authentification prêts à l'emploi pour toutes vos applications mercadie-*.

#### `getCurrentUser()`
Récupère l'utilisateur Parse actuellement authentifié.

**Returns:** `Promise<Parse.User>`

**Throws:** Error si aucun utilisateur connecté

**Usage Template:**
```typescript
import { getCurrentUser } from '../utils/authUtils';

try {
  const user = await getCurrentUser();
  console.log('Utilisateur actuel:', user.get('email'));
  // Utilisez les données utilisateur dans votre app
} catch (error) {
  console.error('Aucun utilisateur connecté');
  // Gérez la redirection vers login
}
```

**🔧 Réutilisable dans toutes vos apps mercadie-* sans modification**

---

### Date Utils (Template de Dates)

**Location:** `src/utils/dateUtils.ts`

Utilitaires de manipulation de dates en français, prêts à l'emploi pour vos applications mercadie-*.

#### `formatDate(date: Date)`
Formate un objet Date en chaîne YYYY-MM-DD.

**Parameters:**
- `date`: Objet Date à formater

**Returns:** `string`

**Usage Template:**
```typescript
import { formatDate } from '../utils/dateUtils';

const aujourd_hui = new Date();
const formatted = formatDate(aujourd_hui);
console.log(formatted); // "2024-01-15"
// Idéal pour les clés de stockage, paramètres URL, etc.
```

#### `formatReadableDate(dateString: string)`
Formate une date en format lisible français.

**Parameters:**
- `dateString`: Chaîne de date au format YYYY-MM-DD

**Returns:** `string`

**Usage Template:**
```typescript
import { formatReadableDate } from '../utils/dateUtils';

const readable = formatReadableDate('2024-01-15');
console.log(readable); // "lundi 15 janvier 2024"
// Parfait pour l'affichage utilisateur
```

#### `getNextDay(dateString: string)`
Obtient le jour suivant à partir d'une chaîne de date.

**Parameters:**
- `dateString`: Chaîne de date au format YYYY-MM-DD

**Returns:** `string`

**Usage Template:**
```typescript
import { getNextDay } from '../utils/dateUtils';

const demain = getNextDay('2024-01-15');
console.log(demain); // "2024-01-16"
// Utile pour navigation calendaire
```

#### `getPrevDay(dateString: string)`
Obtient le jour précédent à partir d'une chaîne de date.

**Parameters:**
- `dateString`: Chaîne de date au format YYYY-MM-DD

**Returns:** `string`

**Usage Template:**
```typescript
import { getPrevDay } from '../utils/dateUtils';

const hier = getPrevDay('2024-01-15');
console.log(hier); // "2024-01-14"
// Utile pour navigation calendaire
```

**🔧 Fonctions réutilisables dans toutes vos apps mercadie-* pour:**
- Navigation par dates
- Affichage français standardisé  
- Stockage et manipulation de données temporelles

---

### Parse Storage Utils (Template de Données)

**Location:** `src/utils/parseStorageUtils.ts`

API complète pour gérer les données métier avec Parse backend. Template adaptable pour vos applications mercadie-*.

#### `getInitialState()`
Initialise l'état de l'application avec les données utilisateur pour aujourd'hui.

**Returns:** `Promise<{ logs: NutritionLog[] }>` (à adapter selon vos données métier)

**Usage Template:**
```typescript
import { getInitialState } from '../utils/parseStorageUtils';

const initialData = await getInitialState();
console.log(initialData.logs); // Données du jour pour l'utilisateur
// Adaptez selon vos modèles de données
```

#### `getNutritionLogs()` *(à renommer selon vos données)*
Récupère les données de l'utilisateur actuel pour aujourd'hui.

**Returns:** `Promise<NutritionLog[]>` (à adapter selon vos types)

**Usage Template:**
```typescript
import { getNutritionLogs } from '../utils/parseStorageUtils';

const data = await getNutritionLogs();
data.forEach(item => {
  console.log(`${item.name}: ${item.currentValue}/${item.targetValue} ${item.unit.name}`);
  // Adaptez selon vos propriétés métier
});
```

#### `updateNutritionLog()` *(à renommer selon vos données)*
Met à jour les valeurs courantes ou cibles des données utilisateur.

**Pattern Template:**
```typescript
// Pattern réutilisable pour toute mise à jour de données
const updatedData = await updateDataItem(itemId, newValue, isTarget, date);
```

#### `getLogsForDate()` *(à renommer selon vos données)*
Récupère les données pour une date spécifique.

**Pattern Template:**
```typescript
// Pattern réutilisable pour récupérer des données par date
const dataForDate = await getDataForDate('2024-01-15');
```

**🔧 Autres fonctions du template:**
- `toggleNutritionMode()` - Basculement de modes (adaptable selon vos besoins)
- `updateLogToDefault()` - Reset aux valeurs par défaut
- Et plus de 10 autres fonctions utilitaires...

---

### Storage Utils (Template Local Storage)

**Location:** `src/utils/storageUtils.ts`

Utilitaires de stockage local pour fonctionnalité hors ligne et persistance des données.

**Fonctions Template disponibles:**
- `getInitialState()` - État initial depuis localStorage
- `saveState()` - Sauvegarde état dans localStorage
- `getDailyLog()` - Récupération données quotidiennes
- `updateNutritionGoal()` - Mise à jour valeurs (adaptable)
- `resetNutritionGoal()` - Reset des valeurs
- `toggleNutritionMode()` - Basculement de modes
- `updateNutritionTarget()` - Mise à jour cibles

**🔧 Toutes ces fonctions sont adaptables à vos données métier**

## Types & Interfaces

**Location:** `src/types/index.ts`

### Template d'Interfaces TypeScript

**🔧 Interfaces à adapter selon vos besoins métier:**

#### `MeasurementUnit` *(exemple - à personnaliser)*
```typescript
interface MeasurementUnit {
  id: string;
  name: string;
}
```
**Usage Template:** Définit les unités de vos données métier (poids, temps, quantité, etc.)

#### `NutritionLog` *(exemple - à renommer/adapter)*
```typescript
interface DataItem { // Renommez selon vos besoins
  id: string;
  name: string;
  date: Date;
  currentValue: number;
  targetValue: number;
  mode: 'mode1' | 'mode2'; // Adaptez vos modes
  unit: MeasurementUnit;
  user: string;
}
```
**Usage Template:** Structure de données principale pour vos éléments métier

#### `AppState` *(à adapter)*
```typescript
interface AppState {
  currentDate: string;
  dataItems: Record<string, DataItem[]>; // Adaptez selon vos données
}
```
**Usage Template:** Structure d'état principal de l'application

## Parse Backend Integration

**Location:** `src/parseModels.ts`

### Template de Modèles Parse

#### Modèle d'Unité *(exemple)*
Classe Parse pour stocker les unités de mesure de vos données.

#### Modèle de Données *(exemple)*
Classe Parse pour stocker vos données métier.

**🔧 Adaptez les attributs selon vos besoins:**
- `name`, `date`, `currentValue`, `targetValue`
- `mode`, `unit` (référence), `user` (référence)

## Configuration

### Configuration Parse

**Location:** `src/parseConfig.ts`

Configuration backend Parse prête à l'emploi avec variables d'environnement:

```bash
# Variables requises dans .env
VITE_PARSE_APP_ID=votre_app_id
VITE_PARSE_JAVASCRIPT_KEY=votre_js_key  
VITE_PARSE_SERVER_URL=votre_server_url
```

### Données par Défaut

**Location:** `src/data/defaultLogs.ts`

**🔧 À personnaliser selon vos données métier:**

```typescript
const defaultData = [
  { name: 'Élément 1', targetValue: 100, unit: 'unité1' },
  { name: 'Élément 2', targetValue: 200, unit: 'unité2' },
  // Adaptez selon vos besoins...
];
```

## Template Usage Guide

### Guide de Création d'une App Mercadie-*

#### 1. Flux d'Authentification (Prêt à l'emploi)

```typescript
// Pattern réutilisable - aucune modification nécessaire
const handleAuth = async (email: string, password: string) => {
  try {
    const user = await Parse.User.logIn(email, password);
    console.log('Utilisateur connecté:', user.get('email'));
    return user;
  } catch (error) {
    console.error('Échec connexion:', error.message);
    throw error;
  }
};
```

#### 2. Adaptation des Données Métier

```typescript
// Template à personnaliser selon votre domaine
const initializeMyApp = async () => {
  try {
    // Changez "logs" par vos données métier
    const { myData } = await getInitialState();
    console.log('Données du jour:', myData);
    
    // Adaptez selon vos actions métier
    const updatedData = await updateMyDataItem(
      myData[0].id,
      newValue,
      false,
      formatDate(new Date())
    );
    
    console.log('Données mises à jour:', updatedData);
  } catch (error) {
    console.error('Erreur initialisation:', error);
  }
};
```

#### 3. Navigation par Dates (Réutilisable)

```typescript
// Fonctions prêtes à l'emploi pour navigation calendaire
const naviguerDates = (dateActuelle: string, direction: 'next' | 'prev') => {
  const nouvelleDate = direction === 'next' 
    ? getNextDay(dateActuelle) 
    : getPrevDay(dateActuelle);
  
  console.log(`Navigation de ${dateActuelle} vers ${nouvelleDate}`);
  console.log(`Affichage: ${formatReadableDate(nouvelleDate)}`);
  
  return nouvelleDate;
};
```

### Checklist de Personnalisation

#### ✅ **Obligatoire - À adapter pour chaque app:**
1. **Types TypeScript** (`src/types/index.ts`)
   - Renommer `NutritionLog` → `VotreDataType`
   - Adapter les propriétés selon vos besoins métier

2. **Données par défaut** (`src/data/defaultLogs.ts`)
   - Remplacer par vos données métier par défaut

3. **Modèles Parse** (`src/parseModels.ts`)
   - Adapter les classes Parse selon vos entités

4. **Page d'accueil** (`src/App.tsx`)
   - Remplacer le contenu de la route `/` par votre interface

#### 🔧 **Optionnel - Déjà fonctionnel:**
- Composants Auth et Navbar (styles adaptables)
- Utilitaires de dates (réutilisables tels quels)
- Configuration Parse (juste variables d'environnement)
- Gestion d'authentification (prête à l'emploi)

### Configuration Environnement

```bash
# Fichier .env à créer
VITE_PARSE_APP_ID=votre_app_id_mercadie
VITE_PARSE_JAVASCRIPT_KEY=votre_js_key_mercadie
VITE_PARSE_SERVER_URL=votre_server_url_mercadie
```

### Démarrage Rapide

```bash
# 1. Cloner le template
git clone <template-mercadie>

# 2. Installation
npm install

# 3. Configuration
cp .env.example .env
# Éditer .env avec vos variables Parse

# 4. Personnalisation rapide
# - Éditer src/types/index.ts
# - Éditer src/data/defaultLogs.ts
# - Adapter la page d'accueil dans src/App.tsx

# 5. Développement
npm run dev

# 6. Build production
npm run build
```

### Architecture Recommandée

```
mercadie-monapp/
├── src/
│   ├── components/       # Auth, Navbar (réutilisables)
│   ├── services/         # Config service (à adapter)
│   ├── utils/           # Date, auth, storage (réutilisables)
│   ├── types/           # Interfaces (À ADAPTER)
│   ├── data/            # Données par défaut (À ADAPTER)
│   └── App.tsx          # Page principale (À ADAPTER)
└── .env                 # Variables Parse (À CONFIGURER)
```

**🎯 Ce template vous fait gagner ~80% du temps de développement initial !**

**📚 Pour support et questions:** Consultez les commentaires inline dans le code pour les détails d'implémentation.