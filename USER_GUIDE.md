# BENCHY User Guide

## The 5W's of BENCHY

### Who
BENCHY is designed for:
- AI/ML engineers comparing LLM performance
- Developers integrating LLMs into applications
- Researchers studying LLM behavior
- Teams evaluating different LLM providers

### What
A benchmarking tool that provides:
- Real-time comparison of different LLM models
- Performance metrics (speed, cost, accuracy)
- Side-by-side analysis of tool calling capabilities
- Autocomplete functionality testing

### When
Use BENCHY when you need to:
- Choose between different LLM providers
- Optimize your LLM integration costs
- Test long chains of tool/function calls
- Compare autocomplete capabilities
- Evaluate model performance for specific use cases

### Where
BENCHY runs locally on your machine, allowing you to:
- Test in your development environment
- Keep sensitive prompts private
- Avoid network latency issues
- Maintain full control over testing parameters

### Why
BENCHY helps you:
- Make data-driven decisions about LLM selection
- Optimize costs across different providers
- Understand performance trade-offs
- Test specific use cases in a controlled environment

## Setup

### Prerequisites
- Python 3.12 or higher
- Node.js and npm/bun/yarn
- API keys for:
  - OpenAI
  - Anthropic
  - Google Cloud (Gemini)

### Installation Steps

1. **API Key Setup**
   - Obtain API keys from:
     - [Anthropic](https://docs.anthropic.com/en/api/getting-started)
     - [Google Cloud](https://ai.google.dev/gemini-api/docs/api-key)
     - [OpenAI](https://help.openai.com/en/articles/4936850-where-do-i-find-my-openai-api-key)

2. **Frontend Setup**
   ```bash
   # Install dependencies (using bun, npm, or yarn)
   bun install   # recommended
   # or npm install
   # or yarn install

   # Start development server
   bun dev
   # or npm run dev
   # or yarn dev
   ```

3. **Backend Setup**
   ```bash
   # Navigate to server directory
   cd server

   # Set up Python environment
   uv sync

   # Configure environment variables
   cp .env.sample .env
   # Edit .env and add your API keys:
   # ANTHROPIC_API_KEY=your_key
   # OPENAI_API_KEY=your_key
   # GEMINI_API_KEY=your_key

   # Start the server
   uv run python server.py
   ```

## Project Structure

### Important Files

#### Configuration Files
- `.env` - Frontend environment variables for API keys
- `server/.env` - Backend environment variables for API keys
- `package.json` - Frontend dependencies and scripts
- `server/pyproject.toml` - Backend dependencies (Python packages)
  ```toml
  dependencies = [
      "anthropic>=0.39.0",    # Anthropic API client
      "flask>=3.0.3",         # Web server
      "llm>=0.17.1",         # LLM interface library
      "openai>=1.54.3",      # OpenAI API client
      "pydantic>=2.9.2",     # Data validation
      "python-dotenv>=1.0.1", # Environment variable management
      "google-generativeai>=0.8.3"  # Gemini API client
  ]
  ```

#### Frontend Components
- `src/store/*` - Vue stores for state management
  - `autocompleteStore.ts` - State for Multi Autocomplete feature
  - `toolCallStore.ts` - State for Tool Call Benchmark feature

- `src/api/*` - API communication layer
  - `autocompleteApi.ts` - Endpoints for autocomplete functionality
  - `toolCallApi.ts` - Endpoints for tool call operations

#### Backend Components
- `server/server.py` - Main Flask server with API routes
  - `/prompt` endpoint for text completions
  - `/tool-prompt` endpoint for tool call operations

- `server/modules/llm_models.py` - Core LLM integration
  - Model management and initialization
  - Token counting and cost calculation
  - Unified interface for all LLM providers

- LLM Provider Implementations:
  - `server/modules/openai_llm.py` - OpenAI-specific implementation
  - `server/modules/anthropic_llm.py` - Anthropic (Claude) implementation
  - `server/modules/gemini_llm.py` - Google Gemini implementation

### File Organization

```
benchy/
├── .env                    # Frontend environment variables
├── package.json           # Frontend dependencies
├── src/
│   ├── api/              # API client code
│   ├── store/            # State management
│   └── components/       # Vue components
└── server/
    ├── .env              # Backend environment variables
    ├── pyproject.toml    # Backend dependencies
    ├── server.py         # Flask server
    └── modules/          # LLM implementations
        ├── llm_models.py # Core LLM logic
        ├── openai_llm.py
        ├── anthropic_llm.py
        └── gemini_llm.py
```


### Environment Variables

Both frontend and backend require environment variables for API keys:

1. Frontend (`.env`):
   ```
   VITE_OPENAI_API_KEY=your_openai_key
   VITE_ANTHROPIC_API_KEY=your_anthropic_key
   VITE_GEMINI_API_KEY=your_gemini_key
   ```

2. Backend (`server/.env`):
   ```
   # Edit .env and add your API keys:
   # ANTHROPIC_API_KEY=your_key
   # OPENAI_API_KEY=your_key
   # GEMINI_API_KEY=your_key

   # Start the server
   uv run python server.py
   ```

## Features

BENCHY offers two main benchmarking tools:

### 1. Tool Call Benchmark
- Designed for testing and comparing LLMs in long chains of tool/function calls (15+ calls)
- Features three tabs:
  - Tool Call: Main benchmarking interface
  - JSON Prompt: Configure prompt templates
  - Notes: Documentation and tips

### 2. Multi Autocomplete Benchmark
- Compare different LLM models for text completion tasks
- Special focus on Claude 3.5 haiku and GPT-4 predictive outputs
- Features three tabs:
  - Benchmark: Main testing interface
  - Prompt: Configure base prompts
  - Notes: Documentation and usage tips

## Supported Models

### Claude Models
- Claude 3.5 Haiku (claude-3-5-haiku-20241022)
- Claude 3.5 Sonnet (claude-3-5-sonnet-20241022)

### Gemini Models
- Gemini 1.5 Pro (gemini-1.5-pro-002)
- Gemini 1.5 Flash (gemini-1.5-flash-002)
- Gemini 1.5 Flash 8B (gemini-1.5-flash-8b-latest)

### GPT Models
- GPT-4o (gpt-4o)
- GPT-4o Mini (gpt-4o-mini)
- GPT-4o Predictive (gpt-4o with predictive output)
- GPT-4o Mini Predictive (gpt-4o-mini with predictive output)


## Tool Call Benchmark

### Available Tools

BENCHY provides three specialized agents for different tasks:

1. **Coder Agent** (`run_coder_agent`)
   - Purpose: Helps with writing, reviewing, or modifying code
   - Use when you need assistance with coding tasks
   - Input: A prompt describing what code needs to be written or modified
   - Example: "Add a new CLI argument 'mode' to main.py"

2. **Git Agent** (`run_git_agent`)
   - Purpose: Assists with git operations and repository management
   - Use when you need help with git operations, commits, or repo management
   - Input: A prompt describing what git operations need to be performed
   - Example: "Commit the changes with message 'Add mode argument'"

3. **Documentation Agent** (`run_docs_agent`)
   - Purpose: Helps create, update, or review documentation
   - Use when you need assistance with documentation tasks
   - Input: A prompt describing what documentation needs to be created or updated
   - Example: "Update the documentation to describe the new 'mode' argument"

### Tool Call Format

Each tool call in the benchmark follows this structure:
```javascript
{
  "tool_name": "one of: run_coder_agent, run_git_agent, run_docs_agent",
  "params": {
    "prompt": "Description of what the agent should do"
  }
}
```

### Getting Started Example

Let's create a benchmark to compare how different LLMs handle a chain of tool calls:

1. **Setup the Test**
   ```javascript
   // Example JSON Prompt Template
   {
     "system_prompt": "You are a helpful assistant that manages code and documentation updates.",
     "user_prompt": "Update the main.py file to add a new feature and document it",
     "expected_tools": [
       {
         "tool_name": "run_coder_agent",
         "params": {
           "prompt": "Add a new CLI argument 'mode' to main.py"
         }
       },
       {
         "tool_name": "run_docs_agent",
         "params": {
           "prompt": "Update the documentation to describe the new 'mode' argument"
         }
       },
       {
         "tool_name": "run_git_agent",
         "params": {
           "prompt": "Commit the changes with message 'Add mode argument'"
         }
       }
     ]
   }
   ```

2. **Run the Benchmark**
   - Open the Tool Call tab
   - Select models to compare (e.g., GPT-4, Claude 3.5, Gemini 1.5)
   - Paste the JSON prompt template
   - Click "Run Benchmark"

3. **Analyze Results**
   The benchmark will show:
   - Execution time for each tool call
   - Success rate of tool call sequences
   - Cost per model
   - Relative price percentage
   - Number of correct tool calls
   - Percent correct

### Example Tool Call Sequences

1. **Code Update Sequence**
   ```javascript
   [
     {
       "tool_name": "run_coder_agent",
       "params": {
         "prompt": "Add error handling to the process_payment function"
       }
     },
     {
       "tool_name": "run_docs_agent",
       "params": {
         "prompt": "Document the new error handling in process_payment"
       }
     }
   ]
   ```

2. **Git Operations Sequence**
   ```javascript
   [
     {
       "tool_name": "run_git_agent",
       "params": {
         "prompt": "Create a new branch called feature/payment-validation"
       }
     },
     {
       "tool_name": "run_coder_agent",
       "params": {
         "prompt": "Add payment validation function"
       }
     },
     {
       "tool_name": "run_git_agent",
       "params": {
         "prompt": "Commit and push the new validation function"
       }
     }
   ]
   ```

### Tips for Tool Call Benchmarking

1. **Chain Planning**
   - Start with simple, single tool calls
   - Gradually increase complexity
   - Ensure tool calls are logically connected
   - Test different combinations of tools

2. **Prompt Design**
   - Be specific in tool call prompts
   - Include clear success criteria
   - Consider dependencies between tools
   - Test edge cases and error scenarios

3. **Performance Optimization**
   - Monitor execution times for each tool
   - Watch for timeout issues in long chains
   - Consider cost implications of tool sequences
   - Use appropriate model for task complexity

## Multi Autocomplete Benchmark

### Getting Started Example

Let's create a benchmark to compare how different models complete code:

1. **Setup the Test**
   ```python
   # Example Base Prompt
   def calculate_fibonacci(n):
     """
     Calculate the nth number in the Fibonacci sequence.
     
     Args:
         n (int): The position in the sequence (starting from 0)
         
     Returns:
         int: The Fibonacci number at position n
     """
     # Implementation here
   ```

2. **Configure the Benchmark**
   - Open the Benchmark tab
   - Select models to test (e.g., Claude 3.5 Haiku, GPT-4o Predictive)
   - Set the base prompt
   - Configure parameters:
     ```json
     {
       "max_tokens": 150,
       "temperature": 0.7,
       "stop_sequences": ["def", "class"]
     }
     ```

3. **Run and Compare**
   - Click "Run Benchmark"
   - Compare completions:
     - Code structure
     - Error handling
     - Edge cases
     - Documentation
   - Review metrics:
     - Response time
     - Token usage
     - Cost per completion

### Example Results Analysis

```
Model: Claude 3.5 Haiku
Time: 1.2s
Cost: $0.0015
Completion:
    if n <= 1:
        return n
    
    a, b = 0, 1
    for _ in range(n):
        a, b = b, a + b
    return a

Model: GPT-4o Predictive
Time: 0.8s
Cost: $0.0025
Completion:
    if n < 0:
        raise ValueError("Input must be non-negative")
    if n <= 1:
        return n
        
    prev, curr = 0, 1
    for _ in range(n-1):
        prev, curr = curr, prev + curr
    return curr
```

### Interface Overview
- **Benchmark Tab**
  - Test multiple LLMs simultaneously
  - Compare completion results
  - View performance metrics

- **Prompt Tab**
  - Set up base prompts
  - Configure completion parameters
  - Save prompt templates

- **Notes Tab**
  - Access feature documentation
  - View implementation details
  - Read usage guidelines

## Known Limitations

1. **Performance Measurements**
   - Network latency to LLM provider servers is not factored into performance measurements

2. **Cost Calculations**
   - Gemini models: Cost calculations don't account for price increases after 128k tokens
   - Caching costs are not included in calculations

3. **Technical Limitations**
   - Uses default settings in [LLM](https://github.com/simonw/llm) and [OpenAI](https://github.com/openai/openai-python) libraries
   - Streaming is disabled
   - Response token limits and other performance optimization techniques are not utilized
   - Models must be manually updated and configured - no dynamic loading
   - All API keys must be manually set up (see `.env.sample`)

## Technical Implementation

BENCHY is built with:
- Vue 3 with TypeScript
- AG Grid for data grid implementation
- CodeMirror 6 for code editing
- UnoCSS for styling

## Tips and Best Practices

### General Usage
- Save your configurations regularly using the "Save" button
- Use the reset function to clear state and start fresh
- Monitor costs when running extensive benchmarks

### Tool Call Benchmarking
- Start with smaller tool call chains before testing longer ones
- Use varied test cases to ensure comprehensive results
- Monitor execution times for timeout prevention

### Autocomplete Benchmarking
- Use consistent prompts across models for fair comparison
- Consider token limits when designing prompts
- Test with different completion lengths

### Cost Management
- Monitor API usage across different models
- Use the cost estimation features before running large tests
- Consider using smaller models for initial testing

### Performance Optimization
- Run benchmarks in batches to avoid rate limits
- Use saved states for consistent testing
- Clear browser cache if experiencing performance issues

Remember to check the "Notes" tab in each benchmark tool for specific implementation details and additional tips for that particular feature.


## Extending BENCHY

### Adding Custom Agents

You can extend BENCHY by adding your own specialized agents. Here's a step-by-step guide:

1. **Create Agent Function**
   In `server/modules/tools.py`, add your agent function:
   ```python
   def run_custom_agent(prompt: str) -> str:
       """Your custom agent implementation"""
       # Add your logic here
       return "agent response"
   ```

2. **Register Agent with LLM Providers**
   In the same file, add your agent to all provider tool lists:

   ```python
   # Add to all three formats
   tool_definition = {
       # Anthropic format
       "name": "run_custom_agent",
       "description": "Description of what your agent does",
       "input_schema": {
           "type": "object",
           "properties": {
               "prompt": {
                   "type": "string",
                   "description": "What the prompt should contain"
               }
           },
           "required": ["prompt"]
       }
   }

   # Add to each provider's list
   anthropic_tools_list.append(tool_definition)
   gemini_tools_list[0]["function_declarations"].append(tool_definition)
   openai_tools_list.append({
       "type": "function",
       "function": tool_definition
   })

   # Update available tools list
   all_tools_list.append("run_custom_agent")
   ```

3. **Use Your Agent**
   In your tool call sequences:
   ```javascript
   {
     "system_prompt": "System context for your agent",
     "user_prompt": "What you want the agent to do",
     "expected_tools": [
       {
         "tool_name": "run_custom_agent",
         "params": {
           "prompt": "Specific instructions for your agent"
         }
       }
     ]
   }
   ```

### Example: Adding a SQL Agent

```python
# 1. Define the agent
def run_sql_agent(prompt: str) -> str:
    """Generate SQL queries based on natural language prompts"""
    # Add SQL generation logic
    return "SELECT * FROM users WHERE status = 'active';"

# 2. Create tool definition
sql_tool = {
    "name": "run_sql_agent",
    "description": "Generate SQL queries from natural language",
    "input_schema": {
        "type": "object",
        "properties": {
            "prompt": {
                "type": "string",
                "description": "Natural language description of the SQL query needed"
            }
        },
        "required": ["prompt"]
    }
}

# 3. Register with providers
anthropic_tools_list.append(sql_tool)
gemini_tools_list[0]["function_declarations"].append(sql_tool)
openai_tools_list.append({
    "type": "function",
    "function": sql_tool
})
all_tools_list.append("run_sql_agent")
```


### Best Practices

1. **Agent Implementation**
   - Keep agent functions focused and single-purpose
   - Handle errors gracefully with clear error messages
   - Return structured responses (e.g., JSON for complex data)
   - Add logging for debugging and monitoring
   - Include type hints and docstrings
   - Follow existing agent patterns for consistency
   ```python
   def run_custom_agent(prompt: str) -> str:
       """
       Run custom agent with error handling and logging.
       
       Args:
           prompt: Natural language instruction
           
       Returns:
           Structured response as string
           
       Raises:
           ValueError: If prompt is invalid
       """
       try:
           # Validate input
           if not prompt:
               raise ValueError("Empty prompt")
               
           # Add logging
           logging.info(f"Running custom agent with prompt: {prompt}")
           
           # Process prompt and generate response
           response = process_prompt(prompt)
           
           # Validate response
           if not response:
               raise ValueError("Empty response")
               
           return response
           
       except Exception as e:
           logging.error(f"Error in custom agent: {str(e)}")
           raise
   ```

2. **Tool Definition**
   - Write clear, specific descriptions of agent purpose
   - Document expected prompt format with examples
   - Include validation rules and constraints
   - Specify required and optional parameters
   - Add usage examples for each provider
   ```python
   tool_definition = {
       "name": "run_custom_agent",
       "description": """
       Process custom tasks with specific format requirements.
       
       Examples:
       - "Analyze data in CSV format"
       - "Generate report with metrics"
       
       Constraints:
       - Input must be < 1000 chars
       - CSV must have headers
       """,
       "input_schema": {
           "type": "object",
           "properties": {
               "prompt": {
                   "type": "string",
                   "description": "Task description following format above",
                   "maxLength": 1000
               }
           },
           "required": ["prompt"]
       }
   }
   ```

3. **Testing**
   - Test with all supported LLM providers
   ```python
   def test_custom_agent():
       # Test with each provider
       providers = ["anthropic", "openai", "gemini"]
       for provider in providers:
           response = run_custom_agent_test(provider)
           assert response is not None
   ```
   
   - Verify error handling
   ```python
   def test_error_handling():
       # Test empty prompt
       with pytest.raises(ValueError):
           run_custom_agent("")
           
       # Test invalid input
       with pytest.raises(ValueError):
           run_custom_agent("invalid format")
   ```
   
   - Check performance impact
   ```python
   def test_performance():
       start = time.time()
       response = run_custom_agent(test_prompt)
       duration = time.time() - start
       
       # Verify response time
       assert duration < 5.0  # Max 5 seconds
       
       # Check token usage
       assert count_tokens(response) < 1000
   ```
   
   - Validate response format
   ```python
   def test_response_format():
       response = run_custom_agent(test_prompt)
       
       # Verify structure
       assert isinstance(response, str)
       assert len(response) > 0
       
       # Check content
       assert "required_field" in response
       assert is_valid_format(response)
   ```

4. **Integration**
   - Register agent with all providers
   - Update tool lists and documentation
   - Add usage examples to user guide
   - Monitor performance metrics
   - Set up error tracking
   - Add logging for debugging

5. **Maintenance**
   - Keep dependencies updated
   - Monitor error rates
   - Track usage patterns
   - Update documentation
   - Review performance metrics
   - Test with new LLM versions

Remember to follow these practices to ensure your custom agents integrate smoothly with BENCHY and provide reliable, maintainable functionality.
