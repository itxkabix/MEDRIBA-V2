# 🤖 MEDRIBA Medical Chatbot Assistant - Code Section for medriba_app.py

## 📍 WHERE TO ADD THIS CODE

Add this code section in your `medriba_app.py` file **BEFORE** the "About Models" section (around line 850-900).

---

## 🔧 STEP 1: Add Imports at the TOP of medriba_app.py

```python
# Add these imports after your existing imports (around line 10-15)
import google.generativeai as genai
from datetime import datetime
import os
from dotenv import load_dotenv

# Load environment variables
load_dotenv()
GEMINI_API_KEY = os.getenv('GEMINI_API_KEY', '')
```

---

## 🔧 STEP 2: Update Sidebar Navigation

Find the sidebar navigation section (around line 350) and update it:

```python
# FIND THIS CODE:
selected = option_menu(
    menu_title=None,
    options=["🏠 Home", "🩸 Diabetes Prediction", "❤️ Heart Disease Prediction", "📊 About Models"],
    icons=["house", "activity", "heart", "info-circle"],
    ...
)

# REPLACE WITH THIS:
selected = option_menu(
    menu_title=None,
    options=[
        "🏠 Home", 
        "🩸 Diabetes Prediction", 
        "❤️ Heart Disease Prediction", 
        "💬 Medical Assistant",  # ← NEW!
        "📊 About Models"
    ],
    icons=["house", "activity", "heart", "chat-dots", "info-circle"],  # ← Added "chat-dots"
    menu_icon="cast",
    default_index=0,
    styles={
        "container": {"padding": "0!important", "background-color": "#1e3a8a"},
        "icon": {"color": "white", "font-size": "20px"},
        "nav-link": {
            "font-size": "16px",
            "text-align": "left",
            "margin": "0px",
            "color": "white",
            "--hover-color": "#3b82f6"
        },
        "nav-link-selected": {"background-color": "#667eea"},
    }
)
```

---

## 🔧 STEP 3: Add Gemini Helper Functions

Add these functions AFTER the load_models() function (around line 120):

```python
# ===========================
# GEMINI AI HELPER FUNCTIONS
# ===========================

def configure_gemini(api_key):
    """Configure Gemini AI with API key"""
    try:
        genai.configure(api_key=api_key)
        return True
    except Exception as e:
        st.error(f"Error configuring Gemini: {e}")
        return False

def create_medical_chat_session(api_key):
    """Create a Gemini chat session with medical assistant persona"""
    try:
        genai.configure(api_key=api_key)
        
        model = genai.GenerativeModel(
            model_name="gemini-1.5-pro",
            generation_config={
                "temperature": 0.7,
                "top_p": 0.95,
                "top_k": 40,
                "max_output_tokens": 2048,
            },
            system_instruction="""You are MEDRIBA Medical Assistant, an expert AI healthcare advisor specializing in:

1. **Personalized Diet Plans**: Nutrition advice for diabetes, heart disease, weight management, and other conditions
2. **Exercise Routines**: Safe, effective workout plans tailored to health conditions and fitness levels
3. **Yoga & Meditation**: Specific asanas, pranayama, and meditation techniques for various health concerns
4. **General Medical Advice**: Evidence-based health information, lifestyle recommendations, and preventive care

**Your Approach**:
- Provide evidence-based, scientific recommendations
- Always prioritize patient safety
- Be empathetic, supportive, and encouraging
- Tailor advice to individual health conditions
- Include appropriate disclaimers for serious conditions
- Suggest consulting healthcare professionals when needed
- Explain medical concepts in simple, understandable language

**Your Tone**: Professional, caring, informative, and motivating
**Your Goal**: Empower users to make informed health decisions and improve their wellbeing"""
        )
        
        chat = model.start_chat(history=[])
        return chat
    except Exception as e:
        st.error(f"Error creating chat session: {e}")
        return None

def get_ai_response(chat, user_message, health_context=None):
    """Get response from Gemini with optional health context from predictions"""
    try:
        # Add health context if available from predictions
        if health_context:
            context_msg = f"""
**Patient Health Context**:
- Diabetes Risk Assessment: {health_context.get('diabetes_risk', 'Not assessed')}
- Heart Disease Risk: {health_context.get('heart_risk', 'Not assessed')}
- Age: {health_context.get('age', 'Not provided')} years
- BMI: {health_context.get('bmi', 'Not provided')}
- Recent Predictions: {health_context.get('predictions', 'None')}

**User Question**: {user_message}

Please provide personalized advice considering the above health context.
"""
            response = chat.send_message(context_msg)
        else:
            response = chat.send_message(user_message)
        
        return response.text
    except Exception as e:
        return f"❌ Error: {str(e)}\n\nPlease check your API key and try again."

# Quick prompt templates
QUICK_HEALTH_PROMPTS = {
    "🍎 Diet & Nutrition": {
        "Diabetic Diet Plan": "Create a comprehensive 7-day meal plan for someone with diabetes, including breakfast, lunch, dinner, and snacks with portion sizes and nutritional information.",
        "Heart-Healthy Diet": "Design a heart-healthy diet plan to lower cholesterol and blood pressure, with specific foods to include and avoid.",
        "Weight Loss Plan": "Suggest a balanced, sustainable weight loss meal plan with calorie tracking and portion control strategies.",
        "Low-Carb Diet": "Explain a low-carb diet approach for diabetes management with meal examples and carb counting tips."
    },
    "🏃 Exercise & Fitness": {
        "Cardio for Heart Health": "Recommend safe cardiovascular exercises for someone at risk of heart disease, with intensity levels and duration.",
        "Diabetes Exercise Routine": "Create a weekly exercise plan specifically designed for diabetes management and blood sugar control.",
        "Beginner Workout Plan": "Design a beginner-friendly full-body workout routine with step-by-step instructions.",
        "Walking Program": "Develop a progressive walking program for improving cardiovascular health over 12 weeks."
    },
    "🧘 Yoga & Meditation": {
        "Yoga for Diabetes": "List specific yoga asanas beneficial for diabetes management with detailed instructions and benefits.",
        "Heart Health Yoga": "Recommend yoga poses and breathing exercises for cardiovascular health and stress reduction.",
        "Morning Yoga Routine": "Create a 20-minute energizing morning yoga sequence with pose descriptions.",
        "Meditation for Stress": "Guide me through meditation techniques for reducing stress and improving mental health."
    },
    "💊 General Health": {
        "Blood Sugar Management": "Provide lifestyle tips and natural methods for managing blood sugar levels effectively.",
        "Heart Disease Prevention": "What daily habits can help prevent heart disease and improve cardiovascular health?",
        "Healthy Sleep Habits": "How can I improve my sleep quality for better overall health outcomes?",
        "Hydration Guidelines": "What are the optimal hydration strategies for someone with diabetes or heart disease?"
    }
}
```

---

## 🔧 STEP 4: Add Medical Assistant Page

Add this ENTIRE section BEFORE the "About Models" page (around line 850):

```python
    # ===========================
    # MEDICAL ASSISTANT PAGE (CHATBOT)
    # ===========================
    elif selected == "💬 Medical Assistant":
        st.markdown("## 💬 MEDRIBA Medical Assistant")
        st.markdown("### AI-Powered Health Guidance with Gemini")
        
        # Info banner
        st.info("""
        🤖 **Your Personal Health Advisor**
        
        Get personalized advice on diet plans, exercise routines, yoga, meditation, and general health questions.
        Powered by Google Gemini AI.
        """)
        
        # API Key Configuration
        col1, col2 = st.columns([3, 1])
        
        with col1:
            api_key_input = st.text_input(
                "🔑 Enter Your Gemini API Key",
                value=GEMINI_API_KEY,
                type="password",
                help="Get your free API key from Google AI Studio",
                placeholder="AIzaSy..."
            )
        
        with col2:
            st.markdown("###")
            if st.button("🔗 Get API Key", use_container_width=True):
                st.markdown("[Get Free API Key →](https://makersuite.google.com/app/apikey)")
        
        if not api_key_input:
            st.warning("⚠️ **API Key Required**")
            st.markdown("""
            **How to get your FREE Gemini API key:**
            
            1. Visit: **https://makersuite.google.com/app/apikey**
            2. Sign in with your Google account
            3. Click **"Create API Key"**
            4. Copy the key and paste it above
            
            ✨ **Free tier includes:**
            - 60 requests per minute
            - Unlimited usage for personal projects
            - No credit card required
            """)
            st.stop()
        
        # Initialize chat session
        if 'chat_session' not in st.session_state or st.session_state.get('last_api_key') != api_key_input:
            with st.spinner("🔄 Initializing AI Medical Assistant..."):
                st.session_state.chat_session = create_medical_chat_session(api_key_input)
                st.session_state.last_api_key = api_key_input
                st.session_state.chat_history = []
                
                if st.session_state.chat_session:
                    st.success("✅ Medical Assistant ready!")
                else:
                    st.error("❌ Failed to initialize. Please check your API key.")
                    st.stop()
        
        # Quick Topic Buttons
        st.markdown("---")
        st.markdown("### 🎯 Quick Health Topics")
        st.markdown("Click any button below to get instant guidance:")
        
        tab1, tab2, tab3, tab4 = st.tabs(["🍎 Diet & Nutrition", "🏃 Exercise & Fitness", "🧘 Yoga & Meditation", "💊 General Health"])
        
        with tab1:
            cols = st.columns(2)
            for idx, (label, prompt) in enumerate(QUICK_HEALTH_PROMPTS["🍎 Diet & Nutrition"].items()):
                with cols[idx % 2]:
                    if st.button(f"📋 {label}", key=f"diet_{idx}", use_container_width=True):
                        st.session_state.pending_prompt = prompt
        
        with tab2:
            cols = st.columns(2)
            for idx, (label, prompt) in enumerate(QUICK_HEALTH_PROMPTS["🏃 Exercise & Fitness"].items()):
                with cols[idx % 2]:
                    if st.button(f"💪 {label}", key=f"exercise_{idx}", use_container_width=True):
                        st.session_state.pending_prompt = prompt
        
        with tab3:
            cols = st.columns(2)
            for idx, (label, prompt) in enumerate(QUICK_HEALTH_PROMPTS["🧘 Yoga & Meditation"].items()):
                with cols[idx % 2]:
                    if st.button(f"🧘‍♀️ {label}", key=f"yoga_{idx}", use_container_width=True):
                        st.session_state.pending_prompt = prompt
        
        with tab4:
            cols = st.columns(2)
            for idx, (label, prompt) in enumerate(QUICK_HEALTH_PROMPTS["💊 General Health"].items()):
                with cols[idx % 2]:
                    if st.button(f"🏥 {label}", key=f"health_{idx}", use_container_width=True):
                        st.session_state.pending_prompt = prompt
        
        # Chat Interface
        st.markdown("---")
        st.markdown("### 💬 Chat with Medical Assistant")
        
        # Display chat history
        chat_container = st.container()
        with chat_container:
            for idx, msg in enumerate(st.session_state.get('chat_history', [])):
                if msg['role'] == 'user':
                    st.markdown(f"""
                    <div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); 
                                padding: 15px; border-radius: 15px; margin: 10px 0; 
                                color: white; max-width: 80%; margin-left: auto;">
                        <strong>👤 You</strong><br>
                        {msg['content']}
                        <div style="text-align: right; font-size: 0.75em; opacity: 0.8; margin-top: 5px;">
                            {msg['timestamp'].strftime('%I:%M %p')}
                        </div>
                    </div>
                    """, unsafe_allow_html=True)
                else:
                    st.markdown(f"""
                    <div style="background: #f1f5f9; padding: 15px; border-radius: 15px; 
                                margin: 10px 0; color: #1e3a8a; max-width: 80%; border-left: 4px solid #667eea;">
                        <strong>🤖 MEDRIBA Assistant</strong><br>
                        {msg['content'].replace(chr(10), '<br>')}
                        <div style="text-align: left; font-size: 0.75em; color: #64748b; margin-top: 5px;">
                            {msg['timestamp'].strftime('%I:%M %p')}
                        </div>
                    </div>
                    """, unsafe_allow_html=True)
        
        # User Input
        user_question = st.text_area(
            "💭 Ask your health question:",
            value=st.session_state.get('pending_prompt', ''),
            placeholder="e.g., What diet should I follow to manage my diabetes?",
            height=100,
            key="user_input_field"
        )
        
        col1, col2, col3 = st.columns([1, 1, 4])
        
        with col1:
            send_clicked = st.button("📤 Send Message", use_container_width=True, type="primary")
        
        with col2:
            clear_clicked = st.button("🗑️ Clear Chat", use_container_width=True)
        
        # Handle send
        if send_clicked and user_question.strip():
            # Add user message
            st.session_state.chat_history.append({
                'role': 'user',
                'content': user_question,
                'timestamp': datetime.now()
            })
            
            # Get AI response
            with st.spinner("🤔 MEDRIBA is thinking..."):
                # Get health context if available from previous predictions
                health_context = st.session_state.get('health_context', None)
                
                ai_response = get_ai_response(
                    st.session_state.chat_session,
                    user_question,
                    health_context
                )
            
            # Add AI response
            st.session_state.chat_history.append({
                'role': 'assistant',
                'content': ai_response,
                'timestamp': datetime.now()
            })
            
            # Clear pending prompt
            if 'pending_prompt' in st.session_state:
                del st.session_state.pending_prompt
            
            st.rerun()
        
        # Handle clear
        if clear_clicked:
            st.session_state.chat_history = []
            if 'pending_prompt' in st.session_state:
                del st.session_state.pending_prompt
            st.rerun()
        
        # Export Chat
        if st.session_state.get('chat_history'):
            st.markdown("---")
            if st.button("💾 Export Chat History"):
                chat_text = "MEDRIBA Medical Assistant - Chat History\\n"
                chat_text += "=" * 50 + "\\n\\n"
                for msg in st.session_state.chat_history:
                    chat_text += f"{msg['role'].upper()} [{msg['timestamp'].strftime('%Y-%m-%d %I:%M %p')}]:\\n"
                    chat_text += f"{msg['content']}\\n\\n"
                
                st.download_button(
                    label="📥 Download as Text File",
                    data=chat_text,
                    file_name=f"medriba_chat_{datetime.now().strftime('%Y%m%d_%H%M%S')}.txt",
                    mime="text/plain"
                )
        
        # Medical Disclaimer
        st.markdown("---")
        st.warning("""
        **⚕️ Important Medical Disclaimer**
        
        This AI assistant provides general health information and suggestions based on medical knowledge. 
        It is **NOT a substitute** for professional medical advice, diagnosis, or treatment.
        
        **Always consult qualified healthcare providers** for:
        - Medical diagnosis
        - Treatment decisions
        - Medication advice
        - Emergency situations
        
        If you're experiencing a medical emergency, call your local emergency number immediately.
        """)
```

---

## 📝 SUMMARY

**Add to medriba_app.py:**
1. ✅ Imports at top (Step 1)
2. ✅ Updated sidebar navigation (Step 2)
3. ✅ Helper functions after load_models() (Step 3)
4. ✅ Complete chatbot page (Step 4)

**Total additions:** ~300 lines of code

**Next:** See the companion files I'll create next!
