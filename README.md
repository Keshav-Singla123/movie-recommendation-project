# 🎬 GraphRAG Movie Recommendation System

**A Hybrid AI System: Knowledge Graph (Neo4j) + Vector Search (Pinecone) + Google Gemini**

This project is a sophisticated Movie Recommendation Engine that uses **GraphRAG** (Retrieval-Augmented Generation). It eliminates AI hallucinations by cross-referencing a **Knowledge Graph** (facts) with **Vector Search** (similarity) to provide accurate, context-aware answers.

![Node.js](https://img.shields.io/badge/Node.js-v20+-green)
![Neo4j](https://img.shields.io/badge/Neo4j-Graph_DB-blue)
![Pinecone](https://img.shields.io/badge/Pinecone-Vector_DB-orange)
![Gemini](https://img.shields.io/badge/AI-Google_Gemini-8E75B2)

---

## 🚀 What It Does

1.  **Ingests Data:** Reads a raw PDF file containing movie details.
2.  **Builds a "Brain":**
    *   **Structured Knowledge:** Extracts entities (Actors, Directors, Awards) and links them in a **Neo4j Graph Database**.
    *   **Vector Memory:** Converts movie summaries into mathematical vectors (embeddings) stored in **Pinecone**.
3.  **Smart Querying:**
    *   **Graph Questions:** *"Who directed Inception?"* (Traverses the graph for exact facts).
    *   **Similarity Questions:** *"I liked Interstellar, what else is similar?"* (Uses vector search for recommendations).
    *   **Reasoning:** Uses **Google Gemini** to generate natural language answers based on the retrieved data.

---

## 🔑 Configuration: Getting Your Keys

You need to create a `.env` file in the root folder and fill in these keys. Here is exactly where to get them:

### 1. Google Gemini API
*   **Variable:** `GEMINI_API_KEY`
*   **Where to get:** [Google AI Studio](https://aistudio.google.com/)
*   **Action:** Click "Get API key" -> "Create API key".

### 2. Neo4j Graph Database
*   **Variables:** `NEO4J_URI`, `NEO4J_USERNAME`, `NEO4J_PASSWORD`
*   **Where to get:** [Neo4j Aura (Free Tier)](https://neo4j.com/cloud/platform/aura-graph-database/)
*   **Action:** Create a Free Instance. Download the `.txt` file provided; it contains your URI and Password.
    

### 3. Pinecone Vector Database
*   **Variables:** `PINECONE_API_KEY`, `PINECONE_INDEX_NAME`
*   **Where to get:** [Pinecone.io](https://www.pinecone.io/)
*   **Action:** Create a "Serverless" Index.
    *   **Name:** `movies` (or your choice).
    *   **Dimensions:** **3072** (⚠️ Crucial!).
    *   **Metric:** `cosine`.

---

## 🛠️ Installation

1.  **Clone the repository**
    ```bash
    git clone https://github.com/Keshav-Singla123/movie-recommendation-project.git
    cd movie-recommendation-project
    ```

2.  **Install Dependencies**
    ```bash
    npm install
    ```

3.  **Set up Environment**
    Create a file named `.env` and paste your keys:
    ```env
    GEMINI_API_KEY=your_key
    NEO4J_URI=neo4j+s://your_uri
    NEO4J_USERNAME=neo4j
    NEO4J_PASSWORD=your_password
    PINECONE_API_KEY=your_key
    PINECONE_INDEX_NAME=movies
    ```

---

## 🏃‍♂️ How to Use / Run

### Step 1: Prepare Data (⚠️ Important)
You must create a valid PDF file. **Do not create this file inside VS Code.**
1.  Open **Notepad** or **Word**.
2.  Paste your movie data.
3.  **Print to PDF**.
4.  Save as `movies.pdf` inside the `data/` folder.

### Step 2: Test Connections
Run this first to ensure all APIs are connected.
```bash
npm run test
```

### Step 3: Index Data
Reads the PDF and uploads knowledge to the databases. Run this once.
```bash
npm run index
```

### Step 4: Start Chatting
Launch the interactive AI assistant.
```bash
npm run query
```

## 📂 Project Structure
```text
├── data/
│   └── movies.pdf           # Source Data (Must be a valid binary PDF)
├── 1_testConnection.js      # System Health Check
├── 2_config.js              # Central Config (Env vars & Clients)
├── 3_pdfParser.js           # PDF Reading Logic
├── 4_entityExtractor.js     # AI Entity Extraction (PDF -> JSON)
├── 5_graphBuilder.js        # Graph Injection Logic (JSON -> Neo4j)
├── 6_vectorStore.js         # Vector Logic (Text -> Embeddings -> Pinecone)
├── 7_runIndexing.js         # MAIN PIPELINE script
├── 8_cypherTemplates.js     # Neo4j Query Templates
├── 9_entityResolver.js      # Maps user keywords to DB Entities
├── 10_queryClassifier.js    # Router (Graph Query vs. Vector Query)
├── 11_graphHandler.js       # Handles Fact-based questions
├── 12_similarityHandler.js  # Handles Recommendation questions
└── 13_runQuery.js           # CLI Chat Interface
```

## 🔧 Troubleshooting

| Error | Solution |
| --- | --- |
| "The document has no pages" | Your PDF is corrupt (likely created in VS Code). Use "Microsoft Print to PDF" in Notepad instead. |
| "Illegal host" (Neo4j) | Your NEO4J_URI is wrong. It must start with neo4j+s://. |
| "Dimension mismatch" | Your Pinecone index is set to 1536 or 768. Delete it and create a new one with dimension 3072. |
| 400 / Model not found | Ensure you are using gemini-1.5-flash in 2_config.js. |
| Rate Limit (429) | You are on the Free Tier. Edit 7_runIndexing.js to limit movies: extractAllEntities(pdfPath, 10). |

## 👨‍💻 Author
Keshav Singla  
GitHub: @Keshav-Singla123

## 🙌 Acknowledgments
- Google Gemini for the LLM and Embeddings.
- Neo4j for Graph Database technology.
- Pinecone for Vector Search.
- LangChain for orchestrating the AI workflow.

