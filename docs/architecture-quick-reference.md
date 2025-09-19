# KET-RAG Architecture Quick Reference

## 🏗️ Core Components

### Indexing Pipeline (索引管道)
- **DataShaper Workflows**: Declarative data processing pipelines
- **Entity Extraction**: LLM-based named entity recognition
- **Relationship Extraction**: Semantic relationship identification
- **Community Detection**: Knowledge graph community structure analysis
- **SkeletonRAG**: High-quality knowledge graph skeleton via PageRank
- **KeywordRAG**: Lightweight keyword-to-text mapping

### Query Engine (查询引擎)
- **Local Search**: Entity-focused retrieval combining KG + text chunks
- **Global Search**: Map-reduce over community reports for dataset-wide queries
- **DRIFT Search**: Community-enhanced local search with broader context
- **Question Generation**: Follow-up question generation for deeper exploration

## 🔄 Data Flow

```
Raw Documents → Text Chunking → Entity/Relationship Extraction
                     ↓
              SkeletonRAG + KeywordRAG
                     ↓
            Knowledge Graph + Indexes
                     ↓
          Local/Global/DRIFT Search
                     ↓
              Generated Answers
```

## 🎯 Key Innovation: Dual Indexing Strategy

| Component | Purpose | Method | Output |
|-----------|---------|--------|--------|
| **SkeletonRAG** | High-quality skeleton | PageRank + LLM extraction | Refined knowledge graph |
| **KeywordRAG** | Lightweight mapping | Keyword extraction + bipartite graph | Text-keyword index |

## 📊 Storage & Caching

### Storage Options
- **File**: Local filesystem storage
- **Blob**: Azure cloud storage  
- **Memory**: In-memory storage

### Cache Levels
- **JSON**: Persistent file-based cache
- **Memory**: Fast in-memory cache
- **None**: No caching for development

## ⚙️ Configuration Hierarchy

```yaml
settings.yaml
├── encoding_model: "cl100k_base"
├── local_search:
│   ├── text_unit_prop: 0.5
│   └── community_prop: 0.1
└── global_search:
    ├── max_tokens: 12000
    └── data_max_tokens: 12000
```

## 🚀 Usage Commands

```bash
# Initialize project
python -m graphrag init --root <project>/

# Tune prompts  
python -m graphrag prompt-tune --root <project>/ --config settings.yaml

# Build index
python -m graphrag index --root <project>/

# Generate context (KET-RAG specific)
python indexing_sket/create_context.py <project>/ keyword 0.5

# Generate answers (KET-RAG specific)
python indexing_sket/llm_answer.py <project>/
```

## 🔧 Tech Stack

**Core**: Python 3.10+, DataShaper, OpenAI API, pandas
**Vector**: FAISS, LanceDB, Azure Search
**NLP**: tiktoken, NLTK
**Storage**: File/Blob/Memory backends

## 💡 Performance Benefits

- ✅ **Cost Reduction**: Significantly lower indexing costs
- ✅ **Quality Improvement**: Better retrieval and generation
- ✅ **Scalability**: Suitable for large-scale applications
- ✅ **Flexibility**: Multiple retrieval strategies
- ✅ **Efficiency**: Multi-level caching and optimized data flow