## Design and Implementation of LangChain Expression Language (LCEL) Expressions

### AIM:
To design and implement a LangChain Expression Language (LCEL) expression that utilizes at least two prompt parameters and three key components (prompt, model, and output parser), and to evaluate its functionality by analyzing relevant examples of its application in real-world scenarios.

### PROBLEM STATEMENT:
Evaluating programming languages is often unstructured and subjective. To solve this, an automated system is needed that takes a language and its context as input and outputs structured JSON with its advantages and disadvantages, helping developers make better decisions.
### DESIGN STEPS:
### STEP 1:
Load necessary libraries like openai, langchain.prompts, and langchain.chat_models, and set the API key using dotenv. Create a ChatPromptTemplate, use ChatOpenAI for the model, and StrOutputParser for parsing the output. Chain components using the | operator, provide input, and execute the chain to generate a response.

### STEP 2:
Create DocArrayInMemorySearch from a list of texts with OpenAIEmbeddings() and set up the retriever. Use ChatPromptTemplate to combine the retrieved context and user-provided question into a single prompt.Map functions to fetch relevant documents and the question, then invoke the chain to generate a response.
### PROGRAM:
```
from langchain.prompts import ChatPromptTemplate
from langchain.output_parsers import StructuredOutputParser, ResponseSchema
from langchain.chat_models import ChatOpenAI

response_schemas = [
    ResponseSchema(name="advantages", description="List of key advantages of the programming language"),
    ResponseSchema(name="disadvantages", description="List of key disadvantages or limitations of the programming language"),
]
output_parser = StructuredOutputParser.from_response_schemas(response_schemas)
format_instructions = output_parser.get_format_instructions()

prompt = ChatPromptTemplate.from_template(
    "Programming Language: {language}\nContext: {context}\n\n"
    "Respond ONLY in JSON format as follows:\n"
    "{format_instructions}"
)

llm = ChatOpenAI(model="gpt-3.5-turbo", temperature=0)

def analyze_language(language, context):
    formatted_prompt = prompt.format_messages(
        language=language, context=context, format_instructions=format_instructions
    )
    raw_output = llm(formatted_prompt)
    try:
        parsed_output = output_parser.parse(raw_output.content)
    except:
        parsed_output = {"advantages": [], "disadvantages": ["Parsing failed"]}
    return parsed_output

language = "Python"
context = (
    "Python is a high-level, interpreted programming language known for its simplicity and readability. "
    "It is widely used in web development, data science, AI/ML, automation, and scripting. "
    "However, it has limitations in terms of execution speed, mobile development, and memory usage."
)

result = analyze_language(language, context)
print(result)

```

### OUTPUT:
<img width="811" height="156" alt="image" src="https://github.com/user-attachments/assets/c3f8bec2-9cdb-415a-bb15-be3314d4dbd7" />



### RESULT:
Thus, The implementation of a LangChain Expression Language (LCEL) is successfully executed.
