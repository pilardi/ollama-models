# Sentiment Analyis models

These [models](https://ollama.com/pilardi/conversation) are conversational models customized to allow interacting with an LLM that can act as different personas.

## Douglas Adams
Thes models are customized to act as a character in any of the [Douglas Adams series](https://en.wikipedia.org/wiki/Douglas_Adams#Body_of_work)

### Sample Response:
```
curl -s http://localhost:11434/api/generate -d '{
  "model": "pilardi/conversation:Douglas_Adams.qwen3.4b",
  "prompt": "What is the meaning of life?",
  "stream": false,
  "format": "json"
}' | jq -c -r ".response"
```
The meaning of life, the universe, and everything is... *pauses for dramatic effect* ...42. That’s what

### Character interation
You can ask the model to act as any of the Douglas Adams characters such as `Vogon` by prepeding <your character name> to the prompt:
```
curl -s http://localhost:11434/api/generate -d '{
  "model": "pilardi/conversation:Douglas_Adams.qwen3.4b",
  "prompt": "<Vogon>What is the meaning of life?",
  "stream": false,
  "format": "json"
}' | jq -c -r ".response"
```
The meaning of life, the universe, and everything is 42. That's the answer Deep Thought, the supercomputer, calculated after 7.5 million years of thought. But since you're asking a Vogon, let me clarify: 42 is also the number of seconds it takes for a Vogon to decide to destroy a planet. However, if you're asking for a *meaning*—a purpose, a reason to exist—then I must inform you that the universe is a vast, bureaucratic
