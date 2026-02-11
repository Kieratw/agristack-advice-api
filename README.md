<p align="center">
  <img src="https://img.shields.io/badge/python-3.11+-blue?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/Google%20Gemini-8E75B2?style=for-the-badge&logo=google&logoColor=white" />
</p>

# AgriStack Advice API

Backend API do generowania rekomendacji ochrony roślin w oparciu o **Google Gemini** z dostępem do internetu (grounding). Część systemu [AgriStack](https://github.com/TWOJ_USER/agristack) — aplikacja mobilna komunikuje się z tym serwerem, wysyłając dane o uprawie i stanie zdrowotnym, a w odpowiedzi otrzymuje konkretne środki ochrony roślin z dawkami w formacie JSON.

---

## API

| Metoda | Ścieżka | Opis |
|--------|---------|------|
| `GET` | `/` | Health check |
| `POST` | `/api/advice` | Rekomendacja ochrony roślin |

### Request body (`/api/advice`)

| Parametr | Typ | Wymagany | Opis |
|----------|-----|----------|------|
| `crop` | `string` | ✅ | Nazwa uprawy (np. `"pszenica ozima"`) |
| `status` | `string` | ✅ | Stan zdrowotny / choroba (np. `"healthy"`, `"septorioza"`) |
| `bbch` | `string` | ❌ | Faza rozwojowa BBCH (np. `"37-39"`) |
| `season_context` | `string` | ❌ | Kontekst sezonu (np. `"maj, T2"`) |
| `time_since_last_spray_days` | `int` | ❌ | Dni od ostatniego oprysku |
| `situation_description` | `string` | ❌ | Opis warunków (np. `"wilgotno, po deszczach"`) |

### Przykładowa odpowiedź

```json
{
  "summary": "W fazie T2 przy wilgotnych warunkach zalecany zabieg fungicydowy...",
  "products": [
    {
      "name": "Adexar Plus",
      "dose": "1,5 l/ha",
      "store_talk_hint": "Proszę Adexar Plus na pszenicę, faza T2"
    }
  ],
  "sources": ["https://wiescirolnicze.pl/...", "https://farmer.pl/..."],
  "disclaimer": "Dane orientacyjne. Sprawdź etykietę przed użyciem."
}
```

---

## Uruchomienie

Wymaga klucza [Google Gemini API](https://aistudio.google.com/apikey).

Wpisz swój klucz w pliku `.env`, a potem:

```bash
pip install -r requirements.txt
python main.py
```

Serwer startuje na `http://localhost:8000`. Swagger UI: `http://localhost:8000/docs`

---

## Zmienne środowiskowe

| Zmienna | Opis | Wymagana | Domyślna |
|---------|------|----------|----------|
| `GEMINI_API_KEY` | Klucz API Google Gemini | ✅ | — |
| `PORT` | Port serwera | ❌ | `8000` |

---

## Technologie

- **[FastAPI](https://fastapi.tiangolo.com/)** — framework API
- **[Google Gemini](https://ai.google.dev/)** — model AI z grounding
- **[Pydantic](https://docs.pydantic.dev/)** — walidacja danych
