# X Search

Search X/Twitter for real-time conversation on any topic. Costs 5 credits per search.

## Setup

```bash
TOKEN=$(node -e "console.log(require('$HOME/.atris/credentials.json').token)")
```

## Search

```bash
curl -s -X POST "https://api.atris.ai/api/x-search/search" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"query": "your search terms", "limit": 10}'
```

With date filter (last N days):
```bash
-d '{"query": "your search terms", "limit": 10, "days_back": 7}'
```

## Query tips

| Goal | Example |
|------|---------|
| Exact phrase | `"revenue operations"` |
| OR logic | `"CRM is dead" OR "Salesforce alternative"` |
| From a user | `from:levelsio` |
| High engagement | `"AI agents" min_faves:50` |
| Exclude retweets | `"your query" -is:retweet` |

## When to use

- Before writing anything. See what people are actually saying.
- To validate an angle. Is anyone talking about this? How?
- To find proof points. Real tweets beat hypothetical examples.
