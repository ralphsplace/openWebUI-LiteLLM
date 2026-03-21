
## OpenWebUI and LiteLLM

A very Local Hosted approach to the home lab and AI

Thank you - to the folks at Open Web UI and LiteLLM for making this possible.

Validating LLMs are sup and running.

```
curl http://localhost:4000/v1/models -H "Content-Type: application/json"   -H "Authorization: Bearer ${LITELLM_API_KEY}"

curl -s ${OLLAMA_SRC1}/api/tags

curl -s $(OLLAMA_SRC2)/api/tags
```