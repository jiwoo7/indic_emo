# indic_emo
# India's first Hinglish emotion dataset collector.

import streamlit as st
import datetime
import time
import json

st.set_page_config(page_title="IndicEmo 🇮🇳", page_icon="🔥")
st.title("🇮🇳 IndicEmo – India’s First Hinglish Emotion Dataset")
st.write("Built by a Class 11 student for MIT/Stanford/Harvard research 🔥")

name = st.text_input("Your Name (only for consent)", placeholder="Rahul / Priya")
age = st.slider("Age", 13, 25, 17)
language = st.selectbox("Main language you will speak/type", ["Hinglish", "Tanglish", "Bengali-English", "Pure Hindi", "Pure English", "Other"])

emotion_options = ["Happy 😊", "Sad 😢", "Angry 😡", "Neutral 😐", "Excited 🎉", "Frustrated 😤", "Fearful 😨"]
emotion = st.selectbox("How are you feeling RIGHT NOW?", emotion_options)

st.write("### Step 1: Record your voice (5–20 seconds)")
recorded_audio = st.audio_input("Press and speak naturally in your accent!")  # Updated to st.audio_input for better compatibility

st.write("### Step 2: Type exactly how you talk (with shortcuts, emojis, code-mixing)")
typed_text = st.text_area("Type here (backspace as much as you want)", height=120)

if st.button("🚀 Submit & Help India Get AI That Understands Us!", type="primary"):
    if recorded_audio is not None and typed_text.strip() != "":
        timestamp = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
        meta = {
            "name": name, "age": age, "language": language,
            "emotion": emotion, "typed_text": typed_text,
            "timestamp": timestamp
        }
        st.session_state.setdefault("data", []).append(meta)
        
        st.success(f"THANK YOU {name.upper()}! 🇮🇳 You are participant #{len(st.session_state.data)}")
        st.balloons()
        st.write("Share this link → your friends can help too!")
    else:
        st.error("Please record audio AND type something!")
        
st.write(f"### Total participants so far: {len(st.session_state.get('data', []))}")
if st.button("Show me the raw data (for researcher only)"):
    st.json(st.session_state.get("data", []))
