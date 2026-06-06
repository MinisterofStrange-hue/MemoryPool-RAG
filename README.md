# MemoryPool-RAG
MemoryPool‑RAG is a low‑budget prototype that demonstrates how a software‑defined pooled memory fabric combined with intelligent KV caching can reduce peak DRAM requirements for LLM inference and RAG pipelines. The repo contains a simulator, cache experiments, a small RAG demo, a cost model, and a Streamlit dashboard for visualization.
MemoryPool-RAG/
├─ README.md
├─ requirements.txt
├─ notebooks/
│  ├─ 01_simulator.ipynb
│  ├─ 02_cache_experiments.ipynb
│  └─ 03_rag_demo.ipynb
├─ src/
│  ├─ simulator/
│  │  ├─ __init__.py
│  │  ├─ simple_sim.py
│  │  └─ run_sim.py
│  ├─ cache/
│  │  ├─ __init__.py
│  │  ├─ kv_cache.py
│  │  └─ run_cache_sim.py
│  ├─ rag/
│  │  ├─ __init__.py
│  │  └─ rag_demo.py
│  └─ api/
│     └─ mf_cp_api.py
├─ frontend/
│  └─ streamlit_app/
│     └─ app.py
├─ docs/
│  ├─ architecture.png
│  └─ demo.mp4
└─ scripts/
   └─ generate_cost_report.py
git clone https://github.com/your-username/MemoryPool-RAG.git
cd MemoryPool-RAG
python -m venv .venv
# macOS / Linux
source .venv/bin/activate
# Windows PowerShell
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
python -m src.simulator.run_sim --nodes 8 --pool_nodes 2 --workload_trace notebooks/sample_trace.json
python -m src.cache.run_cache_sim --policy LRU --cache_gb 4 --trace notebooks/sample_queries.json
streamlit run frontend/streamlit_app/app.py
from simulator.simple_sim import Server, PoolNode, MemoryFabricSimulator

servers = [Server(id=i, dram_gb=16.0) for i in range(8)]
pools = [PoolNode(id=0, capacity_gb=256.0, latency_ms=0.5)]
sim = MemoryFabricSimulator(servers, pools)

alloc = sim.allocate(server_id=0, size_gb=2.0)
print(alloc)  # {"where":"local","latency_ms":0.1} or {"where":"pool:0","latency_ms":0.5}
from cache.kv_cache import KVCache
cache = KVCache(max_items=10000)
cache.set("embedding_123", embedding_vector)
vec = cache.get("embedding_123")
fastapi==0.95.2
uvicorn==0.22.0
streamlit==1.26.0
numpy==1.26.2
pandas==2.2.2
matplotlib==3.8.1
scikit-learn==1.3.2
sentence-transformers==2.2.2
faiss-cpu==1.7.4
transformers==4.35.0
accelerate==0.22.0
peft==0.5.0
pytest==7.4.0
black==24.3.0
git checkout -b feature/kv-cache
# implement code
git add .
git commit -m "Add LFU cache policy"
git push origin feature/kv-cache
# open a pull request on GitHub
.venv/
__pycache__/
*.pyc
.env
secrets.json
