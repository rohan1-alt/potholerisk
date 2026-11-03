# PotholeRisk — Bengaluru Dynamic Risk Engine

A full-stack web application that analyzes road and weather conditions to estimate pothole-related driving risk across Bengaluru.

🚀 Live Demo

https://potholerisk.onrender.com

## Features

* Real-time weather integration using OpenWeather API
* Secure backend API proxy using Node.js & Express
* Dynamic risk analysis engine
* Responsive frontend UI
* Environment variable protection using `.env`
* API key hidden from the browser and DevTools

---

## Tech Stack

* Frontend: HTML, CSS, JavaScript
* Backend: Node.js, Express.js
* API: OpenWeather API

---

## Security

The OpenWeather API key is stored securely on the server using environment variables.

The browser never directly accesses the API key.

### Architecture

```text
Browser  ──GET /api/weather──▶  Express Server  ──▶ OpenWeather API
         ◀── Weather JSON ───                 ◀── Weather JSON
```

* The frontend communicates only with the backend server
* `server.js` injects the API key securely
* The API key is never exposed in frontend code or network requests

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/AnushreeKasturi/Potholerisk.git
cd Potholerisk
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Configure Environment Variables

Create a `.env` file from the template:

```bash
cp .env.example .env
```

Add your OpenWeather API key:

```env
OPENWEATHER_API_KEY=your_api_key_here
```

---

## Running the Project

```bash
npm start
```

Open in browser:

```text
http://localhost:3000
```

---

## Project Structure

```text
Potholerisk/
├── server.js
├── package.json
├── .env
├── .env.example
├── .gitignore
└── public/
    └── index.html
```

---

## Future Improvements

* Live pothole detection using computer vision
* Accident probability prediction
* Route safety scoring
* Interactive GIS mapping
* User reporting system

---

## Author

Anushree Kasturi
