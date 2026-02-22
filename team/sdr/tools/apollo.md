# Apollo API

REST API for lead enrichment and prospecting.

## Base URL

```
https://api.apollo.io/v1
```

## Authentication

API key via header: `X-Api-Key: {APOLLO_API_KEY}`

## Endpoints

### Search people

```
POST /mixed_people/search
```

Find contacts matching criteria (title, company size, industry).

### Enrich person

```
POST /people/match
```

Get full profile from email or LinkedIn URL. Returns: name, title, company, phone, social links.

### Enrich organization

```
GET /organizations/enrich?domain={domain}
```

Company details from domain. Returns: size, industry, funding, tech stack.
