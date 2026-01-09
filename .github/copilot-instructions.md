# Copilot Instructions for Norman AI Bot Demo

## Project Overview

This is a mini-RAG (Retrieval-Augmented Generation) quickstart project that demonstrates how to build a RAG implementation integrating Azure OpenAI and CosmosDB into a Microsoft Teams channel. The solution allows users to ask questions about sample data stored in a database via Teams chat.

### Architecture

The system consists of the following components:
- **Teams**: User interface for chat interactions
- **Logic App**: Shuttles messages between Teams and Azure Functions
- **Azure Function**: Reads from CosmosDB, creates prompts, and calls OpenAI
- **CosmosDB**: Stores facts/data used to augment chat requests
- **Azure OpenAI**: LLM that processes enhanced requests and formulates responses

## Technology Stack

- **Language**: Python 3.10/3.11
- **Cloud Platform**: Microsoft Azure
- **Key Services**:
  - Azure Functions (Python runtime)
  - Azure OpenAI (GPT models)
  - CosmosDB (NoSQL database)
  - Azure Logic Apps
  - Microsoft Teams
- **Key Libraries**:
  - `azure-functions`: Azure Functions SDK
  - `openai`: Azure OpenAI client library

## Coding Standards and Style Guidelines

### Python Code Style
- Follow PEP 8 Python style guidelines
- Use meaningful variable and function names that describe their purpose
- Keep functions focused and single-purpose
- Use type hints where appropriate to improve code clarity

### Function Structure
- Azure Functions should use the decorator pattern (`@app.function_name`, `@app.route`, etc.)
- Always include proper logging with `logging.info()` or appropriate log levels
- Handle errors gracefully with appropriate HTTP response codes

### Environment Variables
- Use `os.getenv()` or `os.environ.get()` for configuration values
- Provide sensible defaults for non-critical settings
- Document all required environment variables

### Code Organization
- Keep business logic separate from Azure Function entry points
- Use descriptive names for function parameters and variables
- Add comments only when necessary to explain complex logic or business rules

### Security Practices
- Always sanitize user input (e.g., remove HTML tags from user questions)
- Never commit API keys, connection strings, or secrets to source control
- Use environment variables for all sensitive configuration

## Testing Requirements

- Test Azure Functions locally using Azure Functions Core Tools before deployment
- Validate CosmosDB connections and data retrieval
- Test OpenAI API integration with sample prompts
- Verify end-to-end flow through Teams when possible

### Manual Testing
Since this project doesn't have automated tests currently:
- Test the Azure Function locally using the Azure Functions runtime
- Validate responses from the RAG model with various questions
- Check error handling with invalid inputs
- Verify environment variable configuration

## Documentation Expectations

### Code Documentation
- Add docstrings to functions that explain their purpose, parameters, and return values
- Document complex algorithms or business logic
- Keep inline comments minimal but use them for non-obvious code

### README Updates
- Update the README.md if you add new features or change deployment steps
- Document any new environment variables or configuration requirements
- Include examples for new functionality

### Deployment Documentation
- Document any changes to the deployment process
- Update configuration scripts in the `bin/` directory if needed
- Note any new Azure resource requirements

## Architecture Patterns

### RAG Implementation Pattern
This project implements a simplified RAG (Retrieval-Augmented Generation) pattern:
1. **Data Retrieval**: Facts are retrieved from CosmosDB using Azure Functions bindings
2. **Prompt Enhancement**: Retrieved facts are combined with the user's question
3. **LLM Generation**: Enhanced prompt is sent to Azure OpenAI for response generation
4. **Response Delivery**: Generated response is returned via Logic App to Teams

### Key Patterns Used
- **Azure Functions Bindings**: Use decorators for HTTP triggers and CosmosDB input bindings
- **Environment-Based Configuration**: All service endpoints and settings come from environment variables
- **Message Enrichment**: User queries are enriched with relevant context before LLM processing

### CosmosDB Data Access
- Use Azure Functions input bindings for CosmosDB (`@app.cosmos_db_input`)
- Access documents via the `DocumentList` parameter
- Extract facts using `doc.data['fact']` pattern

### OpenAI Integration
- Use the `AzureOpenAI` client from the `openai` library
- Configure with environment variables: `AOAI_ENDPOINT`, `AOAI_KEY`, `MODEL`
- Structure messages with system and user roles for context and query

## Clarifying Questions Policy

**Always ask clarifying questions before implementing significant changes or new features.** Specifically, ask about:

- **Scope**: What is the exact scope of the change? Should it apply to all similar cases?
- **Requirements**: Are there specific business rules or constraints to consider?
- **Dependencies**: Will this change affect other components or integrations?
- **Configuration**: Are new environment variables or Azure resources needed?
- **Testing**: How should the change be tested? Are there specific test cases to cover?
- **Deployment**: Will this require changes to deployment scripts or Azure configuration?

## File Structure

```
.
├── README.md                    # Main project documentation
├── bin/                         # Deployment and setup scripts
│   ├── setup.sh                 # Environment setup script
│   ├── createDB.sh             # CosmosDB container creation
│   ├── insertCosmos.py         # Data import script
│   ├── updateFNConfig.sh       # Azure Function configuration
│   └── deployFunc.sh           # Function deployment script
├── data/                        # Sample data and prompts
│   ├── cosmosdb-facts.txt      # Sample facts for CosmosDB
│   └── datagen_prompt.txt      # Prompt for generating test data
└── src/
    └── azureFunction/           # Azure Function source code
        ├── function_app.py      # Main function implementation
        ├── host.json           # Function host configuration
        └── requirements.txt     # Python dependencies
```

## Common Development Tasks

### Adding New Dependencies
1. Add the package to `src/azureFunction/requirements.txt`
2. Test locally with the new dependency
3. Redeploy the Azure Function

### Modifying the RAG Prompt
- Edit the prompt construction in `function_app.py`
- Adjust the system message or user message format
- Test with various questions to ensure quality responses

### Updating CosmosDB Facts
- Modify `data/cosmosdb-facts.txt`
- Run `bin/insertCosmos.py` to reload data
- Verify using Azure Portal Data Explorer

### Changing OpenAI Parameters
- Modify environment variables: `TEMPERATURE`, `MAX_TOKENS`, `TOP_P`, etc.
- Use `bin/updateFNConfig.sh` to update Azure Function settings
- Restart the Azure Function if needed

## Best Practices

1. **Keep Changes Minimal**: Make surgical, focused changes rather than broad refactorings
2. **Test Locally First**: Use Azure Functions Core Tools to test changes before deployment
3. **Environment Parity**: Ensure local and Azure environments are configured similarly
4. **Error Handling**: Always return appropriate HTTP status codes and error messages
5. **Logging**: Add logging statements to help with debugging and monitoring
6. **Security First**: Never expose secrets; always sanitize user input
7. **Documentation**: Update documentation when making significant changes

## Additional Resources

- [Azure Functions Python Developer Guide](https://learn.microsoft.com/en-us/azure/azure-functions/functions-reference-python)
- [Azure OpenAI Service Documentation](https://learn.microsoft.com/en-us/azure/ai-services/openai/)
- [CosmosDB Python SDK](https://learn.microsoft.com/en-us/azure/cosmos-db/nosql/quickstart-python)
- [RAG Overview](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)
