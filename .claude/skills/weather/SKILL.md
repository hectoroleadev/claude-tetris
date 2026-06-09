---
name: weather
description: >
  Consulta el clima actual y pronóstico del tiempo local usando wttr.in.
  Usar cuando el usuario pregunta por el clima, temperatura, lluvia, viento,
  o pronóstico. Frases trigger: "/weather", "clima", "¿qué clima hace?",
  "cómo está el clima", "va a llover", "temperatura", "forecast", "weather",
  "how's the weather", "pronóstico".
---

Fetch weather using `curl` from wttr.in. No API key needed. Steps:

## Detecting location

If the user provides a city/location in their message, use it directly.
If no location is given, use auto-detection (wttr.in detects by IP):

```bash
curl -s 'wttr.in/?format=v2'
```

For an explicit city (URL-encode spaces as `+`):
```bash
curl -s 'wttr.in/Ciudad+de+Mexico?format=v2'
```

## Formats

- `?format=v2` — Clean multi-line ASCII output with current conditions + 3-day forecast. **Use this by default.**
- `?format=j1` — JSON with full detail (wind, humidity, UV, hourly). Use only if the user asks for detailed data.
- `?format=1` — Single line: `⛅ +22°C` (for quick inline responses).

## How to respond

1. Run the curl command via Bash tool.
2. Present the raw `v2` output in a code block — it's already formatted for terminal display.
3. Add a 1-sentence summary: current conditions, temperature, and any notable weather (rain, wind, storm).
4. If the user asked about a specific aspect (e.g., "¿va a llover?"), answer that directly after the block.

## Error handling

- If curl fails or returns an error page, tell the user wttr.in may be unreachable and suggest checking `curl wttr.in` manually.
- If location is ambiguous (multiple cities match), show the result and note which location wttr.in resolved to (it's shown in the output header).
