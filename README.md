# 🏥 MEDRIBA - Multi-Model Expert Data-driven Risk Identification & Bio-health Analytics

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-1.31.0-red.svg)](https://streamlit.io/)
[![XGBoost](https://img.shields.io/badge/XGBoost-2.0.3-orange.svg)](https://xgboost.readthedocs.io/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

## 📋 Overview

MEDRIBA is a cutting-edge, research-based AI healthcare application that predicts **Diabetes** and **Heart Disease** using state-of-the-art machine learning models. Built with Streamlit, it provides an intuitive, professional interface for healthcare predictions with confidence intervals and explainable AI.

### ✨ Key Features

- 🩸 **Diabetes Prediction** - Random Forest model with **96.27% accuracy**
- ❤️ **Heart Disease Prediction** - XGBoost model with **97.57% accuracy**
- 🔍 **SHAP Explainability** - Understand which factors influence predictions
- 📊 **Confidence Intervals** - Know how confident the model is
- 🎨 **Professional UI** - Beautiful gradient design with interactive visualizations
- 📈 **Real-time Analytics** - Feature importance and probability distributions
- 🔬 **Research-Based** - Implementation follows 10+ peer-reviewed papers

---

## 🚀 Quick Start

### Prerequisites

- Python 3.8 or higher
- pip package manager

### Installation

1. **Clone or download the repository**

```bash
cd medriba
```

2. **Install dependencies**

```bash
pip install -r requirements.txt
```

3. **Ensure all model files are in the project directory**

Required files:
```
medriba/
├── medriba_app.py
├── requirements.txt
├── diabetes_rf_model.pkl
├── diabetes_scaler.pkl
├── diabetes_selected_features.pkl
├── heart_xgb_model.pkl
├── heart_scaler.pkl
└── heart_selected_features.pkl
```

4. **Run the application**

```bash
streamlit run medriba_app.py
```

5. **Open your browser**

The app will automatically open at `http://localhost:8501`

---

## 📂 File Structure

```
medriba/
│
├── medriba_app.py                      # Main Streamlit application
├── requirements.txt                    # Python dependencies
├── README.md                           # Documentation
│
├── Diabetes_Prediction_RF.ipynb        # Diabetes model training notebook
├── Heart_Disease_Complete.ipynb        # Heart disease model training notebook
│
├── diabetes_rf_model.pkl               # Trained Random Forest model
├── diabetes_scaler.pkl                 # Diabetes data scaler
├── diabetes_selected_features.pkl      # Selected diabetes features
│
├── heart_xgb_model.pkl                 # Trained XGBoost model
├── heart_scaler.pkl                    # Heart disease data scaler
└── heart_selected_features.pkl         # Selected heart features
```

---
### Create a `.env` file in the root directory and add your Pinecone & openai credentials as follows:

```ini
GEMINI_API_KEY = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

## 🎯 Model Performance

### Diabetes Prediction Model
- **Algorithm:** Random Forest Classifier
- **Accuracy:** 96.27%
- **Features:** 14 (including engineered features)
- **Cross-Validation:** 5-Fold Stratified
- **Class Balancing:** SMOTE
- **Based on:** Research Paper 4

### Heart Disease Prediction Model
- **Algorithm:** XGBoost Classifier
- **Accuracy:** 97.57%
- **Features:** 20 (including engineered features)
- **Cross-Validation:** 5-Fold Stratified
- **Class Balancing:** SMOTE
- **Based on:** Research Paper 5

---

## 💡 Usage Guide

### Home Page
- Overview of both prediction models
- Key statistics and features
- Navigation to prediction pages

### Diabetes Prediction
1. Navigate to "🩸 Diabetes Prediction"
2. Enter patient information:
   - Number of pregnancies
   - Glucose level
   - Blood pressure
   - Skin thickness
   - Insulin level
   - BMI
   - Diabetes pedigree function
   - Age
3. Click "🔬 Analyze Diabetes Risk"
4. View results:
   - Risk prediction (High/Low)
   - Confidence level with gauge
   - Probability distribution
   - Top contributing factors

### Heart Disease Prediction
1. Navigate to "❤️ Heart Disease Prediction"
2. Enter patient information:
   - Age, sex, chest pain type
   - Blood pressure, cholesterol
   - ECG results
   - Maximum heart rate
   - Exercise data
   - ST depression, slope
   - Major vessels, thalassemia
3. Click "💓 Analyze Heart Health"
4. View results:
   - Risk prediction (High/Low)
   - Confidence level
   - Probability distribution
   - Key risk factors

### About Models
- Detailed model specifications
- Research compliance checklist
- Feature descriptions
- DOs and DON'Ts implemented

---

## 🔬 Research Compliance

### ✅ DOs Implemented

1. ✓ **Optimal Algorithms**
   - XGBoost for heart disease (97.57%)
   - Random Forest for diabetes (96.27%)

2. ✓ **SHAP Explainability**
   - Feature importance visualization
   - Model interpretability

3. ✓ **Feature Engineering**
   - Interaction features
   - Domain knowledge integration

4. ✓ **Feature Selection**
   - SelectKBest with ANOVA F-test
   - 3-5% accuracy improvement

5. ✓ **Cross-Validation**
   - 5-fold stratified CV
   - Reliable performance estimation

6. ✓ **Class Balancing**
   - SMOTE technique
   - Handles imbalanced datasets

7. ✓ **Confidence Intervals**
   - Probability distributions
   - Prediction confidence scores

8. ✓ **Realistic Accuracy**
   - 95-97% target (not 100%)
   - Evidence-based claims

9. ✓ **Ensemble Comparison**
   - Multiple model evaluation
   - Best algorithm selection

10. ✓ **External Validation Ready**
    - Multi-dataset support
    - Generalization capability

### ❌ DON'Ts Avoided

- ✗ No 100% accuracy claims
- ✗ No single dataset reliance
- ✗ No feature engineering skip
- ✗ No class imbalance ignore
- ✗ No interpretability omission
- ✗ No confidence interval skip
- ✗ No overly complex methods (federated learning)
- ✗ No temporal models without temporal data

---

## 🛠️ Technical Stack

| Component | Technology |
|-----------|-----------|
| **Frontend** | Streamlit |
| **ML Framework** | scikit-learn, XGBoost |
| **Data Processing** | pandas, NumPy |
| **Visualization** | Plotly, Matplotlib, Seaborn |
| **Explainability** | SHAP |
| **Class Balancing** | imbalanced-learn (SMOTE) |

---

## 📊 Features Explained

### Diabetes Features
- **Pregnancies:** Number of times pregnant
- **Glucose:** Plasma glucose concentration (mg/dL)
- **Blood Pressure:** Diastolic blood pressure (mm Hg)
- **Skin Thickness:** Triceps skin fold thickness (mm)
- **Insulin:** 2-hour serum insulin (μU/mL)
- **BMI:** Body mass index (weight/height²)
- **Diabetes Pedigree Function:** Genetic diabetes likelihood
- **Age:** Age in years
- **Engineered Features:** Glucose×BMI, Age×BMI, etc.

### Heart Disease Features
- **Age:** Age in years
- **Sex:** Male/Female
- **Chest Pain Type:** 4 categories
- **Resting BP:** Resting blood pressure (mm Hg)
- **Cholesterol:** Serum cholesterol (mg/dL)
- **Fasting Blood Sugar:** >120 mg/dL (Yes/No)
- **Resting ECG:** 3 categories
- **Max Heart Rate:** Maximum heart rate achieved
- **Exercise Angina:** Exercise-induced angina (Yes/No)
- **ST Depression:** Induced by exercise
- **Slope:** Slope of peak exercise ST segment
- **Major Vessels:** 0-3 vessels colored by fluoroscopy
- **Thalassemia:** Blood disorder status
- **Engineered Features:** Age×Cholesterol, Risk scores, etc.

---

## 🎨 UI Features

- **Gradient Theme:** Professional purple-blue gradient
- **Responsive Design:** Works on desktop and tablet
- **Interactive Charts:** Plotly visualizations
- **Confidence Gauge:** Visual confidence meter
- **Feature Importance:** Top contributing factors
- **Probability Distribution:** Clear risk visualization
- **Sidebar Navigation:** Easy page switching
- **Model Statistics:** Real-time performance metrics

---

## 🔮 Future Enhancements

- [ ] Add more disease prediction models
- [ ] Implement user authentication
- [ ] Store prediction history
- [ ] Add PDF report generation
- [ ] Integrate with hospital systems
- [ ] Mobile app version
- [ ] Real-time biomarker integration
- [ ] Multi-language support

---

## 📝 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

## 👨‍💻 Developer

**itxkabix**
- MCA Student specializing in Data Science
- Research-based implementation following 10+ peer-reviewed papers
- Focus on explainable AI and production-ready solutions

---

## ⚠️ Disclaimer

**IMPORTANT:** This application is for educational and research purposes only. It should NOT be used as a substitute for professional medical advice, diagnosis, or treatment. Always seek the advice of qualified healthcare providers with any questions regarding medical conditions.

The predictions made by this system are based on machine learning models trained on historical data and may not be 100% accurate for all cases. Clinical judgment should always take precedence.

---

## 🙏 Acknowledgments

- Research papers that guided the implementation
- Scikit-learn and XGBoost communities
- Streamlit team for the amazing framework
- Healthcare datasets providers
- Open-source ML community

---

## 📧 Contact

For questions, suggestions, or collaborations:
- GitHub: [@itxkabix](https://github.com/itxkabix)
- Project: MEDRIBA v1.0

---

## 🌟 Star this repository if you find it useful!

Made with ❤️ for better healthcare through AI
