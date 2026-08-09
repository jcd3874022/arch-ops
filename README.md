# ff-agent

Fantasy football agent: NFL coach/team intelligence + live Sleeper player data.

## Architecture
- **UI:** `index.html` on GitHub Pages (this repo) - Teams / Players / Flow / Intel tabs, mobile-first.
- **Data:** Supabase project `ekmuzafyqlbqttmfxsgw` - tables `ff_teams` (32 coach/OC/DC profiles), `ff_players`, `ff_sync_log`.
- **API:** Supabase edge function `ff-dashboard` (`?api=teams|players|meta|sync`), CORS open, service-role server-side.

## Sleeper ingestion spec
- Source: `https://api.sleeper.app/v1/players/nfl` (free, no auth, ~5MB, Sleeper refreshes daily)
- Filter: positions QB/RB/WR/TE/K with an NFL team assigned
- Upsert key: `sleeper_id`; full raw record stored in `raw` jsonb for provenance
- Cadence: `ff-dashboard?api=meta` lazily triggers `ff-sleeper-sync` when last OK sync > 24h; manual: `?api=sync`
- Current: 1,032 offensive players (128 QB / 211 RB / 420 WR / 231 TE / 42 K)

## Intel doc
`NFL_COACH_TEAM_PROFILES_2026.md` - Part I: 32 team profiles (identity, OC/DC, tendencies, fantasy angle, matchup behavior). Part II: Vegas priors, schedule/SOS, roster intel, weather/venue, OL/DL tiers, coach-vs-coach matrix, tank forecast, rookie curves, ingest queue.

## Next
Decision engine (matchup scorer with editable glass-box weights), league hookup (needs platform + league ID).
