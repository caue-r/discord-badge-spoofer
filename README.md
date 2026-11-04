# discord-badge-spoofer

Inflates the playtime and games-played stats on your Discord profile.

## How to run

Requires Python 3.10+. No dependencies (stdlib only).

```
python spoofer.py
```

1. Paste your Discord account token when asked. It is saved to `science_state.json`.
2. Paste your `cookie` header (needs `cf_clearance`). Copy it from any `/science` request in the client's devtools (Network tab).
3. Pick an option from the menu:
   - `1` claim games: choose how many games and how many hours per game
   - `2` refresh auth: fetch a new analytics token and update the cookie
   - `3` set fingerprint (optional, Windows only): generate an `executable_fingerprint` from a game's `.exe` or PID
   - `4` exit

Badges update on Discord's analytics cycle, which takes 1-2 days.

## How it works

The Discord client reports game activity to `/api/v9/science` using `launch_game` and `running_game_heartbeat` events. This script sends the same events with whatever games and durations you choose. `duration_tracked_ms` is not validated, so Discord accepts the values (`204`).

- The analytics token comes from `GET /users/@me?with_analytics_token=true` and is cached for 12h.
- Client properties come from `cordapi.dolfi.es`, with a local fallback.
- The game list is downloaded from Discord's CDN (`detectables/games.json`).
- Events go out in batches of 50, one batch per second.

## Risks

- **This violates Discord's Terms of Service.** Your account can be flagged or banned. Only use an account you are willing to lose.
- **Your token gives full access to your account.** It is stored in plain text in `science_state.json`. Keep that file private and never share it.
- It can stop working at any time if Discord patches the endpoint.
- Provided as-is, with no warranty.

## License

MIT. See [LICENSE](LICENSE).
