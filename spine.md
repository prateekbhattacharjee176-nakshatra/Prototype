# Nakshatra AI — Engineering Layer README

## 1. Overview

This document defines the core AI engineering stack for Nakshatra AI.

The system is built using pre-trained LLMs + custom intelligence layers, not by training models from scratch.

### Core Layers

1. LLM Layer (Gemini / GPT)
2. Astrology Brain Layer
3. Personality & Memory Layer
4. Prompt Engineering Layer
5. RAG (Retrieval Augmented Generation)

---

## 2. LLM Layer

### Providers

- Google — Gemini (Primary)
- OpenAI — GPT (Fallback / Premium)

### Responsibilities

- Natural language understanding
- Response generation
- Emotional tone handling

### Design

- Stateless usage
- No fine-tuning initially
- Controlled via prompts + context injection

### Model Routing (Concept)

```python
def route_model(intent, usage_level):
    if intent == "deep_astrology" or intent == "emotional":
        return "premium_model"
    elif usage_level > threshold:
        return "light_model"
    return "standard_model"
```

---

## 3. Astrology Brain Layer

### Purpose

Provide deterministic intelligence (non-AI) for astrology logic.

### Responsibilities

- Birth chart generation
- Planetary calculations
- Dasha system
- Rule-based interpretation

### Implementation

- Python modules
- JSON/YAML rule definitions

### Example Structure

```json
{
  "saturn_in_7th": {
    "meaning": "delayed relationships",
    "advice": "focus on long-term compatibility"
  }
}
```

### Output

Structured astrology context:

```json
{
  "current_dasha": "Saturn",
  "insights": ["delay in career growth", "need for discipline"]
}
```

---

## 4. Personality & Memory Layer

### Purpose

Enable long-term personalization and emotional continuity.

### Data Stored

- User preferences
- Emotional states
- Relationship history
- Key life events
- Summarized chat history

### Memory Types

1. **Short-term memory**
   - Current session
   - Stored in Redis
2. **Long-term memory**
   - Persistent summaries
   - Stored in DB (PostgreSQL)

### Memory Compression

- Raw chat → summarized context
- Prevents token explosion

### Example

```json
{
  "user_profile": {
    "mood_pattern": "anxious about career",
    "relationship_status": "recent breakup",
    "goals": ["startup success"]
  }
}
```

---

## 5. Prompt Engineering Layer

### Purpose

Control behavior, tone, and output quality of LLM.

### Structure

#### System Prompt Template

```text
You are Nakshatra AI, an emotionally intelligent astrologer.
You combine astrology with practical life advice.
You are calm, supportive, and direct.
```

#### Dynamic Injection

- User input
- Memory summary
- Astrology insights
- Retrieved knowledge

#### Final Prompt Format

```text
[System Role]
[User Memory Summary]
[Astrology Context]
[User Message]
```

### Responsibilities

- Tone consistency
- Context fusion
- Guardrails (avoid hallucination)

---

## 6. RAG (Retrieval Augmented Generation)

### Purpose

Inject domain knowledge without training models.

### Data Sources

- Astrology rules
- Interpretations
- Pre-written guidance templates

### Pipeline

1. Convert data into embeddings
2. Store in vector database
3. Retrieve relevant chunks per query
4. Inject into prompt

### Retrieval Flow

```python
query_embedding = embed(user_query)
results = vector_db.search(query_embedding, top_k=5)
context = combine(results)
```

### Benefits

- Accurate responses
- Updatable knowledge
- Reduced hallucination

---

## 7. End-to-End Flow

```text
User Input
   ↓
Fetch Memory
   ↓
Run Astrology Engine
   ↓
Retrieve Knowledge (RAG)
   ↓
Build Prompt
   ↓
Route Model (Gemini / GPT)
   ↓
Generate Response
   ↓
Store + Summarize Memory
   ↓
Return Output
```

---

## 8. Key Design Principles

1. **AI is not the product**
   - LLM = interface
   - Logic + memory = product

2. **Deterministic + Generative Hybrid**
   - Astrology → deterministic
   - Conversation → generative

3. **Cost Control**
   - Context compression
   - Model routing
   - Limited token usage

4. **Personalization First**
   - Memory layer drives retention

---

## 9. Future Extensions

- Fine-tuned domain model
- Emotion detection layer
- Multi-modal inputs (voice, image)
- Personalized reasoning engine

---

## 10. Summary

Nakshatra AI’s engineering stack is built on:

- Pre-trained LLMs (Gemini / GPT)
- Deterministic astrology logic
- Persistent personality memory
- Structured prompt orchestration
- Retrieval-based knowledge injection
