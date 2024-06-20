# Tokenizer

The **Tokenizer** submodule in Anuvaad is designed to break down paragraphs into sentences or words, facilitating efficient preprocessing and accurate translations. This submodule is integral for preparing text data for subsequent processing and translation tasks.

### Key Features

- **Paragraph to Sentence Tokenization**: Splits paragraphs into individual sentences, making the text easier to process.
- **Sentence to Word Tokenization**: Breaks down sentences into individual words for detailed analysis and translation.
- **Document-Specific Handling**: Manages document-specific symbols and special characters to ensure consistency and accuracy in tokenization.
- **Flexible Integration**: Can be invoked independently as a standalone service or as part of a larger workflow through the Workflow Manager.

### Usage

The Tokenizer can be utilized in two main ways:

1. **Independent Invocation**:
   - As an independent service, the Tokenizer can be directly called to process text data. This is useful for isolated tasks where only tokenization is required.

2. **Workflow Manager Integration**:
   - Within the Workflow Manager, the Tokenizer works as a part of the broader document processing and translation pipeline. This integration allows for seamless interaction with other Anuvaad submodules, ensuring smooth and efficient data flow.


### Code Repository

You can find the code for the Tokenizer submodule in the Anuvaad repository at the following link:
[GitHub Repository](https://github.com/project-anuvaad/anuvaad/tree/master/anuvaad-etl/anuvaad-extractor/sentence)

### API Contract

For detailed information about the API endpoints and their usage, refer to the API contract available at:
[API Contract](https://petstore.swagger.io/?url=https://raw.githubusercontent.com/project-anuvaad/anuvaad/master/anuvaad-etl/anuvaad-extractor/sentence/docs/sentence-api-contarct.yml)

By employing the Tokenizer submodule, Anuvaad ensures that text data is meticulously prepared, contributing to the overall accuracy and efficiency of the document processing and translation workflow.

---