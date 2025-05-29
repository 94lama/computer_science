# API
Le API di OpenAI sono particolarmente utili in quanto costituiscono il *de facto* standard per i vari modelli di IA.

## [Python](Python.md)
### Messaggi
```message
message = {
	"role": "user",
	"content": "testo inviato/ricevuto"
}
```

Dove:
- **role** contiene il ruolo dell'utente che sta interagendo con l'AI
- **content** contiene il messaggio di richiesta/risposta
### Login
```login.py
import os, openai
# ollama configuratin
host = os.getenv("OLLAMA_HOST")
api_key = os.getenv("AUTH")
base_url = f"https://{api_key}@{host}/v1"
# accessing to the server
client = openai.OpenAI(
	base_url = base_url,
	api_key = api_key,
)
```

### openai
Rappresenta la classe 
#### OpenAI
```openai.OpenAI
{
	chat: {
		completions
	}
}
```