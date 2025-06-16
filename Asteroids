import streamlit as st
import pandas as pd
import numpy as np
import plotly.express as px
import plotly.graph_objects as go
from plotly.subplots import make_subplots
import datetime
from math import sqrt, log10
import io

# Konfiguration der Seite
st.set_page_config(
    page_title="🌍 NASA CNEOS Sentry - Asteroideneinschlag-Risiko Monitor",
    page_icon="☄️",
    layout="wide",
    initial_sidebar_state="expanded"
)

# Titel und Header
st.title("🌍 NASA CNEOS Sentry - Asteroideneinschlag-Risiko Monitor")
st.markdown("### Echte NASA-Daten zu Asteroidenbedrohungen")

# Echte NASA CNEOS Sentry Daten
@st.cache_data
def load_real_sentry_data():
    # Die echten NASA CNEOS Sentry Daten
    csv_data = """Object Designation  ,Year Range  ,Potential Impacts  ,Impact Probability (cumulative),Vinfinity (km/s),H (mag),Estimated Diameter (km),Palermo Scale (cum.),Palermo Scale (max.),Torino Scale (max.),
29075 (1950 DA),2880-2880,1,3.9e-4,14.10,17.9,1.300,-0.92,-0.92,,a0029075
101955 Bennu (1999 RQ36),2178-2290,157,5.7e-4,5.99,20.6,0.490,-1.40,-1.59,,a0101955
(2008 JL3),2027-2122,44,1.7e-4,8.42,25.3,0.029,-2.68,-2.69,0,bK08J03L
(2000 SG344),2069-2122,300,2.7e-3,1.36,24.8,0.037,-2.77,-3.12,0,bK00SY4G
(2010 RF12),2095-2122,70,1.0e-1,5.10,28.4,0.007,-2.97,-2.97,0,bK10R12F
(2022 PX1),2040-2040,1,3.2e-6,35.11,22.3,0.120,-3.13,-3.13,0,bK22P01X
(2005 QK76),2030-2059,6,7.1e-5,19.67,25.2,0.031,-3.18,-3.28,0,bK05Q76K
(2025 JU),2074-2108,10,6.2e-5,15.27,23.2,0.078,-3.24,-3.38,0,bK25J00U
(2021 GX9),2032-2052,2,8.2e-5,16.79,25.3,0.029,-3.25,-3.25,0,bK21G09X
(2024 BY15),2071-2124,331,9.4e-3,NaN,26.7,0.015,-3.30,-3.89,0,bK24B15Y
(2025 LK),2052-2079,15,1.7e-3,9.85,26.9,0.014,-3.48,-3.49,0,bK25L00K
(2015 JJ),2111-2111,1,2.1e-5,10.70,22.1,0.130,-3.56,-3.56,0,bK15J00J
(2023 DO),2057-2092,25,4.5e-4,6.97,25.6,0.026,-3.57,-3.58,0,bK23D00O
(2019 VB37),2049-2067,5,5.7e-5,14.55,24.5,0.043,-3.61,-3.61,0,bK19V37B
(2008 UB7),2044-2101,50,3.4e-5,18.53,23.8,0.058,-3.62,-4.24,0,bK08U07B
(2024 JW16),2082-2121,12,2.3e-6,23.93,21.0,0.220,-3.63,-3.99,0,bK24J16W
(2007 DX40),2035-2122,93,7.8e-5,15.52,24.6,0.040,-3.66,-4.04,0,bK07D40X
(2016 YM4),2121-2121,1,1.3e-5,18.98,22.4,0.110,-3.69,-3.69,0,bK16Y04M
(2012 QD8),2047-2120,16,6.1e-6,20.76,23.1,0.081,-3.71,-3.79,0,bK12Q08D
(2000 SB45),2067-2118,194,1.6e-4,7.53,24.3,0.046,-3.72,-4.19,0,bK00S45B
(2024 TK5),2028-2028,1,3.3e-4,8.93,27.7,0.010,-3.76,-3.76,0,bK24T05K
(2008 EX5),2056-2093,28,5.3e-5,9.92,23.8,0.059,-3.78,-4.05,0,bK08E05X
(2020 VW),2074-2079,12,7.0e-3,9.69,28.3,0.007,-3.82,-4.10,0,bK20V00W
(2020 VV),2044-2122,424,2.3e-3,2.58,27.3,0.012,-3.83,-4.31,0,bK20V00V
(2017 WT28),2083-2121,113,1.2e-2,4.47,28.1,0.008,-3.84,-3.86,0,bK17W28T
(2012 HG2),2052-2122,689,2.0e-3,3.32,27.0,0.014,-3.84,-4.27,0,bK12H02G
(2013 VW13),2063-2095,14,4.4e-4,16.35,26.2,0.019,-3.86,-4.03,0,bK13V13W
(2007 KE4),2029-2096,3,2.0e-5,10.06,25.2,0.031,-3.88,-3.89,0,bK07K04E
(2013 TP4),2026-2026,1,3.5e-5,25.30,27.5,0.011,-3.90,-3.90,0,bK13T04P
(2023 BZ),2026-2026,1,3.8e-5,8.25,26.6,0.016,-3.92,-3.92,0,bK23B00Z
(2023 VD3),2034-2042,3,1.5e-4,17.78,27.1,0.013,-3.94,-3.94,0,bK23V03D
(2006 DM63),2031-2122,46,1.4e-4,10.37,26.7,0.015,-3.95,-4.20,0,bK06D63M
(2020 UL3),2122-2124,3,3.5e-5,10.18,23.2,0.076,-3.97,-4.22,0,bK20U03L"""
    
    # CSV einlesen
    df = pd.read_csv(io.StringIO(csv_data))
    
    # Spaltennamen bereinigen
    df.columns = df.columns.str.strip()
    
    # Datenbereinigung und -aufbereitung
    df['Object Designation'] = df['Object Designation'].str.strip()
    df['Diameter_m'] = df['Estimated Diameter (km)'] * 1000  # Umrechnung in Meter
    df['Impact Probability (cumulative)'] = pd.to_numeric(df['Impact Probability (cumulative)'], errors='coerce')
    df['Torino Scale (max.)'] = pd.to_numeric(df['Torino Scale (max.)'], errors='coerce').fillna(0)
    
    # Jahr-Range aufteilen
    df[['Start_Year', 'End_Year']] = df['Year Range'].str.split('-', expand=True).astype(int)
    df['Years_Until_Start'] = df['Start_Year'] - 2025
    df['Risk_Period_Length'] = df['End_Year'] - df['Start_Year']
    
    # Zusätzliche Berechnungen
    df['Estimated_Energy_MT'] = ((df['Diameter_m'] / 1000) ** 3) * 0.5  # Vereinfachte Energieformel
    df['Risk_Score'] = df['Impact Probability (cumulative)'] * df['Estimated_Energy_MT'] * 1000
    
    # Simulierte geografische Koordinaten basierend auf statistischen Einschlagverteilungen
    np.random.seed(42)  # Für reproduzierbare "zufällige" Koordinaten
    df['Lat'] = np.random.uniform(-60, 60, len(df))  # Bevorzugt bewohnte Breiten
    df['Lon'] = np.random.uniform(-180, 180, len(df))
    
    # Kategorisierung der Gefahr
    def get_risk_category(prob, diameter):
        if prob > 1e-3:
            return "Sehr Hoch"
        elif prob > 1e-4:
            return "Hoch"
        elif prob > 1e-5:
            return "Mittel"
        else:
            return "Niedrig"
    
    df['Risk_Category'] = df.apply(lambda row: get_risk_category(row['Impact Probability (cumulative)'], row['Diameter_m']), axis=1)
    
    return df

# Daten laden
df = load_real_sentry_data()

# Sidebar für Einstellungen
st.sidebar.header("⚙️ Filter & Einstellungen")
risk_threshold = st.sidebar.select_slider(
    "Minimale Einschlagwahrscheinlichkeit",
    options=[1e-6, 1e-5, 1e-4, 1e-3, 1e-2],
    value=1e-5,
    format_func=lambda x: f"1:{int(1/x):,}"
)

min_diameter = st.sidebar.slider("Minimaler Durchmesser (m)", 0, 1300, 0)
time_filter = st.sidebar.selectbox("Zeitraum filtern", 
                                  ["Alle", "Bis 2050", "Bis 2100", "Nach 2100"])

# Filter anwenden
filtered_df = df[df['Impact Probability (cumulative)'] >= risk_threshold].copy()
filtered_df = filtered_df[filtered_df['Diameter_m'] >= min_diameter]

if time_filter == "Bis 2050":
    filtered_df = filtered_df[filtered_df['Start_Year'] <= 2050]
elif time_filter == "Bis 2100":
    filtered_df = filtered_df[filtered_df['Start_Year'] <= 2100]
elif time_filter == "Nach 2100":
    filtered_df = filtered_df[filtered_df['Start_Year'] > 2100]

# Informationsbox über das Sentry-System
with st.expander("ℹ️ Über das NASA CNEOS Sentry-System", expanded=True):
    st.markdown("""
    **Sentry** ist das automatisierte Einschlagvorhersagesystem der NASA, das kontinuierlich 
    near-Earth objects (NEOs) auf mögliche zukünftige Einschläge überwacht.
    
    **📊 Aktuelle Daten zeigen:**
    - **33 Objekte** auf der Sentry-Risikoliste (Stand: Juni 2025)
    - **Alle aktuellen Objekte** haben Torino-Skala 0 (kein signifikantes Risiko)
    - **Höchste Wahrscheinlichkeit:** 2010 RF12 mit 10% für 2095-2122
    - **Größter Asteroid:** 1950 DA mit 1,3 km Durchmesser (Risiko: 2880)
    
    **🎯 Wichtige Erkenntnisse:**
    - Kein Asteroid stellt derzeit eine unmittelbare Bedrohung dar
    - Die meisten Risiken liegen weit in der Zukunft (>50 Jahre)
    - Kontinuierliche Überwachung verbessert Vorhersagegenauigkeit
    """)

# Hauptdashboard
col1, col2, col3, col4 = st.columns(4)

with col1:
    st.metric("Überwachte Objekte", len(df), delta="Offizielle NASA-Daten")

with col2:
    high_prob_objects = len(df[df['Impact Probability (cumulative)'] > 1e-4])
    st.metric("Höhere Wahrscheinlichkeit", high_prob_objects, delta=">0.01%")

with col3:
    next_risk_year = df['Start_Year'].min()
    st.metric("Nächstes Risikojahr", next_risk_year, delta=f"in {next_risk_year-2025} Jahren")

with col4:
    largest_object = df.loc[df['Diameter_m'].idxmax(), 'Object Designation']
    st.metric("Größtes Objekt", largest_object.split('(')[0].strip())

# Tabs für verschiedene Ansichten
tab1, tab2, tab3, tab4, tab5 = st.tabs(["🗺️ Weltkarte", "📊 Risiko-Analyse", "📋 Objektliste", "🎯 Detailanalyse", "📈 Statistiken"])

with tab1:
    st.subheader("Potenzielle Einschlagsorte (simuliert)")
    
    if len(filtered_df) == 0:
        st.warning("Keine Objekte entsprechen den gewählten Filterkriterien.")
    else:
        # Weltkarte mit Asteroiden
        fig_map = px.scatter_mapbox(
            filtered_df,
            lat="Lat",
            lon="Lon",
            size="Diameter_m",
            color="Impact Probability (cumulative)",
            hover_name="Object Designation",
            hover_data={
                "Start_Year": True,
                "End_Year": True,
                "Potential Impacts": True,
                "Diameter_m": ":.0f",
                "Impact Probability (cumulative)": ":.2e"
            },
            color_continuous_scale="Reds",
            size_max=50,
            zoom=1,
            height=600,
            title="Simulierte Einschlagspositionen basierend auf NASA CNEOS Sentry-Daten"
        )
        
        fig_map.update_layout(mapbox_style="open-street-map")
        st.plotly_chart(fig_map, use_container_width=True)

with tab2:
    st.subheader("Risiko-Analyse der NASA CNEOS Sentry-Daten")
    
    col1, col2 = st.columns(2)
    
    with col1:
        # Wahrscheinlichkeitsverteilung
        fig_prob = px.histogram(
            df,
            x="Impact Probability (cumulative)",
            title="Verteilung der Einschlagwahrscheinlichkeiten",
            log_x=True,
            nbins=20
        )
        fig_prob.update_layout(xaxis_title="Einschlagwahrscheinlichkeit", yaxis_title="Anzahl Objekte")
        st.plotly_chart(fig_prob, use_container_width=True)
    
    with col2:
        # Größenverteilung
        fig_size = px.histogram(
            df,
            x="Diameter_m",
            title="Verteilung der Asteroidengrößen",
            nbins=15
        )
        fig_size.update_layout(xaxis_title="Durchmesser (m)", yaxis_title="Anzahl Objekte")
        st.plotly_chart(fig_size, use_container_width=True)
    
    # Zeitliche Verteilung
    fig_timeline = px.scatter(
        df,
        x="Start_Year",
        y="Impact Probability (cumulative)",
        size="Diameter_m",
        color="Risk_Category",
        hover_name="Object Designation",
        title="Zeitliche Verteilung der Asteroidenrisiken",
        log_y=True
    )
    fig_timeline.update_layout(xaxis_title="Erstes Risikojahr", yaxis_title="Einschlagwahrscheinlichkeit")
    st.plotly_chart(fig_timeline, use_container_width=True)

with tab3:
    st.subheader("Vollständige NASA CNEOS Sentry-Liste")
    
    # Suchfeld
    search_term = st.text_input("🔍 Objekt suchen:", placeholder="z.B. Bennu, 1950 DA...")
    
    # Filtern
    display_df = filtered_df.copy() if len(filtered_df) > 0 else df.copy()
    if search_term:
        display_df = display_df[display_df['Object Designation'].str.contains(search_term, case=False, na=False)]
    
    # Sortierung
    sort_options = {
        'Einschlagwahrscheinlichkeit': 'Impact Probability (cumulative)',
        'Durchmesser': 'Diameter_m',
        'Erstes Risikojahr': 'Start_Year',
        'Potenzielle Einschläge': 'Potential Impacts',
        'Palermo-Skala': 'Palermo Scale (cum.)'
    }
    sort_by = st.selectbox("Sortieren nach:", list(sort_options.keys()))
    ascending = st.checkbox("Aufsteigend", value=False)
    
    display_df = display_df.sort_values(sort_options[sort_by], ascending=ascending)
    
    # Tabelle formatieren
    table_df = display_df[['Object Designation', 'Year Range', 'Potential Impacts', 
                          'Impact Probability (cumulative)', 'Diameter_m', 'Estimated_Energy_MT',
                          'Palermo Scale (cum.)', 'Risk_Category']].copy()
    
    table_df['Impact Probability (cumulative)'] = table_df['Impact Probability (cumulative)'].apply(
        lambda x: f"{x:.2e} (1:{int(1/x):,})" if pd.notna(x) and x > 0 else "N/A"
    )
    table_df['Diameter_m'] = table_df['Diameter_m'].round().astype(int)
    table_df['Estimated_Energy_MT'] = table_df['Estimated_Energy_MT'].round(2)
    
    st.dataframe(table_df, use_container_width=True, height=500)

with tab4:
    st.subheader("🎯 Detailanalyse ausgewählter Objekte")
    
    # Top-Risiko Objekte
    top_risk_objects = df.nlargest(5, 'Impact Probability (cumulative)')
    
    selected_object = st.selectbox("Objekt für Detailanalyse wählen:", 
                                  df['Object Designation'].tolist(),
                                  index=df['Object Designation'].tolist().index(top_risk_objects.iloc[0]['Object Designation']))
    
    if selected_object:
        obj_data = df[df['Object Designation'] == selected_object].iloc[0]
        
        col1, col2 = st.columns(2)
        
        with col1:
            st.info(f"""
            **{selected_object}**
            
            🔸 **Durchmesser:** {obj_data['Diameter_m']:.0f} m  
            🔸 **Einschlagwahrscheinlichkeit:** {obj_data['Impact Probability (cumulative)']:.2e} (1:{int(1/obj_data['Impact Probability (cumulative)']):,})  
            🔸 **Risikozeitraum:** {obj_data['Year Range']}  
            🔸 **Potenzielle Einschläge:** {obj_data['Potential Impacts']}  
            🔸 **Palermo-Skala:** {obj_data['Palermo Scale (cum.)']}  
            🔸 **Geschätzte Energie:** {obj_data['Estimated_Energy_MT']:.2f} Megatonnen TNT
            """)
            
            # Relativgeschwindigkeit (falls verfügbar)
            if pd.notna(obj_data['Vinfinity (km/s)']):
                st.info(f"🚀 **Relativgeschwindigkeit:** {obj_data['Vinfinity (km/s)']} km/s")
        
        with col2:
            # Vergleichskontext
            diameter = obj_data['Diameter_m']
            probability = obj_data['Impact Probability (cumulative)']
            
            if diameter < 50:
                impact_scale = "Lokale Zerstörung (Stadt/Region)"
                comparison = "Vergleichbar mit Tscheljabinsk-Meteor 2013"
            elif diameter < 200:
                impact_scale = "Regionale Katastrophe"
                comparison = "Mehrere Großstädte könnten betroffen sein"
            elif diameter < 1000:
                impact_scale = "Kontinentale Auswirkungen"
                comparison = "Klimatische Auswirkungen möglich"
            else:
                impact_scale = "Globale Katastrophe"
                comparison = "Massenaussterben-Event möglich"
            
            st.warning(f"""
            **Auswirkungen bei Einschlag:**
            
            💥 **Zerstörungsgrad:** {impact_scale}  
            📊 **Vergleich:** {comparison}  
            ⏰ **Zeitrahmen:** {obj_data['Years_Until_Start']} Jahre bis Risikobeginn  
            🎯 **Wahrscheinlichkeit pro Jahr:** ≈ {probability/max(obj_data['Risk_Period_Length'], 1):.2e}
            """)
        
        # Spezielle Hinweise für bekannte Objekte
        if "Bennu" in selected_object:
            st.success("""
            **🛰️ Spezial: OSIRIS-REx Mission**  
            Bennu wurde von der NASA-Sonde OSIRIS-REx besucht und Proben zur Erde gebracht. 
            Diese Mission hat unser Verständnis von Bennos Bahn und Zusammensetzung erheblich verbessert.
            """)
        elif "1950 DA" in selected_object:
            st.warning("""
            **⚠️ Spezial: 1950 DA**  
            Dies ist der einzige Asteroid mit einem nominellen Einschlagrisiko für das Jahr 2880. 
            Moderne Technologien haben bis dahin vermutlich Ablenkungsmöglichkeiten entwickelt.
            """)

with tab5:
    st.subheader("📈 Statistische Auswertungen")
    
    col1, col2 = st.columns(2)
    
    with col1:
        st.markdown("**📊 Risikostatistiken:**")
        st.write(f"• Höchste Wahrscheinlichkeit: {df['Impact Probability (cumulative)'].max():.2e}")
        st.write(f"• Durchschnittliche Wahrscheinlichkeit: {df['Impact Probability (cumulative)'].mean():.2e}")
        st.write(f"• Größter Asteroid: {df['Diameter_m'].max():.0f} m")
        st.write(f"• Kleinster Asteroid: {df['Diameter_m'].min():.0f} m")
        
        # Dekaden-Analyse
        df['Decade'] = (df['Start_Year'] // 10) * 10
        decade_counts = df['Decade'].value_counts().sort_index()
        
        fig_decades = px.bar(
            x=decade_counts.index,
            y=decade_counts.values,
            title="Asteroidenrisiken nach Dekaden",
            labels={'x': 'Dekade', 'y': 'Anzahl Objekte'}
        )
        st.plotly_chart(fig_decades, use_container_width=True)
    
    with col2:
        st.markdown("**🎯 Risikokategorien:**")
        risk_counts = df['Risk_Category'].value_counts()
        for category, count in risk_counts.items():
            percentage = (count / len(df)) * 100
            st.write(f"• {category}: {count} Objekte ({percentage:.1f}%)")
        
        # Palermo-Skala Verteilung
        fig_palermo = px.histogram(
            df,
            x="Palermo Scale (cum.)",
            title="Palermo-Skala Verteilung",
            nbins=15
        )
        st.plotly_chart(fig_palermo, use_container_width=True)

# Footer mit Datenquelle
st.markdown("---")
st.markdown("""
<div style='text-align: center; color: #666;'>
<p><strong>Datenquelle:</strong> NASA Center for Near Earth Object Studies (CNEOS) - Sentry System<br>
<strong>Letztes Update:</strong> Juni 2025 | <strong>Anzahl Objekte:</strong> 33</p>
<p><em>Hinweis: Diese App verwendet die offiziellen NASA CNEOS Sentry-Daten. Geografische Positionen sind simuliert.</em></p>
<p>🔗 <a href='https://cneos.jpl.nasa.gov/sentry/'>Offizielle NASA CNEOS Sentry-Website</a></p>
</div>
""", unsafe_allow_html=True)

# Sidebar mit zusätzlichen Informationen
st.sidebar.markdown("---")
st.sidebar.subheader("📚 Bewertungsskalen")
st.sidebar.markdown("""
**Torino-Skala (0-10):**
- 0: Kein Risiko
- 1: Verdächtig  
- 2-4: Aufmerksamkeit verdienend
- 5-7: Bedrohlich
- 8-10: Sichere Kollision

**Palermo-Skala:**
- < -2: Kein Grund zur Sorge
- -2 bis 0: Aufmerksamkeit verdienend
- > 0: Besorgnis erregend

**Aktueller Status:**
- Alle 33 Objekte: Torino-Skala 0
- Höchstes Risiko: 2010 RF12 (10%)
""")

if st.sidebar.button("🔄 Daten neu laden"):
    st.cache_data.clear()
    st.rerun()
