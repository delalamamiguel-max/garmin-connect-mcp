# garmin-connect-mcp

MCP server for Garmin Connect. Access your fitness, health, and training data from Claude Code, Claude Desktop, Cursor, Windsurf, or any MCP client.

**61 tools** across 7 categories: activities, daily health, trends, sleep, body composition, performance/training, and profile/devices.

API endpoints and authentication flow based on [`python-garminconnect`](https://github.com/cyberjunky/python-garminconnect) by [cyberjunky](https://github.com/cyberjunky).

## Requirements

- Node.js 20+
- A Garmin Connect account (email and password)

## Installation

### Claude Code

```bash
claude mcp add garmin -e GARMIN_EMAIL=you@email.com -e GARMIN_PASSWORD=yourpass -- npx -y @nicolasvegam/garmin-connect-mcp
```

### Claude Desktop

Add to `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "garmin": {
      "command": "npx",
      "args": ["-y", "@nicolasvegam/garmin-connect-mcp"],
      "env": {
        "GARMIN_EMAIL": "you@email.com",
        "GARMIN_PASSWORD": "yourpass"
      }
    }
  }
}
```

### Cursor

Add to `.cursor/mcp.json` in your project root:

```json
{
  "mcpServers": {
    "garmin": {
      "command": "npx",
      "args": ["-y", "@nicolasvegam/garmin-connect-mcp"],
      "env": {
        "GARMIN_EMAIL": "you@email.com",
        "GARMIN_PASSWORD": "yourpass"
      }
    }
  }
}
```

### Windsurf

Add to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "garmin": {
      "command": "npx",
      "args": ["-y", "@nicolasvegam/garmin-connect-mcp"],
      "env": {
        "GARMIN_EMAIL": "you@email.com",
        "GARMIN_PASSWORD": "yourpass"
      }
    }
  }
}
```

### Any MCP Client

Run the server with environment variables:

```bash
GARMIN_EMAIL=you@email.com GARMIN_PASSWORD=yourpass npx -y @nicolasvegam/garmin-connect-mcp
```

The server communicates over stdio using the [Model Context Protocol](https://modelcontextprotocol.io/).

## Available Tools

### Activities (12 tools)
| Tool | Description |
|------|-------------|
| `get_activities` | List recent activities with pagination |
| `get_activities_by_date` | Search activities within a date range |
| `get_last_activity` | Get the most recent activity |
| `count_activities` | Get total number of activities |
| `get_activity` | Summary data for a specific activity |
| `get_activity_details` | Detailed metrics: HR, pace, elevation time series |
| `get_activity_splits` | Per-km or per-mile split data |
| `get_activity_weather` | Weather conditions during activity |
| `get_activity_hr_zones` | Time in each heart rate zone |
| `get_activity_exercise_sets` | Strength training sets (reps, weight) |
| `get_activity_types` | All available activity types |
| `get_progress_summary` | Fitness stats over a date range by activity type |

### Daily Health (14 tools)
| Tool | Description |
|------|-------------|
| `get_daily_summary` | Full daily summary (steps, calories, distance, etc.) |
| `get_steps` | Step count for a date |
| `get_steps_chart` | Intraday step data throughout the day |
| `get_heart_rate` | Heart rate data (resting, max, zones, time series) |
| `get_resting_heart_rate` | Resting heart rate for a date |
| `get_stress` | Stress levels and time series |
| `get_body_battery` | Body Battery energy levels (date range) |
| `get_body_battery_events` | Battery charge/drain events for a day |
| `get_respiration` | Breathing rate data |
| `get_spo2` | Blood oxygen saturation |
| `get_intensity_minutes` | Moderate/vigorous intensity minutes |
| `get_floors` | Floors climbed chart data |
| `get_hydration` | Daily hydration/water intake |
| `get_daily_events` | Daily wellness events |

### Trends (4 tools)
| Tool | Description |
|------|-------------|
| `get_daily_steps_range` | Daily step counts over a date range |
| `get_weekly_steps` | Weekly step aggregates |
| `get_weekly_stress` | Weekly stress aggregates |
| `get_weekly_intensity_minutes` | Weekly intensity minutes |

### Sleep (2 tools)
| Tool | Description |
|------|-------------|
| `get_sleep_data` | Sleep stages, score, bed/wake times |
| `get_sleep_data_raw` | Raw sleep data with HR and SpO2 |

### Body Composition (5 tools)
| Tool | Description |
|------|-------------|
| `get_body_composition` | Weight, BMI, body fat %, muscle mass (date range) |
| `get_latest_weight` | Most recent weight entry |
| `get_daily_weigh_ins` | All weigh-ins for a date |
| `get_weigh_ins` | Weigh-in records over a date range |
| `get_blood_pressure` | Blood pressure readings (date range) |

### Performance & Training (11 tools)
| Tool | Description |
|------|-------------|
| `get_vo2max` | VO2 Max estimate (running/cycling) |
| `get_training_readiness` | Training Readiness score |
| `get_training_status` | Training status and load |
| `get_hrv` | Heart Rate Variability |
| `get_endurance_score` | Endurance fitness score |
| `get_hill_score` | Climbing performance score |
| `get_race_predictions` | 5K/10K/half/full marathon predictions |
| `get_fitness_age` | Estimated fitness age |
| `get_personal_records` | All personal records |
| `get_lactate_threshold` | Lactate threshold HR and pace |
| `get_cycling_ftp` | Functional Threshold Power (cycling) |

### Profile & Devices (13 tools)
| Tool | Description |
|------|-------------|
| `get_user_profile` | User social profile and preferences |
| `get_user_settings` | User settings, measurement system, sleep schedule |
| `get_devices` | Registered Garmin devices |
| `get_device_settings` | Settings for a specific device |
| `get_device_last_used` | Last used device info |
| `get_primary_training_device` | Primary training device |
| `get_device_solar_data` | Solar charging data |
| `get_gear` | All tracked gear/equipment |
| `get_gear_stats` | Usage stats for a gear item |
| `get_goals` | Active goals and progress |
| `get_earned_badges` | Earned badges and achievements |
| `get_workouts` | Saved workouts |
| `get_workout` | Specific workout by ID |

## Authentication

Uses Garmin Connect credentials (email/password) via environment variables. OAuth tokens are cached in `~/.garmin-mcp/` to avoid re-authentication on each request.

### MFA (Multi-Factor Authentication)

If your Garmin account has MFA enabled (required for devices with ECG capabilities), you need to run the interactive setup once before using the MCP server:

```bash
GARMIN_EMAIL='you@email.com' GARMIN_PASSWORD='yourpass' npx -y @nicolasvegam/garmin-connect-mcp setup
```

This will:
1. Log in to Garmin Connect
2. Prompt you for the MFA code sent to your email or authenticator app
3. Save OAuth tokens to `~/.garmin-mcp/`

After setup, the MCP server will use the saved tokens automatically — no MFA prompt needed until the tokens expire. When they do, simply run the setup command again.

## Development

```bash
git clone https://github.com/Nicolasvegam/garmin-connect-mcp.git
cd garmin-connect-mcp
npm install
npm run build
```

To test locally:

```bash
GARMIN_EMAIL=you@email.com GARMIN_PASSWORD=yourpass npm start
```

## Credits

- API endpoints and authentication flow based on [`python-garminconnect`](https://github.com/cyberjunky/python-garminconnect) by [cyberjunky](https://github.com/cyberjunky)

## License

MIT


---

# Owned checkout guide

## Purpose and status

This owned reference fork retains Nicolasvegam’s Garmin Connect MCP README and python-garminconnect attribution. It is a TypeScript stdio MCP server for personal Garmin data. The source now includes write tools beyond the read-oriented categories/count in the retained upstream description; runtime availability and counts were not tested.

## Repository map

[`src/index.ts`](src/index.ts) registers tools and starts stdio transport; [`src/setup.ts`](src/setup.ts) provides interactive authentication. [`src/client/`](src/client/) implements authentication and API requests; [`src/tools/`](src/tools/) and [`src/dtos/`](src/dtos/) define tools and input schemas. [`package.json`](package.json), [`tsup.config.ts`](tsup.config.ts) and [`CLAUDE.md`](CLAUDE.md) describe packaging and contributor context.

## Architecture and integrations

The MCP SDK routes calls through GarminClient. Authentication uses Garmin SSO and OAuth exchanges, including an external OAuth consumer configuration fetch. Tokens and profile information are persisted under `~/.garmin-mcp/`. The entry point registers write tools unconditionally, including activity creation/rename/deletion and health-record writes; this is not a read-only server.

## Setup and environments

Upstream setup specifies Node.js 20+ and a Garmin account; startup requires `GARMIN_EMAIL` and `GARMIN_PASSWORD`. Interactive setup can request MFA, authenticate remotely and save token/profile files. Do not run setup or supply account credentials for documentation review. The package declares version 1.1.0 while server metadata in source says 1.0.0; installed/version compatibility remains unverified.

## Safe commands

For documentation review, use local, read-only Git inspection: `git status --short`, `git ls-files`, `git diff -- README.md`, and `git diff --check`. Inspect scripts as text before considering execution. Installation, build, test, authentication and publication commands shown in the upstream documentation are outside this README-only review. `npm run setup` authenticates against Garmin; `npm start` starts the built server. Build/dev use tsup, and prepublishOnly builds the package. The upstream npx examples download and execute a package; none were used here.

## Testing and validation

The tracked [client test file](src/client/garmin.client.test.ts) explicitly uses the live API and credentials loaded through dotenv. Vitest is a development dependency, but the package has no test script. Do not run this file as an offline smoke test. Only README structure, new relative links, whitespace and preservation were checked; no health data, authentication or API behavior was tested.

## Deployment and rollback

The manifest packages build output as command-line executables; no hosted deployment is established by this checkout. Future installation/upgrade needs a deliberate version and client configuration review. Reverting the README or server version does not reverse Garmin writes, recover deleted activities or remove cached credentials. No package publication occurred here.

## Risks and guardrails

Health, fitness, account credentials and cached tokens are sensitive. Keep them outside Git and review what an agent may retrieve or change before enabling tools. The delete-activity tool describes deletion as permanent; do not invoke tools speculatively. The manifest declares MIT and the upstream README attribution is retained, but this snapshot has no tracked standalone LICENSE file; this guide adds no new licensing terms.

## Related documentation

Read [CLAUDE.md](CLAUDE.md), [package metadata](package.json), [authentication source](src/client/garmin-auth.ts), [write tools](src/tools/write.tools.ts) and the retained upstream setup and attribution above.
