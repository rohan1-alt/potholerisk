<div align="center">

# 🕳️ PotholeRisk

### Bengaluru Dynamic Risk Engine

**Real-time weather + road conditions + machine learning, turned into a driving-risk score for Bengaluru.**

[![Live Demo](https://img.shields.io/badge/🚀_Live_Demo-potholerisk.onrender.com-00c853?style=for-the-badge)](https://potholerisk.onrender.com)

![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat-square&logo=nodedotjs&logoColor=white)
![Express](https://img.shields.io/badge/Express-000000?style=flat-square&logo=express&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![XGBoost](https://img.shields.io/badge/XGBoost-EB5B00?style=flat-square)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=flat-square&logo=jupyter&logoColor=white)
![OpenWeather](https://img.shields.io/badge/OpenWeather-EB6E4B?style=flat-square)
![Deployed on Render](https://img.shields.io/badge/Render-46E3B7?style=flat-square&logo=render&logoColor=black)

</div>

---

## 📌 What is PotholeRisk?

Bengaluru's roads change with every monsoon shower. **PotholeRisk** estimates how dangerous a stretch of road is *right now* by combining:

- 🌦️ **Live weather** (rain, visibility, humidity, temperature) from OpenWeather
- 🛣️ **Road and pothole conditions**
- 🚦 **Traffic exposure**
- 🧠 **A model trained on 49,932 real Bengaluru accident records (2016–2023)**

The result is a clear **LOW / MEDIUM / HIGH** risk reading, served through a fast, responsive web app.

---

## ✨ Features

| | Feature | Details |
|---|---|---|
| 🌤️ | **Live weather integration** | Pulls current conditions for any Bengaluru coordinate |
| 🔐 | **Secure API proxy** | OpenWeather key lives only on the server, never in the browser |
| ⚡ | **Dynamic risk engine** | Risk score updates with weather, traffic and road inputs |
| 📱 | **Responsive UI** | Works on desktop and mobile |
| 🤖 | **ML severity model** | XGBoost classifier for High-Risk vs Lower-Risk accidents |
| 📊 | **Reproducible research** | Full notebooks, from data exploration to final results |

---

## 🏗️ Architecture

```text
┌──────────┐   GET /api/weather    ┌────────────────┐        ┌─────────────────┐
│ Browser  │ ────────────────────▶ │ Express Server │ ─────▶ │ OpenWeather API │
│ (UI)     │ ◀──── Weather JSON ── │  (API key 🔑)  │ ◀───── │                 │
└──────────┘                       └────────────────┘        └─────────────────┘
```

- The frontend talks **only** to your backend.
- `server.js` injects the API key server-side.
- The key never appears in frontend code, DevTools, or network requests.

---

## 🧠 The ML Behind It

### Real-world accident severity model

Predicts whether a Bengaluru accident is **High-Risk** (fatal or grievous injury) or **Lower-Risk** (simple injury or damage only), using weather, road and surface condition, collision type, junction control, police-station area and year.

| Metric | Score |
|---|---|
| Accuracy | **72.6%** |
| Precision (weighted) | 72.3% |
| Recall (weighted) | 72.6% |
| F1 (weighted) | 71.8% |

*Held-out 20% test set, stratified, `random_state=42`. Data: Karnataka State Police records, 2016–2023.*

**How we got here**

1. Fixed the evaluation by adding a proper train/test split, after finding the original scores were inflated by class imbalance
2. Reframed 5-class severity as a binary High-Risk vs Lower-Risk problem
3. Replaced integer label encoding with one-hot encoding
4. Added unused real features such as collision type, junction control and road character
5. Added leakage-safe target encoding of police-station area
6. Tuned class-balanced XGBoost

> 💡 **Honest finding:** performance plateaued at this level. Location and year carry more signal than weather or road surface. Vehicle speed, vehicle type and driver behavior, none of which are in the dataset, are likely needed to go higher.

### Synthetic formula-consistency check

A separate check confirms the DBN risk formulation is internally consistent. A classifier recovers its own LOW / MEDIUM / HIGH categories from raw inputs with **93.6% accuracy**.

> ⚠️ This is **not** a real-world accuracy figure. The labels come from the same formula the classifier learns. The real result is the 72.6% above.

---

## 🛠️ Tech Stack

| Layer | Tools |
|---|---|
| **Frontend** | HTML, CSS, JavaScript |
| **Backend** | Node.js, Express.js, dotenv, node-fetch |
| **External API** | OpenWeather |
| **ML / Data** | Python, pandas, scikit-learn, XGBoost, Jupyter |
| **Hosting** | Render |

---

## 🚀 Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/AnushreeKasturi/Potholerisk.git
cd Potholerisk
```

### 2. Install dependencies

```bash
npm install
```

### 3. Add your API key

Get a free key at [openweathermap.org](https://openweathermap.org/api), then:

```bash
cp .env.example .env
```

Edit `.env`:

```env
OPENWEATHER_API_KEY=your_api_key_here
# PORT=3000   (optional)
```

### 4. Run it

```bash
npm start
```

Open **http://localhost:3000** 🎉

For auto-reload during development, run `npm run dev`.

---

## 📂 Project Structure

```text
Potholerisk/
├── public/                  # Frontend (index.html)
├── server.js                # Express server + secure weather proxy
├── package.json
├── .env.example             # Environment template
│
├── notebooks/               # Research pipeline
│   ├── 01_Data_Exploration.ipynb
│   ├── 02_Data_Preprocessing.ipynb
│   ├── 03_Feature_Engineering.ipynb
│   ├── 04_Baseline_Models_Clean.ipynb
│   ├── 05_Bayesian_Risk_Model_Final.ipynb
│   └── 06_Results_Visualization.ipynb
│
├── models_final/            # Final trained model + metrics + training script
├── models_improved/         # Experiments with model improvements
├── synthetic_validation/    # Formula-consistency check (not real accuracy)
├── results/                 # Plots (ROC curve, etc.)
└── dataset/                 # Traffic, weather and processed data
```

---

## 🔮 Roadmap

- [ ] Live pothole detection using computer vision
- [ ] Accident probability prediction per road segment
- [ ] Route safety scoring
- [ ] Interactive GIS map with risk heatmaps
- [ ] User pothole reporting system

---

## 🔒 Security Notes

- `.env` is gitignored and never committed
- All OpenWeather calls go through the server
- If you think a key leaked, regenerate it in your OpenWeather dashboard

---

## 👥 Team

| Name | GitHub |
|---|---|
| **Rohan Anand** |  [@rohan1-alt](https://github.com/rohan1-alt)|
| **Anushree Kasturi** | [@AnushreeKasturi](https://github.com/AnushreeKasturi) |
| **M. Tejas** | |

---

<div align="center">

**Built for safer Bengaluru roads 🛣️**

⭐ If you found this useful, give the repo a star!

</div>
