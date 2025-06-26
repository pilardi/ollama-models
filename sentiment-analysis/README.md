# Sentiment Analyis models

These [models](https://ollama.com/pilardi/sentiment-analysis) are customized to provide sentiment analysis in the form a json objects, they also supports aspect sentiment by pre-pending \<aspec\> to the give text.

## Schema
```json
{
  "type": "object",
  "properties": {
    "sentiment": {
      "description": "A floating-point number representing the sentiment of the text, ranging from -1.0 (negative) to 1.0 positive and 0.0 being neutral",
      "type": "number",
      "maximum": 1.0,
      "minimum": -1.0
    },
    "confidence": {
      "description": "A floating-point representation of how confident you are about this sentiment, ranging from 0.0 (not confident) to 1.0 (certain)",
      "type": "number",
      "maximum": 1.0,
      "minimum": 0
    },
    "reasoning": {
      "description": "An optional brief reasoning of the logic used to determine the numeric sentiment value",
      "type": "string"
    }
  },
  "required": [
    "sentiment",
    "confidence"
  ]
}
```
## Examples:

### Regular Sentiment:
```
curl -s http://localhost:11434/api/generate -d '{
  "model": "pilardi/sentiment-analysis:gemma3",
  "prompt": "I love my pizza without pinapples",
  "stream": false,
  "format": "json"
}' | jq -c ".response | fromjson"
```
Response
```json
{"sentiment":0.90,"confidence":0.95,"reasoning":"The text expresses a strong positive sentiment towards pizza, explicitly stating a preference against pineapple."}
```

### Aspect Sentiment:
```
curl -s http://localhost:11434/api/generate -d '{
  "model": "pilardi/sentiment-analysis:gemma3",
  "prompt": "<pinapples>I love my pizza without pinapples",
  "stream": false,
  "format": "json"
}' | jq -c ".response | fromjson"
```
Response:
```json
{"sentiment":-1.0,"confidence":0.95,"reasoning":"The text expresses a clear dislike for pineapple on pizza."}
```