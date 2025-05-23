**first MCP server for a real-life use case**—an **HR leave management assistant**.


### **Overview:
* **Objective**: Build an **MCP server** that powers an **HR assistant chatbot** to manage employee leave (check balance, apply, view history).


### **Architecture**

1. **Database**: leave database 
2. **MCP Server**: Python-based server using the `mcp` SDK.
3. **Client**: Uses **Claude Desktop** (a generic MCP client) instead of a custom chatbot UI for simplicity.



###  **Setup Instructions**

#### Install Tools:

* `pip install mcp` – MCP SDK.
* `pip install uv` – UV is a Python project manager (like Poetry).
* **Claude Desktop** – Download from official site based on OS.

#### Initialize MCP Project:

```bash
uv init my-first-mcp-server
cd my-first-mcp-server
```

#### Project Structure:

* `main.py`: Your core MCP server code.
* `pyproject.toml`: Metadata/config for UV.

---

### **Mock Database Example**

```python
mock_db = {
    "E001": {"balance": 18, "leaves_taken": ["2024-12-25", "2025-01-01"]},
    "E002": {"balance": 20, "leaves_taken": []}
}
```

---

### **MCP Server Code Design**

* Import `FastMCP` class.
* Create tools:

  * `get_leave_balance(employee_id)`
  * `apply_leave(employee_id, leave_dates)`
  * `get_leave_history(employee_id)`
* Add a **resource** tool for greetings.

> **Key Tip**: Write **detailed docstrings** — they're essential for LLM-based tool selection.

---

### **Running the Server**

```bash
uv mcp install main.py
```

* Registers the server (e.g., `leave-manager`) in Claude's config.

---

### **Claude Desktop Integration**

* Open Claude Desktop.
* Enable **Developer Mode** (important for seeing custom tools).
* Go to **File → Settings → Developer**.
* Edit config to verify the MCP server (`leave-manager`) is correctly registered.

---

###  **Using the Assistant (Claude Desktop)**

* Ask: *“How many leaves does E001 have?”*

  * Calls: `get_leave_balance("E001")`
* Ask: *“Show me leave dates for the same person.”*

  * Maintains context (`E001`) → calls: `get_leave_history("E001")`
* Ask: *“Apply leave for E002 on 4th July.”*

  * Calls: `apply_leave("E002", ["2025-07-04"])`

Claude interprets natural language, maps to the right tool, and fills parameters intelligently.

---

### **Troubleshooting Tip**

If errors like `type str` occur:

```bash
pip install --upgrade typer
```

---