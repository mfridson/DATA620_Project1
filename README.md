# Week 4 Assignment: Centrality Measures

## Data Source: NBA Player Shared Team Network via balldontlie API

For this assignment, I'm proposing a network analysis of NBA players connected by shared team history, sourced from the [balldontlie API](https://www.balldontlie.io/), a free, well-documented REST API that provides current and historical NBA player, team, and season data. The API doesn't require authentication for basic endpoints, which makes it straightforward to pull at scale.

The network would be constructed as follows: each **node** represents an NBA player, and an **edge** connects two players if they appeared on the same team roster during the same season. This creates a co-occurrence network, similar in structure to collaboration networks in academic research, but applied to professional basketball.

### Categorical Variable

The key categorical variable here is **player position** (Guard, Forward, Center), which is available directly from the API's player endpoint. This gives us a natural grouping to compare centrality distributions across. Additional categorical variables could include **active vs. inactive status** and **conference affiliation** (Eastern vs. Western), both of which are derivable from the team data.

## Data Loading Plan

1. **Pull player rosters by season** using the `/players` and `/stats` endpoints from balldontlie. The stats endpoint returns per-game data that includes team and season identifiers, which is what we need to establish co-occurrence.

2. **Construct an edge list** in R by iterating through each team-season combination and creating pairwise connections between all players who appeared on that roster. Players who were traded mid-season would have edges to teammates on multiple teams, which actually makes the network more interesting since those players become natural bridges.

3. **Load into igraph** using `graph_from_data_frame()`, attaching position as a vertex attribute. From there, computing degree, betweenness, closeness, and eigenvector centrality is straightforward with igraph's built-in functions.

4. **Scope the analysis** to a manageable window, probably the last 5 seasons, which would give us roughly 2,500+ unique players and tens of thousands of edges. Enough to see meaningful structural patterns without the computational overhead of pulling 75 years of NBA history.

## Hypothetical Outcome: Degree Centrality and Post-Career Broadcasting Success

Here's where it gets interesting. The hypothesis I'd want to test is whether **players with higher degree centrality are more likely to transition into successful media or broadcasting careers after retirement.** The logic is that degree centrality in this network is essentially a proxy for how many unique teammates a player has had across their career. Players who were traded frequently or had long careers accumulate more connections, and those connections translate into a broader professional network and name recognition across multiple fan bases.

If we compare degree centrality distributions across positions, I'd expect **Guards to have higher average degree centrality than Centers**, both because guard rotations tend to be deeper and because ball-handling positions interact with more teammates in functional on-court networks. If that holds, we'd also expect to see guards disproportionately represented in post-career media roles, which anecdotally tracks (think about how many former point guards end up as analysts versus former centers).

The prediction would be: **nodes in the top quartile of degree centrality, particularly those in the Guard category, are more likely to achieve positive post-career outcomes in media visibility.** This could be validated by cross-referencing the network data with a labeled dataset of former players who have transitioned to broadcasting, coaching, or front office roles.

This is a testable, real-world application of centrality measures that goes beyond just describing network structure. It connects graph theory to actual career trajectory prediction, which is the kind of applied analysis that makes network science useful outside of academia.

## Implementation Notes

The analysis notebook (`Assignment4_Centrality.ipynb`) uses two edge layers to construct a connected network:
- **Same team edges** (weight = 1.0): Players on the same current/recent team roster
- **Draft class edges** (weight = 0.3): Players from the same draft year, representing cohort connections

This dual-layer approach is necessary because the free tier of the balldontlie API only provides current team assignments, not historical game-by-game data (which would require the ALL-STAR tier). The draft class connections are a legitimate representation of real professional ties: players drafted in the same year share combine experiences, rookie events, and entered the league as a cohort.

### Requirements
- R with IRkernel (for Jupyter)
- Packages: `httr`, `jsonlite`, `igraph`, `ggplot2`, `dplyr`, `tidyr`, `RColorBrewer`, `gridExtra`, `scales`
- A free balldontlie API key from [app.balldontlie.io](https://app.balldontlie.io)

---

*Marc Fridson*
*DATA 620, CUNY SPS*
*Spring 2026*
