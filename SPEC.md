# Maaltijdplanner - Specificatie

## Concept & Vision

Een warme, persoonlijke maaltijdplanner die koken weer leuk maakt. Geen koude, klinische interface, maar een uitnodigende digitale kookboek-plek waar je met plezier je weekplanning maakt. De app voelt als een lieve helper die je helpt herinneren aan je favoriete recepten en slimme boodschappenlijsten voor je maakt.

## Design Language

### Aesthetic Direction
Warm, organisch, met een vleugje Scandinavische eenvoud. Denk aan een gezellige keuken met houten tinten en veel groen - levendig maar rustgevend.

### Color Palette
- **Primary (Warm Terracotta):** `#D97757` - voor buttons en accenten
- **Secondary (Diep Groen):** `#4A7C59` - voor headers en success states
- **Accent (Zacht Geel):** `#F5C86E` - voor highlights en hover states
- **Background:** `#FDF8F3` - warm off-white
- **Surface:** `#FFFFFF` - kaarten en modals
- **Text Primary:** `#2D3436` - donkergrijs, niet zwart
- **Text Secondary:** `#636E72` - voor secundaire tekst
- **Border:** `#E8E0DA` - subtiele warme randen

### Typography
- **Headings:** 'Playfair Display', serif - elegant en warm
- **Body:** 'Inter', sans-serif - modern en leesbaar
- **Font sizes:** 14px base, 1.25 ratio (14, 18, 22, 28, 35)

### Spatial System
- Base unit: 8px
- Spacing: 8, 16, 24, 32, 48px
- Border radius: 12px (cards), 8px (buttons), 20px (pills)
- Max content width: 1200px

### Motion Philosophy
- Subtiele fade-ins voor content (200ms ease-out)
- Hover states met zachte scale (1.02) en schaduw-lift
- Smooth transitions op alle interactieve elementen (150ms)
- Geen abrupte of schokkerige animaties

### Visual Assets
- Lucide Icons via CDN voor consistente iconografie
- Emoji voor categorieën (🍝, 🥗, 🍖, 🍲, etc.)
- Subtiele patronen als achtergronddecoratie

## Layout & Structure

### Page Structure
Drie hoofdsecties via tab-navigatie:
1. **Recepten** - Grid van alle recepten met filter/search
2. **Planner** - Kalender met drag-drop of click-to-add
3. **Boodschappen** - Eenvoudige boodschappenlijst met handmatige items

### Navigation
- Sticky top navigation met logo en tabs
- Tabs met iconen: Recepten (📖), Planner (📅), Boodschappen (🛒)
- Actieve tab heeft underline animatie

### Responsive Strategy
- Desktop: 3-koloms grid voor recepten, volledige kalender
- Tablet: 2-koloms grid, scrollbare kalender
- Mobile: 1-koloms, compacte kalender met swipe

## Features & Interactions

### 1. Recepten Beheer
- **Toevoegen:** Modal met velden voor naam, categorie, ingrediënten (lijst met hoeveelheden), bereidingstijd, porties
- **Importeren van URL:** Plak een recept-URL en het systeem haalt automatisch naam, bereidingstijd, porties en ingrediënten eruit
- **Bewerken:** Zelfde modal, vooraf ingevuld
- **Verwijderen:** Bevestigingsdialoog
- **Zoeken:** Real-time filter op receptnaam
- **Filteren:** Op categorie (ontbijt, lunch, avondeten, snack)
- **Opslag:** LocalStorage voor persistentie

#### URL Import Details
- Gebruikt CORS proxy (allorigins.win) om externe pagina's te laden
- Parseert JSON-LD schema (standaard receptformaat)
- Valideert en extraheert: naam, bereidingstijd, porties, ingrediënten
- Fallback naar HTML parsing als geen JSON-LD aanwezig is
- Laat gebruiker handmatig aanvullen als parsing incompleet is

### 2. Maand Planner
- **Weergave:** Kalender-grid voor huidige maand
- **Navigatie:** Vorige/volgende maand knoppen
- **Toevoegen:** Klik op dag → modal met receptselectie
- **Verwijderen:** Klik op gepland recept → verwijderoptie
- **Visueel:** Dagen met planning tonen gekleurde indicator
- **Huidige dag:** Highlight met accent-kleur

### 3. Boodschappenlijst
- **Handmatige lijst:** Voeg zelf producten toe met naam en hoeveelheid
- **Drag & drop:** Wijzig de volgorde door items te verslepen
- **Bewerken:** Klik op naam of hoeveelheid om te wijzigen
- **Aanvinken:** Checkbox voor gekochte items (lokaal opgeslagen)
- **Verwijderen:** X-knop om items te verwijderen
- **Auto-genereren:** Op basis van geplande recepten in huidige maand
- **Exporteren:** Print-vriendelijke weergave
- **Geen categorieën:** Eenvoudige platte lijst, volgorde naar wens

### Edge Cases
- **Lege receptenlijst:** Welkomstbericht met CTA om eerste recept toe te voegen
- **Lege kalender:** Suggestie-tekst om recepten te plannen
- **Lege boodschappenlijst:** Instructie om producten toe te voegen
- **Dubbele ingrediënten:** Automatisch mergen met optellen van hoeveelheden

## Component Inventory

### Navigation Tab
- Default: Grijs icoon + tekst
- Hover: Terracotta kleur, subtle lift
- Active: Terracotta met animated underline

### Recipe Card
- Wit oppervlak met subtle shadow
- Hover: Lift effect, stronger shadow
- Content: Naam, categorie-badge, bereidingstijd, preview van ingrediënten
- Actions: Edit (pencil), Delete (trash) icons op hover

### Day Cell (Kalender)
- Default: Lichte achtergrond, datumnummer
- Has-meal: Groene stip indicator
- Today: Accent border en lichte highlight
- Hover: Subtle background change
- Click: Opent meal selector modal

### Grocery Item
- Drag handle (⋮⋮) voor herschikken
- Checkbox voor aankoopstatus
- Bewerkbaar naam-veld
- Bewerkbaar hoeveelheid-veld
- Delete button op hover
- Checked: Strike-through, muted kleur

### Modal
- Centered overlay met backdrop blur
- Fade-in animatie
- Sluiten via X, ESC, of backdrop-click
- Formulieren met clear labels en validation feedback

### Button Variants
- Primary: Terracotta achtergrond, wit tekst
- Secondary: Transparent met border
- Danger: Rood voor verwijderen
- Disabled: Muted kleur, geen pointer

### Input Fields
- Subtle border, focus: terracotta border
- Label boven input
- Error state: Rood border + foutmelding

## Technical Approach

### Architecture
Single HTML file met embedded CSS en JavaScript voor eenvoudige distributie.

### Data Model
```javascript
recipes: [{
  id: string,
  name: string,
  category: 'ontbijt' | 'lunch' | 'avondeten' | 'snack',
  prepTime: number (minutes),
  servings: number,
  ingredients: [{ name: string, amount: string, unit: string }]
}]

mealPlan: {
  'YYYY-MM-DD': recipeId
}

groceries: [{
  id: string,
  name: string,
  amount: string
}]

checkedGroceries: [string] // array of grocery ids
```

### Storage
- LocalStorage voor alle data
- Auto-save op elke wijziging

### Key Implementation Details
- Vanilla JS met module pattern
- Event delegation voor performance
- Template literals voor HTML-generatie
- HTML5 Drag and Drop API voor boodschappenlijst
