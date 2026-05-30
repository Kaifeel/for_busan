# Busan AI Travel Guide Web App

## Objective

Build a small, polished, mobile-first web app for foreign tourists planning a short trip to Busan.

The app should help users quickly create a practical itinerary based on travel duration, budget, travel mood, starting point, and preferred area of Busan. Users should be able to select preferences, view recommended places, see location-based information, and save a short itinerary without reading long travel articles.

The first screen must be the actual trip-planning experience, not a marketing landing page.

---

## Product Scenario

A traveler is visiting Busan for a short trip and wants a simple planner that recommends a route immediately.

Example scenario:

A foreign tourist visiting Busan for one day starts from Busan Station, wants food and ocean views, prefers a mid-range budget, and does not want an overly packed schedule. The app should recommend a practical route, show short descriptions of each place, display location-based information when a place is clicked, and allow the traveler to save selected places into a simple itinerary.

All user-facing text must be written in English.

---

## Core Requirements

Create a working frontend app in this repository.

The app must include:

- A mobile-first trip-planning screen
- Travel duration selection
- Budget range selection
- Companion type selection
- Travel mood/theme filters
- Starting point selection
- East Busan / West Busan / Balanced route preference
- A list or map-style layout of Busan places
- Clickable places or location pins
- Location detail panel shown after clicking a place
- Add-to-itinerary function
- Remove-from-itinerary function
- Duplicate itinerary prevention
- Responsive layout for mobile and desktop
- README instructions for local installation and execution

---

## Recommended MVP Tech Stack

Use a modern lightweight frontend setup.

Recommended stack:

- Next.js App Router
- TypeScript
- Tailwind CSS
- React hooks for client-side state
- Static local data file for Busan places

Do not require a backend server, database, paid API, or external API key for the MVP.

---

## User Inputs

The app should collect the following information from the user.

### 1. Travel Duration

Examples:

- 1 Day
- 2 Days 1 Night
- 3 Days 2 Nights
- Custom

Suggested labels:

- Travel Duration
- Target Trip Length
- How long are you staying in Busan?

### 2. Budget Range

Examples:

- Low
- Mid-range
- Premium
- Custom

The budget should influence recommendations such as food, transportation intensity, activity type, and area suggestions.

### 3. Companion Type

Examples:

- Solo
- Couple
- Friends
- Family

### 4. Standard Travel Theme

Required mood filters:

- Food
- Ocean
- Culture
- Night View
- Relaxed

Additional optional themes:

- Activity
- Shopping
- Healing

### 5. Customized Options

Examples:

- Cafe hopping
- Local markets
- Photo spots
- Easy walking route
- Minimal transportation
- Family-friendly
- Couple-friendly
- Solo travel
- Rainy day route
- Hidden local spots

### 6. Starting Point

Examples:

- Busan Station
- Gimhae International Airport
- Haeundae Station
- Seomyeon Station
- Nampo Station
- Gwangalli Beach
- Custom location

The starting point should affect the recommended route order.

### 7. Area Focus

Options:

- East Busan
- West Busan
- Balanced
- No Preference

East Busan examples:

- Haeundae
- Gwangalli
- Songjeong
- Cheongsapo
- Haedong Yonggungsa
- Centum City

West Busan examples:

- Gamcheon Culture Village
- Jagalchi Market
- BIFF Square
- Nampo
- Songdo
- Taejongdae

---

## Suggested Type Definitions

```ts
type TravelForm = {
  duration: "1 Day" | "2 Days 1 Night" | "3 Days 2 Nights" | "Custom";
  budgetRange: "Low" | "Mid-range" | "Premium" | "Custom";
  companionType: "Solo" | "Couple" | "Friends" | "Family";
  standardTheme:
    | "Food"
    | "Ocean"
    | "Culture"
    | "Night View"
    | "Relaxed"
    | "Activity"
    | "Shopping"
    | "Healing";
  customOptions: string[];
  startingPoint:
    | "Busan Station"
    | "Gimhae International Airport"
    | "Haeundae Station"
    | "Seomyeon Station"
    | "Nampo Station"
    | "Gwangalli Beach"
    | "Custom";
  areaFocus: "East Busan" | "West Busan" | "Balanced" | "No Preference";
};

type BusanPlace = {
  id: string;
  name: string;
  koreanName?: string;
  area: "East Busan" | "West Busan" | "Central Busan";
  addressLabel: string;
  moods: string[];
  budgetLevel: "Low" | "Mid-range" | "Premium";
  description: string;
  recommendedReason: string;
  latitude: number;
  longitude: number;
  recommendedDuration: string;
};
```

---

## Required Busan Places

The local dataset must include at least these places:

- Haeundae Beach
- Gwangalli Beach
- Gamcheon Culture Village
- Jagalchi Market
- Taejongdae
- Songdo Beach / Songdo Skywalk
- BIFF Square
- Seomyeon

Recommended additional places:

- Haedong Yonggungsa Temple
- The Bay 101
- Cheongsapo
- Nampo-dong
- Centum City

---

## Location and Reverse Geocoding Feature

The app should feel like a simple Busan location guide, not only a static list.

### MVP Reverse Geocoding Approach

For the MVP, implement reverse geocoding locally without using a paid map API or backend server.

Use the static Busan place dataset as the source of truth.

Behavior:

- Each place must have latitude and longitude.
- Display places in a clickable map-style layout using cards, pins, or a simple visual grid.
- When the user clicks a place, pin, or coordinate-like item, find the closest known Busan place from the local dataset.
- Show the selected place information in a detail panel.
- Treat this as local reverse geocoding:

```txt
clicked location or coordinate -> nearest known Busan place -> place information
```

### Selected Location Detail Panel

When a user clicks a location, show:

- Place name
- Area
- Address or nearby area label
- Short description
- Mood tags
- Estimated visit time
- Recommended reason
- Distance from user location, only if available
- Add to Itinerary button

### Optional Browser Geolocation

Add a “Use my location” button if practical.

Behavior:

- If the user allows location access, calculate approximate distance from the user to each Busan place.
- Sort or highlight nearby places.
- If the user denies permission, continue showing default Busan recommendations.
- If the user is outside Busan, show a message such as:

```txt
Showing recommended Busan places.
```

Do not fail the app when location permission is denied.

### Distance Calculation

Calculate distance locally using latitude and longitude.

A simple Haversine distance function is enough.

---

## Recommendation Logic

Implement client-side recommendation logic for the MVP.

Recommendations should consider:

```ts
const recommendationFactors = {
  duration,
  budgetRange,
  companionType,
  standardTheme,
  customOptions,
  startingPoint,
  areaFocus,
};
```

The app should:

- Filter places by selected mood
- Prioritize places based on area focus
- Consider budget level
- Consider starting point when ordering route suggestions
- Keep recommendations short and easy to scan
- Avoid overly packed itineraries for relaxed trips

---

## Itinerary Features

Users should be able to build a simple itinerary.

Required behavior:

- Add a place to the itinerary
- Remove a place from the itinerary
- Prevent duplicate places
- Show an empty-state message when no places are selected
- Show selected place count
- Show total estimated visit time if possible
- Keep itinerary visible or easily accessible on mobile

---

## UI and UX Requirements

The app should be clean, practical, and easy to scan.

Design direction:

- Mobile-first layout
- 100% English UI
- Large tap-friendly buttons
- Clear mood filters
- Simple cards
- Practical spacing
- No unnecessary marketing sections
- No long explanations on the first screen

Recommended layout style:

```txt
max-w-md mx-auto min-h-screen shadow-lg
```

Responsive behavior:

- Mobile: single-column layout
- Desktop: expanded layout with place list and itinerary/location panel side by side

The first screen must remain functional and usable immediately.

---

## Error Handling Requirements

The app should not crash if optional features fail.

Required safeguards:

- If browser geolocation fails, show default Busan recommendations.
- If a map-like component fails, show a clean placeholder instead of a blank screen.
- If optional external APIs are later added, provide fallback static content.
- Display clear English error or fallback messages.

Example fallback messages:

```txt
Unable to access your location. Showing default Busan recommendations.
```

```txt
Map view is unavailable. You can still browse places below.
```

---

## README Requirements

Update `README.md` with:

- Short project description
- Tech stack
- Feature summary
- Install command
- Local run command
- Build command

Example commands:

```bash
npm install
npm run dev
npm run build
```

---

## Development Workflow

The AI agent should directly modify the repository and build the app, not only describe a plan.

Before coding:

- Inspect the existing repository structure.
- If a frontend setup already exists, use the existing setup.
- If no frontend setup exists, create a simple Next.js + TypeScript + Tailwind CSS app.
- Keep the implementation simple enough for a hackathon prototype.

After each major step:

- Run a local build or lint command.
- Fix TypeScript, lint, or build errors.
- Commit the completed step.
- Push to the remote repository if Git is configured.

Recommended validation commands:

```bash
npm run build
npm run lint
```

Suggested Git flow:

```bash
git add .
git commit -m "clear commit message"
git push origin main
```

---

## Step-by-Step Implementation Plan

### Step 1: Base Architecture and Mobile UI

Set up the project and create the first usable trip-planning screen.

Tasks:

- Set up or reuse Next.js, TypeScript, and Tailwind CSS
- Create mobile-first layout
- Add travel preference form
- Add mood filters at the top of the main screen
- Ensure all UI text is English

Suggested commit:

```bash
git add .
git commit -m "feat: setup mobile-first Busan planner UI"
git push origin main
```

### Step 2: Static Busan Places Dataset

Create a local dataset for Busan places.

Tasks:

- Add required Busan places
- Add latitude and longitude for each place
- Add area, mood tags, budget level, description, recommended reason, and estimated duration

Suggested commit:

```bash
git add .
git commit -m "feat: add static Busan places dataset"
git push origin main
```

### Step 3: Mood Filters and Recommendation Logic

Implement client-side filtering and recommendation logic.

Tasks:

- Filter places by mood
- Sort or prioritize by area focus, budget, starting point, and duration
- Display recommended places as readable cards

Suggested commit:

```bash
git add .
git commit -m "feat: add mood filters and recommendation logic"
git push origin main
```

### Step 4: Local Reverse Geocoding and Location Details

Implement clickable location behavior.

Tasks:

- Add a map-style layout or visual location panel
- Allow users to click a place or pin
- Find the closest place from the local dataset
- Show detailed location information
- Add optional browser geolocation and distance display if practical
- Gracefully handle location permission denial

Suggested commit:

```bash
git add .
git commit -m "feat: add local reverse geocoding location details"
git push origin main
```

### Step 5: Itinerary Panel

Allow users to save selected places.

Tasks:

- Add Add to Itinerary button
- Show selected places
- Remove selected places
- Prevent duplicates
- Show selected count and estimated total duration

Suggested commit:

```bash
git add .
git commit -m "feat: add itinerary panel"
git push origin main
```

### Step 6: Responsive Polish and README

Finish the MVP.

Tasks:

- Polish mobile and desktop layouts
- Improve spacing, typography, and button states
- Update README
- Run production build
- Fix all errors

Suggested commit:

```bash
git add .
git commit -m "chore: polish UI and update README"
git push origin main
```

---

## Optional Phase 2: AI and External Map Integration

Implement this only after the static MVP works.

### GPT Itinerary Recommendation API

Optional feature:

- Create `app/api/recommend/route.ts`
- Use `OPENAI_API_KEY`
- Return structured JSON itinerary recommendations
- Validate GPT output before rendering
- Provide static fallback JSON if GPT fails
- Do not expose API keys on the client side

Expected response shape:

```json
{
  "itineraryTitle": "1-Day Ocean Lover's Trip",
  "recommendations": [
    {
      "day": 1,
      "sequence": 1,
      "placeName": "Gwangalli Beach",
      "description": "Famous for the Gwangan Bridge view and beachside cafes.",
      "latitude": 35.1532,
      "longitude": 129.1189,
      "area": "East Busan",
      "estimatedCost": "Low",
      "recommendedDuration": "1.5 hours",
      "youtubeQuery": "Busan Gwangalli Beach vlog"
    }
  ]
}
```

### Kakao Maps Integration

Optional feature:

- Use `NEXT_PUBLIC_KAKAO_MAP_API_KEY`
- Show markers for recommended places
- Connect route stops with a polyline
- Gracefully fallback if the SDK or API key is unavailable

### YouTube Vlog Embed

Optional feature:

- Use each place's `youtubeQuery`
- Embed related YouTube search results or videos
- Keep this feature lightweight
- Ensure the main planner still works if embeds fail

### Instagram-Style Itinerary Card

Optional feature:

- Create a 1:1 visual summary card
- Include trip title, selected places, and a short theme sentence
- Use Tailwind CSS first
- Do not require image generation for the MVP

---

## Success Criteria

The project is successful when:

- The app runs locally without errors.
- The first screen is a usable trip planner.
- The UI is fully English.
- Users can select travel duration, budget, companion type, mood, starting point, and area focus.
- Users can filter places by travel mood.
- Users can click a location and see location details.
- Local reverse geocoding maps clicked coordinates or pins to known Busan places.
- Users can add and remove places from an itinerary.
- Duplicate itinerary items are prevented.
- At least 8 Busan places are included.
- The page looks complete on mobile and desktop.
- README contains install, run, and build commands.
- MVP does not require a backend server, database, paid API, or external API key.

---

## Constraints

### MVP Constraints

- Do not require a backend server.
- Do not require a database.
- Do not use paid APIs.
- Do not require Google Maps, Kakao Maps, Naver Maps, or OpenAI for the MVP.
- Use static local data for place information.
- Keep the implementation simple enough for a hackathon prototype.
- Keep the UI practical rather than overly decorative.

### Extended Version Constraints

- Store API keys in environment variables.
- Do not expose secret API keys on the client side.
- Validate GPT output before rendering.
- Provide fallback content when external APIs are unavailable.
- The user experience must remain functional without OpenAI, Kakao Maps, or YouTube.

---

## Environment Variables for Optional Phase 2

Only required for extended features.

```env
OPENAI_API_KEY=
NEXT_PUBLIC_KAKAO_MAP_API_KEY=
```

---

## Final Product Direction

The final product should feel like a practical Busan travel planning assistant.

It should not feel like a generic landing page.

The user should immediately be able to answer:

- How long am I staying?
- What is my budget?
- Who am I traveling with?
- What travel mood do I want?
- Where do I start?
- Do I want East Busan, West Busan, or a balanced route?
- Which place is this when I click the location?
- Which places should I add to my itinerary?

The app should make short-trip planning in Busan faster, lighter, and easier for foreign tourists.
