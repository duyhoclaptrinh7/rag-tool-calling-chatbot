# RAG & Tool Calling Chatbot

Dự án xây dựng Chatbot thông minh kết hợp kỹ thuật **RAG (Retrieval-Augmented Generation)** và **Tool Calling (Function Calling)** sử dụng kiến trúc **ReAct Agent**, hỗ trợ tra cứu tài liệu nội bộ và tìm kiếm dữ liệu thời gian thực trên Internet qua giao diện trực quan Gradio.

---

## 🚀 Tính năng nổi bật

* **RAG Tra cứu tài liệu nội bộ:** Truy xuất chính xác thông tin từ văn bản quy định, nội quy công ty (`noiquycongty.txt`) bằng Vector Database.
* **Web Search thời gian thực:** Tự động gọi công cụ tìm kiếm bên ngoài (Tavily Search) khi câu hỏi nằm ngoài phạm vi tài liệu nội bộ (tin tức, thời giá, sự kiện mới,...).
* **Agent Tự động ra quyết định (Tool Calling):** Tự phân tích ý định người dùng để chọn công cụ phù hợp hoặc trả lời trực tiếp.
* **Giao diện Chatbot trực quan:** Tích hợp giao diện đàm thoại thân thiện bằng Gradio với khả năng ghi nhớ lịch sử hội thoại.

---

## 🛠️ Công nghệ & Kỹ thuật sử dụng

| Thành phần | Công nghệ / Thư viện | Mô tả |
| :--- | :--- | :--- |
| **LLM Core** | `gpt-4o-mini` (OpenAI) | Mô hình ngôn ngữ xử lý lý luận và sinh câu trả lời |
| **Orchestration** | `LangChain`, `LangGraph` | Quản lý luồng tương tác và xây dựng ReAct Agent (`create_react_agent`) |
| **Text Splitter** | `RecursiveCharacterTextSplitter` | Chia nhỏ văn bản (Chunk size: 1200, Overlap: 200) |
| **Embedding** | `sentence-transformers/static-similarity-mrl-multilingual-v1` | Mô hình nhúng đa ngôn ngữ gọn nhẹ từ Hugging Face |
| **Vector Store** | `Chroma DB` | Lưu trữ và tìm kiếm vector tương đồng (Similarity Search) |
| **Custom Tool** | `RetrieveTool` (BaseTool) | Công cụ tùy biến tra cứu nội bộ theo schema Pydantic |
| **Web Search Tool** | `TavilySearch` | Công cụ tìm kiếm dữ liệu trực tuyến qua Tavily API |
| **UI** | `Gradio` | Giao diện Web tương tác nhiều lượt hội thoại |

---

## 📂 Cấu trúc thư mục

```text
rag-tool-calling-chatbot/
├── Data/
│   └── noiquycongty.txt     # Dữ liệu nội quy công ty mẫu phục vụ RAG
├── chatbot.ipynb            # Notebook triển khai toàn bộ luồng RAG, Agent & UI
└── README.md                # Tài liệu hướng dẫn dự án
```

---

## ⚙️ Hướng dẫn cài đặt & Chạy dự án

### 1. Cài đặt các thư viện phụ thuộc

```bash
pip install -U langchain langchain-community langchain-openai langchain-chroma \
               langgraph langchain-huggingface langchain-tavily gradio \
               sentence-transformers pydantic
```

### 2. Thiết lập API Key

Dự án yêu cầu 2 khóa API:
* **OpenAI API Key**: Dành cho mô hình LLM.
* **Tavily API Key**: Dành cho tính năng tìm kiếm Web (đăng ký miễn phí tại [tavily.com](https://tavily.com/)).

Bạn có thể cấu hình thông qua biến môi trường hoặc nhập trực tiếp khi notebook yêu cầu:
```bash
export OPENAI_API_KEY="sk-..."
export TAVILY_API_KEY="tvly-..."
```
*(Trên Windows PowerShell dùng `$env:OPENAI_API_KEY="sk-..."`)*

### 3. Chạy Chatbot

1. Mở file [chatbot.ipynb](chatbot.ipynb) trên VS Code hoặc Jupyter Notebook / Google Colab.
2. Chạy tuần tự các cell từ trên xuống dưới.
3. Ở cell cuối cùng, Gradio sẽ khởi chạy một server cục bộ tại đường dẫn `http://127.0.0.1:7860`.
4. Mở trình duyệt để trò chuyện và kiểm thử các tính năng:
   * **Tra cứu nội bộ:** *"Quy định về bảo mật thông tin và AI của công ty là gì?"*
   * **Tra cứu Internet:** *"Giá xăng dầu hôm nay bao nhiêu?"*
