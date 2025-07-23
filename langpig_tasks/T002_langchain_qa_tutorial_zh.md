# T002｜LangChain QA 问答系统新手教程（中文版） 📙

🧠 执行者：LangPig v0.1  
📅 日期：2025.07.23

---

## 🎯 任务目标

- 阅读 cookbook 目录下的 QA 示例（如 `cookbook/qa_with_sources.ipynb` 等）
- 提炼出最简单可复现的问答流程
- 用中文写成一份“零基础新手也能看懂”的教程文档，要求逻辑清晰、代码简单、每步有解释

---

## 🚦 新手任务指引

### 1. 安装依赖

```bash
pip install langchain openai faiss-cpu

from langchain_openai import ChatOpenAI
from langchain.chains import RetrievalQA
from langchain.vectorstores import FAISS
from langchain.embeddings import OpenAIEmbeddings
from langchain.document_loaders import TextLoader

# 1. 加载你的知识库文档
loader = TextLoader('./data/your_docs.txt', encoding='utf-8')
docs = loader.load()

# 2. 生成向量数据库
db = FAISS.from_documents(docs, OpenAIEmbeddings())

# 3. 初始化语言模型
llm = ChatOpenAI(model_name='gpt-3.5-turbo', temperature=0.2)

# 4. 构建QA链
qa = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type="stuff",
    retriever=db.as_retriever()
)

# 5. 实时提问
question = "LangChain可以做什么？"
answer = qa.run(question)
print(answer)

# 本教程完全可以在本地Python3环境直接运行（推荐配合Jupyter或VS Code）
# 如果遇到任何报错，99%都是环境没装好/路径写错/API Key没配
# 不要怕，问小猪，一步一步来就会通！

## 常见报错与排查 Tips

- `ModuleNotFoundError: No module named 'langchain'`
  → 说明你还没装好依赖，运行 `pip install langchain openai faiss-cpu`。

- `FileNotFoundError: [Errno 2] No such file or directory: './data/your_docs.txt'`
  → 检查 `data` 目录和 `your_docs.txt` 是否真的存在（目录别打错）。

- `openai.error.AuthenticationError: No API key provided.`
  → 你还没配置 OpenAI API Key，见前面代码块的 Key 设置方式。

- `TypeError` 或 `AttributeError`
  → 可能你的 langchain 版本太新/太旧，或某个 API 有变动。建议先装官方推荐版本。

- 其它任何报错
  → 复制报错内容给“小猪”，共生宇宙技术支持随时在线！

---

> 用最放心的心态，多试多问多动手，你已经比99%的人都勇敢！
