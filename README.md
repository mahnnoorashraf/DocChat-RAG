# DocChat-RAG

A Retrieval-Augmented Generation (RAG) application using LangChain and OpenAI for querying AWS EC2 documentation.

## Features

- **Document Processing**: Automatically processes AWS EC2 documentation from markdown files
- **Vector Search**: Uses ChromaDB for efficient similarity search
- **AI-Powered Responses**: Leverages OpenAI's GPT models for intelligent responses
- **Source Attribution**: Provides source documents for each response

## Project Structure

```
DocChat-RAG/
├── RAG-Application-with-LangChain/
│   ├── create_database.py      # Creates vector embeddings from documents
│   ├── query_data.py           # Queries the database and generates responses
│   ├── requirements.txt        # Python dependencies
│   ├── data/                   # AWS EC2 documentation (markdown files)
│   └── chroma/                 # Vector database storage (auto-generated)
├── .gitignore                  # Git ignore rules
└── README.md                   # This file
```

## Prerequisites

- Python 3.8+
- OpenAI API key
- Git

## Installation

1. **Clone the repository**:
   ```bash
   git clone https://github.com/mahnnoorashraf/DocChat-RAG.git
   cd DocChat-RAG/RAG-Application-with-LangChain
   ```

2. **Create a virtual environment**:
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   pip install langchain-community
   pip install "unstructured[md]"
   ```

4. **Set up your OpenAI API key**:
   ```bash
   # Create .env file
   echo 'OPENAI_API_KEY="your_actual_api_key_here"' > .env
   ```

## Usage

### 1. Create the Vector Database

First, process the documents and create embeddings:

```bash
python create_database.py
```

This will:
- Load all markdown files from the `data/aws_ec2_documentation/` directory
- Split them into chunks of 1000 characters with 500 character overlap
- Create vector embeddings using OpenAI's embedding model
- Store them in a ChromaDB database

### 2. Query the Database

Ask questions about the AWS EC2 documentation:

```bash
python query_data.py "What operating systems does EC2 support?"
```

Example queries:
- "How do I create an EC2 instance?"
- "What are the different instance types available?"
- "How do I configure security groups?"
- "What is the pricing model for EC2?"

## Configuration

### Document Path
To use your own documents, modify the `DATA_PATH` variable in `create_database.py`:

```python
DATA_PATH = "path/to/your/documents"
```

### Chunk Size
Adjust text splitting parameters in `create_database.py`:

```python
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,      # Size of each chunk
    chunk_overlap=500,    # Overlap between chunks
    length_function=len,
    add_start_index=True
)
```

### Search Parameters
Modify search behavior in `query_data.py`:

```python
results = db.similarity_search_with_relevance_scores(query_text, k=4)  # Number of results
if len(results) == 0 or results[0][1] < 0.7:  # Relevance threshold
```

## Troubleshooting

### Common Issues

1. **API Key Error**: Make sure your OpenAI API key is correctly set in the `.env` file
2. **Missing Dependencies**: Install all required packages using the installation steps above
3. **Large File Warnings**: The application handles large documents by splitting them into chunks

### Performance Tips

- The first run may take time to process all documents
- Subsequent queries are fast due to the vector database
- Consider adjusting chunk size based on your document types

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Submit a pull request

## License

This project is open source and available under the [MIT License](LICENSE).

## Acknowledgments

- Built with [LangChain](https://langchain.com/)
- Uses [ChromaDB](https://www.trychroma.com/) for vector storage
- Powered by [OpenAI](https://openai.com/) models
- AWS EC2 documentation for sample data
