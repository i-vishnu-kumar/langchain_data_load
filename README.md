# Document Loaders and Text Splitters with LangChain

This repository demonstrates how to load and process documents from various sources using the LangChain library. It also includes examples of splitting large texts into manageable chunks for downstream NLP tasks like summarization, QA, and embeddings.

📦 Requirements
Install the required dependencies:
```bash
pip install langchain langchain-community beautifulsoup4
```

📚 Loaders Used
This project covers different data sources and how to load them:

1. Text File
```python
from langchain_community.document_loaders import TextLoader

loader = TextLoader('speech.txt')
text_document = loader.load()
```
2. PDF File
```python
from langchain_community.document_loaders import PyPDFLoader

loader = PyPDFLoader('Attention.pdf')
docs = loader.load()
```
3. Web Page
```python
from langchain_community.document_loaders import WebBaseLoader
import bs4

loader = WebBaseLoader(
    web_paths=("https://lilianweng.github.io/posts/2023-06-23-agent/",),
    bs_kwargs=dict(parse_only=bs4.SoupStrainer(class_=("post-title", "post-content", "post-hander")))
)
web_data = loader.load()
```
4. Wikipedia
```python
from langchain_community.document_loaders import WikipediaLoader

docs = WikipediaLoader(query="HUNTER X HUNTER", load_max_docs=2).load()
```
✂️ Text Splitting
To handle large documents, text is split into smaller chunks using the RecursiveCharacterTextSplitter or CharacterTextSplitter.

Recursive Character Splitter (for PDFs or large content)
```python
from langchain_text_splitters import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter(chunk_size=500, chunk_overlap=50)
final_documents = text_splitter.split_documents(docs)
```
Character Text Splitter (for structured text)
```python
from langchain_text_splitters import CharacterTextSplitter

speech = ""
with open("speech.txt") as f:
    speech = f.read()

text_splitter = CharacterTextSplitter(separator="\n\n", chunk_size=100, chunk_overlap=20)
text = text_splitter.create_documents([speech])
```
🔍 Output Examples
After loading and splitting the documents, you can view the chunks:

```python
print(final_documents[0])
print("-" * 50)
print(final_documents[1])
```

