# sacha-sankey
Diagrama de facilidades de produccion de petroleo en el campos Sacha
import plotly.graph_objects as go

labels = [
    # Pozos productores
    "Norte 1", "Sur A", "Sur B", "Norte 2", "Central Pozos",
    # Estaciones
    "Est. N1\n(Sep, Tanques)",
    "Est. Sur\n(Sep, Tanques)",
    "Est. N2\n(Sep, LACT, Gas)",
    "Central\n(Scrubbers, Sep, LACT)",
    # Manejo de agua
    "Agua N1", "Agua Sur", "Agua Central",
    # Mecheros
    "Mechero N1", "Mechero Sur", "Mechero N2", "Mechero Cent",
    # Otros
    "Compresor Gas", "Planta Gas", "Generador", "Reinj. Agua", "Reinj. Gas",
    # Oleoducto
    "SOTE",
    # Tanques de lavado
    "Tanque Lavado N1", "Tanque Lavado Sur", "Tanque Lavado N2", "Tanque Lavado Central",
    # Skimmer
    "Skimmer N2", "Skimmer Central"
]

x_pos = [
    0.0, 0.0, 0.0, 0.0, 0.0,
    0.25, 0.25, 0.25, 0.6,
    0.4, 0.4, 0.6,
    0.4, 0.4, 0.4, 0.4,
    0.75, 0.75, 0.85, 0.9, 0.9,
    1.0,
    0.3, 0.3, 0.3, 0.55, 0.6, 0.65
]

y_pos = [
    0.9, 0.6, 0.5, 0.3, 0.7,
    0.9, 0.55, 0.3, 0.7,
    0.8, 0.5, 0.7,
    0.92, 0.55, 0.32, 0.2,
    0.75, 0.7, 0.55, 0.45, 0.35,
    0.6,
    0.85, 0.53, 0.28, 0.68, 0.25, 0.65
]

sources = [
    0, 1, 2, 3, 4,          # Pozos a estaciones
    5, 6, 7,                # Estaciones a Central o SOTE
    5, 6, 7,                # Manejo de agua
    8, 8,                   # Manejo de agua en Central
    5,                     # Mechero N1
    6,                     # Mechero Sur
    7,                     # Mechero N2
    8,                     # Mechero Cent
    8,                     # Compresor
    16, 16, 16, 16, 16,     # Compresor a: planta, generador, reinyecciones
    5, 6, 7, 8,             # Estaciones hacia tanques de lavado
    22, 23, 24, 25,         # Lavado a agua o skimmer
    26, 27,                 # Skimmer a agua Central
    11                     # Agua Central hacia Agua Sur
]

targets = [
    5, 6, 6, 7, 8,
    8, 8, 21,
    9, 10, 11,
    11, 11,
    12,                   # Mechero N1
    13,                   # Mechero Sur
    14,                   # Mechero N2
    15,                   # Mechero Central
    16,
    17, 18, 19, 20, 21,
    22, 23, 24, 25,
    9, 10, 26, 27,
    11, 11,
    10                   # Agua Central hacia Agua Sur
]

values = [
    0.3, 0.15, 0.15, 0.3, 0.1,
    0.6, 0.5, 0.3,
    0.05, 0.05, 0.1,
    0.05, 0.05,
    0.07,
    0.06,
    0.05,
    0.08,
    1.0,
    0.2, 0.15, 0.15, 0.1, 0.4,
    0.05, 0.05, 0.05, 0.05,
    0.02, 0.02, 0.02, 0.02,
    0.01, 0.01,
    0.03
]

node_colors = [
    "#1f77b4", "#1f77b4", "#1f77b4", "#1f77b4", "#1f77b4",     # Pozos
    "#2ca02c", "#2ca02c", "#2ca02c", "#9467bd",                # Estaciones
    "#17becf", "#17becf", "#17becf",                           # Agua
    "#ff7f0e", "#ff7f0e", "#ff7f0e", "#ff7f0e",                # Mecheros
    "#d62728", "#9467bd", "#8c564b", "#e377c2", "#7f7f7f",     # Otros
    "#8c564b",                                                 # Oleoducto
    "#aec7e8", "#aec7e8", "#aec7e8", "#aec7e8",                # Lavado
    "#ffbb78", "#ffbb78"                                      # Skimmers
]

link_colors = [
    "rgba(31,119,180,0.6)", "rgba(31,119,180,0.6)", "rgba(31,119,180,0.6)",
    "rgba(31,119,180,0.6)", "rgba(31,119,180,0.6)",
    "rgba(44,160,44,0.5)", "rgba(44,160,44,0.5)", "rgba(44,160,44,0.5)",
    "rgba(23,190,207,0.5)", "rgba(23,190,207,0.5)", "rgba(23,190,207,0.5)",
    "rgba(23,190,207,0.5)", "rgba(23,190,207,0.5)",
    "rgba(255,127,14,0.5)", "rgba(255,127,14,0.5)", "rgba(255,127,14,0.5)", "rgba(255,127,14,0.5)",
    "rgba(140,86,75,0.7)",
    "rgba(214,39,40,0.6)", "rgba(148,103,189,0.6)", "rgba(140,86,75,0.6)",
    "rgba(227,119,194,0.6)", "rgba(127,127,127,0.6)",
    "rgba(174,199,232,0.5)", "rgba(174,199,232,0.5)",
    "rgba(174,199,232,0.5)", "rgba(174,199,232,0.5)",
    "rgba(174,199,232,0.5)", "rgba(174,199,232,0.5)",
    "rgba(255,187,120,0.5)", "rgba(255,187,120,0.5)",
    "rgba(23,190,207,0.4)"
]

fig = go.Figure(go.Sankey(
    arrangement="snap",
    node=dict(
        pad=30,
        thickness=15,
        line=dict(color="black", width=0.5),
        label=labels,
        x=x_pos,
        y=y_pos,
        color=node_colors,
    ),
    link=dict(
        source=sources,
        target=targets,
        value=values,
        color=link_colors
    )
))

fig.update_layout(
    title=dict(
        text="<b>Campo Sacha – Bloque 60</b>",
        x=0.5,
        xanchor='center',
        font=dict(size=16)
    ),
    font=dict(size=10),
    height=900,
    width=1200
)

fig.show()

fig.write_html("sacha_sankey.html")

# Para Google Colab
from google.colab import files
files.download("sacha_sankey.html")
