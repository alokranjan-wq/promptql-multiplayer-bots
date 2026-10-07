# Meeting Kanban — app

Reference implementation deployed by the Meeting Kanban bot. See [SPEC.md](SPEC.md) for the data model, API and look-and-feel.

```
cd web && npm ci && npm run build && cd ..   # emits ../dist
uv run --script server.py                    # serves dist/ and the API on $PORT (default 8080)
```

On first start, if `data/board.json` is absent the server copies `data/board.sample.json` (fictional data). Replace `data/board.json` with the extracted plan for a real meeting. `/readyz` returns 204 when the app is ready.