# Week 4 Assignment: Centrality Measures

## NBA Player Co-Occurrence Network Analysis

**Marc Fridson**  
DATA 620, CUNY SPS  
Spring 2026

---

### Overview

This project constructs and analyzes an NBA player co-occurrence network using roster data from the last five NBA seasons (2020-21 through 2024-25). Each node represents an NBA player and an edge connects two players who appeared on the same team roster during the same season. The primary categorical variable is player position (Guard, Forward, Center).

### Data Source

Player roster data is sourced from the official NBA Stats API via the `nba_api` Python package. The original proposal referenced the [balldontlie API](https://www.balldontlie.io/), but since the Stats endpoint required for historical roster construction has moved behind a paid tier, this analysis uses `nba_api` to maintain reproducibility. The network construction methodology is identical to what was proposed.

### Network Summary

| Metric | Value |
|--------|-------|
| Nodes (players) | 885 |
| Edges (co-occurrences) | 16,198 |
| Network density | 0.041 |
| Connected components | 1 |
| Players on 2+ teams | 420 (47.5%) |

### Key Findings

1. **No significant positional differences** in degree centrality (Kruskal-Wallis H=0.87, p=0.65) or eigenvector centrality (H=0.31, p=0.86) across Guards, Forwards, and Centers.

2. **Team mobility is the primary centrality driver.** Spearman correlation between number of teams and degree centrality: rho=0.77, p≈0. Players who changed teams frequently bridge otherwise separate clusters.

3. **The null finding is substantively meaningful:** position does not determine structural importance in the co-occurrence network. NBA roster construction and trade patterns distribute positions relatively evenly across the network topology.

### Requirements

```
pip install networkx matplotlib pandas scipy numpy seaborn nba_api
```

### Files

- `DATA620_Assignment4.ipynb` — Complete Jupyter Notebook with analysis
- `README.md` — This file
