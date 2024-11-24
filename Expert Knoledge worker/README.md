# MRID Expert Knowledge Worker

## Overview
The MRID Expert Knowledge Worker is a question-answering agent designed to serve as an expert knowledge assistant for employees of **MRID COMPANY**, an Insurance Tech company. By leveraging **Retrieval Augmented Generation (RAG)**, the system ensures high accuracy while maintaining low costs, addressing the company's need for precise and reliable responses without introducing unnecessary expenses.

## Key Features

- **High Accuracy with RAG**: The agent employs Retrieval Augmented Generation, a cutting-edge technique that retrieves relevant context before generating responses, ensuring answers are both factual and relevant.
- **Low Cost**: Utilizes the "gpt-4o-mini" model, balancing performance and cost to meet budget constraints.
- **Context-Aware**: Capable of parsing through employee and product knowledge bases to provide specific and accurate answers.
- **Prevention of Hallucination**: Configured with a system prompt to avoid generating information outside of the provided context.

## Use Cases

- Assisting employees in retrieving information about internal policies, products, and employee records.
- Answering questions about company operations, product features, and career progression.
- Supporting HR and management in responding to employee inquiries efficiently.

---

## Technical Details

### System Components

1. **Model**:
   - `MODEL = "gpt-4o-mini"`
   - Selected for its balance of cost-efficiency and capability.

2. **Environment Setup**:
   - Environment variables are managed using a `.env` file for secure storage of API keys.
   ```python
   load_dotenv()
   os.environ['OPENAI_API_KEY'] = os.getenv('OPENAI_API_KEY')
   openai = OpenAI()
   ```

3. **Knowledge Bases**:
   - **Employee Knowledge Base**: Stores employee-related documents categorized by names.
   - **Product Knowledge Base**: Stores product-related documentation, such as feature details, pricing, and roadmaps.

4. **System Prompt**:
   - A dedicated system message guides the model to:
     - Provide accurate, concise answers.
     - Refrain from generating information outside the provided context.
   ```
   system_message = "You are an expert in answering accurate questions about Insurellm, the Insurance Tech company. Give brief, accurate answers. If you don't know the answer, say so. Do not make anything up if you haven't been provided with relevant context."
   ```

### Context Retrieval Functionality

- **Context Loading**:
  Employee and product data are dynamically loaded from their respective directories.
  ```python
  employees = glob.glob("knowledge-base/employees/*")
  products = glob.glob("knowledge-base/products/*")
  ```

- **Relevance Matching**:
  The system identifies relevant documents based on user queries.
  ```python
  def get_relevant_context(message):
      relevant_context = []
      for context_title, context_details in context.items():
          if context_title.lower() in message.lower():
              relevant_context.append(context_details)
      return relevant_context
  ```

- **Augmented Responses**:
  Provides additional relevant context to enhance the accuracy of responses.
  ```python
  def add_context(message):
      relevant_context = get_relevant_context(message)
      if relevant_context:
          message += "\n\nThe following additional context might be relevant in answering this question:\n\n"
          for relevant in relevant_context:
              message += relevant + "\n\n"
      return message
  ```

### Example Query

**Input:**
```plaintext
Who is Alex Lancaster?
```

**Output:**
```plaintext
Who is Alex Lancaster?

The following additional context might be relevant in answering this question:

# Avery Lancaster

## Summary
- **Date of Birth**: March 15, 1985  
- **Job Title**: Co-Founder & Chief Executive Officer (CEO)  
- **Location**: San Francisco, California  
... (Additional details)
```

---

## How to Use

1. **Setup Environment**:
   - Add the OpenAI API key to a `.env` file:
   ```plaintext
   OPENAI_API_KEY=your_openai_api_key_here
   ```

2. **Run the Script**:
   - Install required dependencies:
   ```bash
   pip install python-dotenv gradio openai
   ```
   - Execute the script:
   ```bash
   python main.py
   ```

3. **Query the System**:
   - Use the interface to ask questions about employees, products, or general company details.

---

## Future Enhancements

- **Extended Knowledge Base**: Integration with additional databases for broader coverage.
- **Advanced Search**: Implement semantic search capabilities for improved context matching.
- **Real-Time Updates**: Enable real-time data ingestion to keep the knowledge base up-to-date.
- **Customizable User Roles**: Allow different levels of access for employees based on roles and departments.

---

## Contribution

If you wish to contribute to the project:
1. Fork the repository.
2. Create a feature branch.
3. Submit a pull request with detailed explanations of your changes.

---

## License
This project is licensed under the MIT License. See the LICENSE file for details.

---

## Acknowledgements

- **OpenAI**: For providing the GPT model API.
- **Gradio**: For building the user-friendly interface.
- **MRID COMPANY**: For inspiring the development of this expert knowledge worker.


