# Agentic RAG System Using CrewAI & LangChain

This project implements an advanced Retrieval-Augmented Generation (RAG) system using multiple AI agents to improve information retrieval and response quality. The system uses CrewAI for agent orchestration and LangChain for RAG functionality.

## Features

- Multi-agent architecture for sophisticated RAG operations
- Automated routing between vector store and web search
- Multiple validation layers to ensure response quality
- Hallucination detection and prevention
- PDF and web search capabilities

## Prerequisites

```bash
pip install crewai==0.28.8 crewai_tools==0.1.6 langchain_community==0.0.29 sentence-transformers langchain-groq
```

You'll also need the following API keys:
- Groq API key
- Tavily API key

## Agent Architecture

The system implements five specialized agents:

### 1. Router Agent
- **Role**: Routes questions to appropriate search methods
- **Function**: Determines whether to use vector store or web search
- **Goal**: Optimize information retrieval path

### 2. Retriever Agent
- **Role**: Executes information retrieval
- **Function**: Uses either vector store or web search based on router's decision
- **Goal**: Extract relevant information for the given question

### 3. Grader Agent
- **Role**: Validates retrieval relevance
- **Function**: Evaluates if retrieved content matches the question
- **Goal**: Filter out irrelevant retrievals

### 4. Hallucination Grader Agent
- **Role**: Prevents hallucinations
- **Function**: Verifies if answers are factually grounded
- **Goal**: Ensure response accuracy

### 5. Answer Grader Agent
- **Role**: Final quality control
- **Function**: Reviews and refines answers
- **Goal**: Ensure responses are useful and accurate

## Task Pipeline

1. **Routing Task**: Analyzes question keywords and determines search method
2. **Retrieval Task**: Executes search and extracts information
3. **Grading Task**: Evaluates retrieval relevance
4. **Hallucination Check**: Verifies factual accuracy
5. **Answer Quality**: Ensures response usefulness

## Usage

```python
# Initialize the crew with agents and tasks
rag_crew = Crew(
    agents=[Router_Agent, Retriever_Agent, Grader_agent, 
            hallucination_grader, answer_grader],
    tasks=[router_task, retriever_task, grader_task, 
           hallucination_task, answer_task],
    verbose=True,
)

# Execute the pipeline
inputs = {"question": "Your question here"}
result = rag_crew.kickoff(inputs=inputs)
```

## Agent Flow

```
Question Input
    │
    ▼
Router Agent
    │
    ├─► Vector Store Search
    │         or
    ├─► Web Search
    │
    ▼
Retriever Agent
    │
    ▼
Grader Agent
    │
    ▼
Hallucination Grader
    │
    ▼
Answer Grader
    │
    ▼
Final Response
```

## Key Benefits

1. **Improved Accuracy**: Multiple validation layers reduce errors
2. **Flexible Retrieval**: Smart routing between different information sources
3. **Quality Control**: Systematic checks for relevance and factuality
4. **Hallucination Prevention**: Dedicated agent for fact verification
5. **Adaptive Responses**: Falls back to web search if vector store results are inadequate

## Notes

- The system uses Groq's LLM for agent operations
- PDF search capability is implemented using LangChain
- Web search is handled through the Tavily API
- All agents operate with verbose logging for debugging

## Customization

You can modify agent behaviors by adjusting:
- Agent backstories
- Task descriptions
- Expected outputs
- Tool configurations
- LLM parameters

## Limitations

- Requires multiple API keys
- Sequential processing may impact response time
- Dependent on external service availability
- Memory usage scales with PDF size
