# NetGrind MCP server

Connect NetGrind to ChatGPT, Claude and any AI agent through Cotlet.dev. Work in your own Game Master account with 3 hosted tools for the Judgement Hall, Player progress and Shop rewards. Both write tools ask for approval by default.

## What you can ask

> List the submissions waiting in my Judgement Hall.

> Show which Players are stuck or quiet.

> List the rewards in my Shop and their Gold prices.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### ChatGPT

Settings → Apps & Connectors → Create, with connector URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev. No API key needed.

### Claude

Settings → Connectors → Add custom connector, with URL `https://cotlet.dev/mcp/v1/connect`, then sign in with Cotlet.dev.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its NetGrind tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

Run the included Python module with Python 3.10 or later:

```bash
python3 netgrind_agent.py serve
```

Or with Docker:

```bash
docker build -t netgrind-mcp .
docker run -i --rm -e COTLET_API_KEY netgrind-mcp
```

Tools: `netgrind_guide`, `netgrind_status`, `netgrind_open`, `netgrind_read`, `netgrind_click`, `netgrind_type`, `netgrind_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, with no third-party runtime dependencies. Clone this repository and run:

```bash
export COTLET_API_KEY="your-key"
python3 netgrind_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 netgrind_agent.py status    # is NetGrind ready in your Cotlet.dev account
python3 netgrind_agent.py guide     # NetGrind screens and sign-in link
python3 netgrind_agent.py read      # open NetGrind, read the page, close the browser
python3 netgrind_agent.py tools     # list Cotlet.dev tools
python3 netgrind_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from NetGrind users

**Which NetGrind account should I connect?**

Sign in at https://play.netgrind.online/login in your Cotlet Cloud Browser with your Game Master account. Complete Discord, Google or email sign-in yourself.

**Which actions ask for approval?**

review_submissions and manage_shop_rewards ask for approval by default. track_player_progress only reads.

**Is Cotlet affiliated with NetGrind?**

No. Cotlet.dev is independent and not affiliated with NetGrind.

## Learn more

- [NetGrind integration and hosted tools](https://cotlet.dev/integrations/netgrind-api-mcp-server)

---

More: https://cotlet.dev/integrations/netgrind-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=netgrind

Request access: https://cotlet.dev/signup?app=netgrind&utm_source=github&utm_medium=readme&utm_campaign=netgrind

## Workflows on the hosted integration

The live integration page documents these 3 operations:

- `review_submissions`
- `track_player_progress`
- `manage_shop_rewards`

The local stdio adapter exposes seven browser helpers listed above. These helpers forward to Cotlet.dev; the workflow operations are documented on the hosted integration page.
