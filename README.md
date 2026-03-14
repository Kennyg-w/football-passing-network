# ⚽ Football Passing Network Analysis

A **data science project** that analyses football team passing structure using **network science and spatial analysis**.

The project builds a **passing network visualisation** for a football team using event data, allowing us to understand how players interact, how the team is structured on the pitch, and which players are most influential in the passing system.

---

# 📊 Project Goal

The goal of this project is to analyse **team passing structure** by representing players and passes as a **network graph**.

Each player is represented as a **node**, and each completed pass is represented as an **edge** connecting two players.

Using this network, we can measure:

* Team shape
* Passing relationships
* Player influence
* Tactical structure

---

# 🗂 Dataset

This project uses **StatsBomb Open Data**, a publicly available football event dataset containing detailed match events.

Dataset includes:

* Pass events
* Player positions
* Match metadata
* Team information

Source:

https://github.com/statsbomb/open-data

---

# 🛠 Tech Stack

The project is built using the following Python libraries:

* **Python**
* **Pandas** – data processing
* **NumPy** – numerical analysis
* **NetworkX** – network graph construction
* **Matplotlib** – visualisation
* **mplsoccer** – football pitch plotting

---

# 📐 Key Metrics

Several tactical metrics are calculated from the passing network:

### Team Length

Distance between the deepest and most advanced players along the pitch (x-axis).

### Team Width

Distance between the widest players along the pitch (y-axis).

### Network Assortativity

Measures whether players with similar passing involvement tend to connect with each other.

---

# 📈 Passing Network Visualization

Example output from the analysis:

![Passing Network](outputs/barcelona_passing_network.png)

Nodes represent players and edges represent passes between players.

* **Node size** = number of passes made
* **Edge thickness** = number of passes between players

---

# 📁 Project Structure

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
│   └── barcelona_passing_network.png
│
├── src
│
├── README.md
├── CHEATSHEET.md
└── requirements.txt
```

---

# ⚙️ Installation

Clone the repository:

```bash
git clone https://github.com/Kennyg-w/football-passing-network.git
cd football-passing-network
```

Install dependencies:

```bash
pip install -r requirements.txt
```

---

# ▶️ Usage

Run the analysis notebook:

```bash
jupyter notebook notebooks/passing_network_analysis.ipynb
```

The notebook will:

1. Load match event data
2. Filter completed passes
3. Build the passing network
4. Calculate tactical metrics
5. Generate the passing network visualization

---

# 📊 Results

The passing network provides insight into:

* Player connectivity
* Passing dominance
* Tactical formation
* Team compactness

Key outputs include:

* Passing network visualisation
* Player passing statistics
* Team structure metrics

---

# 🚀 Future Improvements

Possible extensions to this project:

* Multi-match analysis
* Season-level passing networks
* Player centrality metrics
* Interactive visualizations
* Machine learning for tactical pattern detection

---

# 👤 Author

Kennyg-w

GitHub:
https://github.com/Kennyg-w

---

# ⭐ If you find this project useful

Feel free to **star the repository**.
