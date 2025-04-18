# IPL-Player-Performance
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Load data
matches_df = pd.read_csv("matches.csv")
deliveries_df = pd.read_csv("deliveries.csv")

# Set general style
sns.set(style="whitegrid")
plt.rcParams['figure.figsize'] = (12, 6)

# Custom vibrant palettes
vibrant_palette = sns.color_palette("tab10")
bright_palette = sns.color_palette("Set2")

# 1. Matches Played Per Season
matches_per_season = matches_df['season'].value_counts().sort_index()
plt.figure()
sns.barplot(x=matches_per_season.index, y=matches_per_season.values, palette=vibrant_palette)
plt.title("Matches Played Per Season", fontsize=16, fontweight='bold')
plt.xlabel("Season")
plt.ylabel("Number of Matches")
plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# 2. Top 10 Run Scorers
top_scorers = deliveries_df.groupby('batter')['batsman_runs'].sum().sort_values(ascending=False).head(10)
plt.figure()
sns.barplot(x=top_scorers.values, y=top_scorers.index, palette=bright_palette)
plt.title("Top 10 Run Scorers (2008–2024)", fontsize=16, fontweight='bold')
plt.xlabel("Runs")
plt.ylabel("Batter")
plt.tight_layout()
plt.show()

# 3. Top 10 Wicket Takers
valid_dismissals = ['bowled', 'caught', 'lbw', 'stumped', 'caught and bowled', 'hit wicket']
wickets_df = deliveries_df[deliveries_df['dismissal_kind'].isin(valid_dismissals)]
top_wicket_takers = wickets_df['bowler'].value_counts().head(10)
plt.figure()
sns.barplot(x=top_wicket_takers.values, y=top_wicket_takers.index, palette=sns.color_palette("rocket_r"))
plt.title("Top 10 Wicket Takers (2008–2024)", fontsize=16, fontweight='bold')
plt.xlabel("Wickets")
plt.ylabel("Bowler")
plt.tight_layout()
plt.show()

# 4. Most Successful Teams
top_teams = matches_df['winner'].value_counts().head(10)
plt.figure()
sns.barplot(x=top_teams.values, y=top_teams.index, palette=sns.color_palette("cool"))
plt.title("Most Successful Teams", fontsize=16, fontweight='bold')
plt.xlabel("Wins")
plt.ylabel("Team")
plt.tight_layout()
plt.show()

# 5. Win Count by Toss Decision
toss_decision_wins = matches_df[matches_df['toss_winner'] == matches_df['winner']]['toss_decision'].value_counts()
plt.figure()
sns.barplot(x=toss_decision_wins.index, y=toss_decision_wins.values, palette=sns.color_palette("Set1"))
plt.title("Matches Won by Toss Decision", fontsize=16, fontweight='bold')
plt.xlabel("Toss Decision")
plt.ylabel("Wins")
plt.tight_layout()
plt.show()

# 6. Venue-wise Match Counts (Top 10)
venue_counts = matches_df['venue'].value_counts().head(10)
plt.figure()
sns.barplot(x=venue_counts.values, y=venue_counts.index, palette=sns.color_palette("crest"))
plt.title("Top 10 Venues by Match Count", fontsize=16, fontweight='bold')
plt.xlabel("Number of Matches")
plt.ylabel("Venue")
plt.tight_layout()
plt.show()

# 7. Distribution of Result Types
result_counts = matches_df['result'].value_counts()
plt.figure()
plt.pie(result_counts, labels=result_counts.index, autopct='%1.1f%%', startangle=140, colors=sns.color_palette('colorblind'))
plt.title("Match Result Types", fontsize=16, fontweight='bold')
plt.tight_layout()
plt.show()

# 8. Scatter Plot: Runs vs Wickets per Match
match_stats = deliveries_df.groupby('match_id').agg({'total_runs': 'sum', 'is_wicket': 'sum'}).reset_index()
plt.figure()
sns.scatterplot(data=match_stats, x='is_wicket', y='total_runs', hue='total_runs', palette='turbo', size='total_runs', sizes=(20, 200), alpha=0.7)
plt.title("Runs vs Wickets per Match", fontsize=16, fontweight='bold')
plt.xlabel("Wickets")
plt.ylabel("Runs")
plt.tight_layout()
plt.show()

# 9. Pie Chart: Toss Decision Distribution
toss_decisions = matches_df['toss_decision'].value_counts()
plt.figure()
plt.pie(toss_decisions, labels=toss_decisions.index, autopct='%1.1f%%', startangle=140, colors=sns.color_palette('pastel'))
plt.title("Toss Decision Distribution", fontsize=16, fontweight='bold')
plt.tight_layout()
plt.show()

# 10. Pie Chart: Super Over Distribution
super_over = matches_df['super_over'].value_counts()
plt.figure()
plt.pie(super_over, labels=super_over.index, autopct='%1.1f%%', startangle=140, colors=sns.color_palette('deep'))
plt.title("Super Over Matches", fontsize=16, fontweight='bold')
plt.tight_layout()
plt.show()

# 11. Pie Chart: Match Type Distribution
match_types = matches_df['match_type'].value_counts()
plt.figure()
plt.pie(match_types, labels=match_types.index, autopct='%1.1f%%', startangle=140, colors=sns.color_palette('bright'))
plt.title("Match Type Distribution", fontsize=16, fontweight='bold')
plt.tight_layout()
plt.show()
