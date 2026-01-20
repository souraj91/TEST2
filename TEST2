import streamlit as st
import pandas as pd
from datetime import datetime, date
import plotly.express as px

# --- CONFIGURATION ---
st.set_page_config(page_title="Morning Routine Tracker", page_icon="☀️", layout="centered")

# Simuler une base de données (Dans un vrai projet, on utiliserait un CSV ou SQLite)
if 'history' not in st.session_state:
    st.session_state.history = pd.DataFrame(columns=["Date", "Tâche", "Complété"])

# Liste de tes habitudes du matin
HABITS = ["Méditation (10min)", "Sport / Yoga", "Lecture", "Petit-déjeuner sain", "Écriture Journal"]

# --- SIDEBAR (HISTORIQUE) ---
st.sidebar.header("🗓️ Historique Rapide")
if not st.session_state.history.empty:
    st.sidebar.write(st.session_state.history.tail(10))
else:
    st.sidebar.info("Aucune donnée enregistrée.")

# --- TITRE ---
st.title("☀️ Ma Morning Routine")
st.subheader(f"Aujourd'hui, nous sommes le {date.today().strftime('%d %B %Y')}")

# --- SECTION 1 : VALIDATION DU JOUR ---
st.markdown("---")
st.write("### ✅ Valider mes habitudes")

# Création d'une colonne par habitude pour un affichage propre
cols = st.columns(len(HABITS))
results = {}

for i, habit in enumerate(HABITS):
    with cols[i]:
        results[habit] = st.checkbox(habit)

if st.button("Enregistrer ma routine", type="primary"):
    new_data = []
    for habit, completed in results.items():
        new_data.append({"Date": date.today(), "Tâche": habit, "Complété": completed})
    
    # Mise à jour de "l'historique"
    new_df = pd.DataFrame(new_data)
    st.session_state.history = pd.concat([st.session_state.history, new_df], ignore_index=True)
    st.success("Routine enregistrée ! Garde le rythme. 🔥")
    st.balloons()

# --- SECTION 2 : DASHBOARD & ANALYSE ---
st.markdown("---")
st.write("### 📊 Analyse de performance")

if not st.session_state.history.empty:
    # Calcul du taux de complétion par tâche
    df_stats = st.session_state.history.groupby("Tâche")["Complété"].mean().reset_index()
    df_stats["Complété"] = df_stats["Complété"] * 100

    # Graphique de performance
    fig = px.bar(
        df_stats, 
        x="Tâche", 
        y="Complété", 
        title="Taux de réussite par habitude (%)",
        color="Complété",
        color_continuous_scale="Viridis",
        range_y=[0, 100]
    )
    st.plotly_chart(fig, use_container_width=True)

    # Widget de score global
    total_score = int(df_stats["Complété"].mean())
    st.metric(label="Score Global de Discipline", value=f"{total_score}%", delta=f"{total_score - 50}% vs objectif")
else:
    st.info("Enregistre ta première routine pour voir les graphiques s'afficher.")

# --- PETITE CITATION MOTIVANTE ---
st.markdown("---")
st.caption("_'Le succès est la somme de petits efforts, répétés jour après jour.'_")
