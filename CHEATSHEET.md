# Football Passing Network Analysis — Cheat Sheet

## 1. Terminal Setup

Create project:

```bash
cd ~/Documents
mkdir football-passing-network
cd football-passing-network
git init
```

Create structure:

```bash
mkdir data data/raw notebooks outputs src
touch .gitignore README.md CHEATSHEET.md requirements.txt
touch notebooks/passing_network_analysis.ipynb
```

Open in VS Code:

```bash
code .
```

---

# 2. Virtual Environment

Create environment:

```bash
python3 -m venv venv
```

Activate environment:

```bash
source venv/bin/activate
```

Deactivate:

```bash
deactivate
```

---

# 3. Install Python Libraries

```bash
pip install pandas numpy matplotlib networkx scipy tabulate mplsoccer jupyter
```

Save requirements:

```bash
pip freeze > requirements.txt
```

Install later:

```bash
pip install -r requirements.txt
```

---

# 4. Run Jupyter Notebook

Start notebook server:

```bash
jupyter notebook
```

Open:

```
notebooks/passing_network_analysis.ipynb
```

---

# 5. Core Python Imports

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import networkx as nx

from mplsoccer import Pitch, Sbopen
```

---

# 6. Load StatsBomb Data

Create parser:

```python
parser = Sbopen()
```

Load competitions:

```python
competitions = parser.competition()
```

Load matches:

```python
matches = parser.match(competition_id=11, season_id=90)
```

Load match events:

```python
events, related, freeze, tactics = parser.event(match_id)
```

---

# 7. Filter Team Passes

Identify team:

```python
team = events[
    events["team_name"].str.contains("Barcelona", case=False)
]["team_name"].mode()[0]
```

Extract passes:

```python
passes = events[
    (events["team_name"] == team) &
    (events["type_name"] == "Pass")
]
```

Completed passes:

```python
completed_passes = passes[passes["pass_outcome_name"].isna()]
```

---

# 8. Player Average Positions

```python
avg_positions = (
    completed_passes
    .groupby("player_name")
    .agg(
        avg_x=("x","mean"),
        avg_y=("y","mean"),
        passes=("player_name","count")
    )
    .reset_index()
)
```

---

# 9. Pass Connections

```python
pass_links = (
    completed_passes
    .groupby(["player_name","pass_recipient_name"])
    .size()
    .reset_index(name="pass_count")
)
```

---

# 10. Build Network Graph

```python
G = nx.DiGraph()

for _, row in avg_positions.iterrows():
    G.add_node(
        row["player_name"],
        x=row["avg_x"],
        y=row["avg_y"],
        passes=row["passes"]
    )

for _, row in pass_links.iterrows():
    G.add_edge(
        row["player_name"],
        row["pass_recipient_name"],
        weight=row["pass_count"]
    )
```

---

# 11. Network Metrics

Team length:

```python
team_length = avg_positions["avg_x"].max() - avg_positions["avg_x"].min()
```

Team width:

```python
team_width = avg_positions["avg_y"].max() - avg_positions["avg_y"].min()
```

Assortativity:

```python
nx.degree_assortativity_coefficient(G)
```

Centrality:

```python
nx.betweenness_centrality(G)
nx.degree_centrality(G)
```

---

# 12. Plot Passing Network

```python
pitch = Pitch(pitch_type="statsbomb")
fig, ax = pitch.draw(figsize=(12,8))

for u,v,data in G.edges(data=True):

    x1 = G.nodes[u]["x"]
    y1 = G.nodes[u]["y"]
    x2 = G.nodes[v]["x"]
    y2 = G.nodes[v]["y"]

    pitch.lines(
        x1,y1,x2,y2,
        lw=data["weight"]*0.4,
        alpha=0.6,
        ax=ax
    )

node_x = [G.nodes[n]["x"] for n in G.nodes]
node_y = [G.nodes[n]["y"] for n in G.nodes]

pitch.scatter(node_x,node_y,ax=ax)

plt.title("Passing Network")
plt.show()
```

---

# 13. Save Outputs

Save figure:

```python
fig.savefig("../outputs/passing_network.png", dpi=300)
```

Save tables:

```python
avg_positions.to_csv("../data/player_positions.csv", index=False)
pass_links.to_csv("../data/pass_links.csv", index=False)
```

---

# 14. Git Workflow

Stage files:

```bash
git add .
```

Commit:

```bash
git commit -m "Initial passing network analysis"
```

Check status:

```bash
git status
```

---

# 15. Debug Commands

Check dataframe:

```python
events.head()
events.columns
events.shape
```

Check missing data:

```python
events.isna().sum()
```

Check passes:

```python
completed_passes.head()
```

---

# 16. Useful pandas Commands

Select columns:

```python
df[["col1","col2"]]
```

Filter rows:

```python
df[df["column"] == value]
```

Groupby:

```python
df.groupby("column").mean()
```

Sort values:

```python
df.sort_values("column", ascending=False)
```

---

# 17. Project Structure

```
football-passing-network
│
├── data
│   └── raw
│
├── notebooks
│   └── passing_network_analysis.ipynb
│
├── outputs
│
├── src
│
├── .gitignore
├── README.md
├── CHEATSHEET.md
└── requirements.txt
```

---

# 18. Skills Demonstrated

This project proves knowledge of:

* Python
* Pandas
* Network analysis
* Data visualization
* Football analytics
* Git workflow
* Reproducible research

---

# 19. Quick Restart Workflow

If returning to the project later:

```bash
cd ~/Documents/football-passing-network
source venv/bin/activate
jupyter notebook
```
