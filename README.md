<p align="center">
  <img src="https://img.shields.io/badge/python-3.11+-blue?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/Google%20Gemini-8E75B2?style=for-the-badge&logo=google&logoColor=white" />
</p>

# AgriStack Advice API

Backend API for generating structured crop protection recommendations based on grounded web information using Google Gemini with web grounding. It is part of the AgriStack system — the mobile application sends crop, plant health, and situational data to this service, which returns structured recommendations together with relevant product information, suggested application rates, source references, and safety disclaimers in JSON format.

---

## API

| Method | Endpoint      | Description                     |
| ------ | ------------- | ------------------------------- |
| `GET`  | `/`           | Health check                    |
| `POST` | `/api/advice` | Generate crop protection advice |

### Request body (`/api/advice`)

| Parameter                    | Type     | Required | Description                                                           |
| ---------------------------- | -------- | -------- | --------------------------------------------------------------------- |
| `crop`                       | `string` | ✅        | Crop name, e.g. `"winter wheat"`                                      |
| `status`                     | `string` | ✅        | Plant health status or disease, e.g. `"healthy"` or `"septoria"`      |
| `bbch`                       | `string` | ❌        | BBCH growth stage, e.g. `"37-39"`                                     |
| `season_context`             | `string` | ❌        | Seasonal context, e.g. `"May, T2"`                                    |
| `time_since_last_spray_days` | `int`    | ❌        | Number of days since the last crop protection treatment               |
| `situation_description`      | `string` | ❌        | Additional field conditions, e.g. `"humid conditions after rainfall"` |

### Example response

```json
{
  "summary": "At the T2 growth stage under humid conditions, a fungicide treatment may be recommended...",
  "products": [
    {
      "name": "Adexar Plus",
      "dose": "1.5 l/ha",
      "store_talk_hint": "Adexar Plus for wheat at the T2 growth stage"
    }
  ],
  "sources": [
    "https://wiescirolnicze.pl/...",
    "https://farmer.pl/..."
  ],
  "disclaimer": "Information is provided for guidance only. Always verify the current product label before use."
}
```

---

## Setup

A [Google Gemini API](https://aistudio.google.com/apikey) key is required.

Add your API key to a `.env` file:

```env
GEMINI_API_KEY=your_gemini_api_key
```

Then install the dependencies and start the server:

```bash
pip install -r requirements.txt
python main.py
```

The API runs by default at:

```text
http://localhost:8000
```

Interactive Swagger documentation is available at:

```text
http://localhost:8000/docs
```

---

## Environment Variables

| Variable         | Description           | Required | Default |
| ---------------- | --------------------- | -------- | ------- |
| `GEMINI_API_KEY` | Google Gemini API key | ✅        | —       |
| `PORT`           | Server port           | ❌        | `8000`  |

---

## Technologies

* **[FastAPI](https://fastapi.tiangolo.com/)** — REST API framework
* **[Google Gemini](https://ai.google.dev/)** — AI model with web grounding
* **[Pydantic](https://docs.pydantic.dev/)** — request and response validation
