# AgriSaarthi - System Design & Architecture

This design doc explains how AgriSaarthi works, what technologies power
it, how data flows through the app, and how the major components
interact to deliver a unified farmer support platform.

## High-Level Architecture:

``` bash
+----------------------------+
|      User Browser / UI     |
|  (React + Tailwind CSS)    |
+-------------+--------------+
              |
              v HTTPS
+-------------+--------------+
|   Frontend SPA (React & TS)|
|                            |
|  Voice UI, Navigation,      |
|  Client API Calls           |
+-------------+--------------+
              |
              v REST / SDK
+-------------+--------------+
|         Backends & APIs     |
| - Firebase Auth             |
| - Firebase Realtime Database|
| - Weather & Price APIs      |
| - AI Voice via Vapi.ai      |
+-------------+--------------+
              |
              v Data Fetch / Streaming
+-------------+--------------+
| External Data Sources       |
| - Weather providers         |
| - Mandi price feeds         |
| - AI Vision / Disease detect|
+----------------------------+
```

This layered setup separates UI logic, data layer, and external
services, making it easier to extend and scale features over time.

## Frontend (Client) -- Tech & Responsibilities

### 🛠 Technology Stack:

-   React (with TypeScript): For building a modern, fast, interactive
    single-page application (SPA).
-   Tailwind CSS: Provides utility-first styling that makes UI
    responsive and clean.
-   Framer Motion: Used for smooth animations and engaging UI
    transitions.
-   React Router DOM : Manages page navigation without full reloads.
-   React Context API : Handles global state like authentication status,
    selected language, and user profile.
-   Icons & UI assets (Lucide React): For consistent, scalable
    iconography.

### Responsibilities:

**Frontend handles:**\
\* Page layout and UI elements \* Voice & text input capture \* Routing
between features (AgriBot, Weather, Mandi Prices, etc.) \* Authorization
state & session \* Sending requests to backend or external services \*
Rendering results (text + voice + visuals) to users

Everything runs client-side first, connecting via APIs or SDKs to
backend features.

### Backend & Service Integrations:

AgriSaarthi doesn't run a traditional custom server --- instead, it
builds on cloud services and third-party platforms:

##### 🔐 Authentication & Database

-   Firebase Authentication: Handles user logins securely with
    email/password or phone auth.
-   Firebase Realtime Database: Stores user profiles, settings, session
    data, and any persistent application state.

#### AI & Conversational Engine:

-   **Vapi.ai Web SDK :** Provides an AI assistant --- AgriBot, that
    understands voice queries and generates responses. This handles
    speech-to-text + AI answers + text-to-speech in supported languages.

This means instead of building own NLP pipeline from scratch,
AgriSaarthi reuses an existing conversational AI platform optimized for
real-time interaction.

#### External Data APIs:

Although not hardcoded, AgriSaarthi's features point to integration
with: \* Weather APIs --- to fetch local forecasts when the user selects
"Weather." \* Mandi Price Feeds --- to display real-time/near-real-time
crop pricing data. \* Machine Vision or Crop Detection AI --- for
pest/disease detection via image upload (front end captures images and
sends them to an AI service)

These may use REST endpoints, 3rd-party SDKs, or public APIs depending
on provider.

## Feature Workflow:

#### 1. AgriBot AI:

-   User taps Chat Now on the homepage.
-   Voice audio captured via browser/Web Audio API.
-   Audio sent to Vapi.ai SDK → STT (speech-to-text).
-   Transcribed query is passed to AI assistant.
-   AI sends back text response.
-   Text converted to voice (TTS) & played.
-   Display transcript & AI reply in UI.

This entire pipeline occurs without your own backend AI server, keeping
the architecture light.

#### 2. Weather Widget:

-   User selects Weather widget.
-   Frontend requests weather API with location (likely from user input
    or GPS).
-   Weather provider returns forecast JSON.
-   UI renders forecast, temperatures, rain alerts, etc.

This is a classic API consumption flow, with frontend handling both
request and rendering.

### 📁 Data Flow:

User action → Frontend event handler → one of:

-   Firebase request (auth or persistent data)
-   Third-party API call (weather, prices, vision)
-   AI SDK call (Vapi.ai for voice Q&A)

Response → Frontend → Render (and optionally cache)

------------------------------------------------------------------------

## 🚁 Drone-Based Monitoring System

### Overview

The AgriSaarthi platform integrates an advanced drone monitoring system
to enhance precision agriculture. Drones are equipped with multispectral
cameras and sensors to collect real-time aerial data of crops.

### Key Features

-   Crop health monitoring using NDVI imaging\
-   Pest and disease detection through AI-based image processing\
-   Automated pesticide and fertilizer spraying\
-   Soil moisture mapping from aerial thermal imaging\
-   Real-time farm surveillance

### Working Mechanism

1.  Drones capture high-resolution aerial images.\
2.  Images are processed using AI models.\
3.  Crop health reports are generated instantly.\
4.  Farmers receive alerts and recommendations via the AgriSaarthi app.

### Benefits

-   Reduces manual labor\
-   Early detection of crop stress\
-   Optimized input usage\
-   Increased yield and productivity

------------------------------------------------------------------------

## 🌐 IoT-Based Smart Farming System

### Overview

AgriSaarthi uses IoT-enabled smart sensors deployed across the farmland
to continuously monitor environmental and soil conditions.

### IoT Components

-   Soil moisture sensors\
-   Temperature and humidity sensors\
-   pH level sensors\
-   Weather monitoring stations\
-   Smart irrigation controllers

### System Architecture

1.  Sensors collect real-time data from the field.\
2.  Data is transmitted to a cloud server via wireless networks
    (LoRa/WiFi/GSM).\
3.  AI algorithms analyze the data.\
4.  Farmers receive actionable insights on irrigation, fertilization,
    and crop care.

### Smart Automation Features

-   Automated drip irrigation control\
-   Water usage optimization\
-   Real-time alerts for abnormal conditions\
-   Data-driven decision support

### Benefits

-   Water conservation\
-   Reduced resource wastage\
-   Precision farming\
-   Improved sustainability

------------------------------------------------------------------------

## 🔗 Integrated Workflow (IoT + Drone + AI)

1.  IoT sensors monitor ground-level data continuously.\
2.  Drones provide aerial insights periodically.\
3.  AI system fuses both datasets for accurate analysis.\
4.  Farmers receive predictive analytics and smart recommendations.

This integrated ecosystem ensures data-driven, efficient, and
sustainable farming practices under the AgriSaarthi platform.
