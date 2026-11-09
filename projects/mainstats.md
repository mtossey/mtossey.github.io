---
layout: project
type: project
image: img/mainstats/results-page.png
title: "MainStats: Mains and One-Tricks in NA Master+"
date: 2026-11-06
published: true
labels:
  - Python
  - SQL
  - SQLite
  - pandas
  - React
  - TypeScript
  - Riot Games API
summary: "A SQL and Python analysis of which League of Legends champions North America's top-ranked players main and one-trick, and whether mains win more on their champion, with a React website to explore the results."
---

<img class="img-fluid" src="../img/mainstats/results-page.png">

## Overview
MainStats answers two questions about North American Master, Grandmaster and Challenger solo queue players:

1. Which champions do they **main** (30% or more of their games) and **one-trick** (70% or more) most, and how do those players do on them?
2. Do mains win more on their champion than other high-ranked players on the same champion?

I collected the data myself through the Riot Games API: 500 players sampled from all three ladders, their 30 most recent ranked games, and **12,211 matches** with all 10 participants stored in SQLite.

## Key findings
- **Two thirds of Master+ players have a main.** 331 of 500 players play at least 30% of their games on one champion, and 122 are one-tricks.
- **Mains are spread out.** 136 of 173 champions have at least one main. Talon and Yone have the most mains; Katarina has the most one-tricks.
- **Mains win 7.3 percentage points more** than other sampled players on the same champion (95% interval +5.6 to +9.2). One-tricks show no extra edge over mains.

<img class="img-fluid" src="../img/mainstats/most-mained.png">

<img class="img-fluid" src="../img/mainstats/win-rate-by-group.png">

## How I built it
- **Data collection (Python):** an API client that respects rate limits and retries on errors, plus resumable scripts that download players, match lists, matches and match timelines.
- **SQL:** player labels and the per-champion table are computed in SQL with CTEs, window functions, conditional aggregation and self-joins (for lane opponents), so thresholds can change without collecting new data.
- **Statistics (pandas, matplotlib):** a player-level bootstrap for confidence intervals, since games from the same player are not independent, and a within-champion comparison so champions with high win rates do not create a fake difference.
- **Website (React, TypeScript, Vite):** a sortable champion table with a role filter, and a page for each champion with builds, players and counters.

## What I learned
The most important lesson was about **selection bias**. The sampled players win more than 50% because they were chosen from the current ladder, so a raw "mains win more" result would be misleading. I compared mains with other players from the same sample instead of with everyone, measured the effect by tier, and wrote up what the data can and cannot prove. I also learned how much faster SQL is for this kind of analysis than looping in Python, and how to plan a data pipeline that can be stopped and resumed safely.

## Link
[GitHub Repository](https://github.com/mtossey/Onetricks)
