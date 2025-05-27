# Financial Researcher

A modular, AI-driven research and reporting framework built on [Crew.ai](https://github.com/crewai/crewai). Define agents and tasks via simple YAML configs, then spin up a “crew” of LLM-powered specialists to research, analyze, and generate professional reports on any company or topic.

---

##  Features

- **Seamless orchestration** of multiple AI agents (researcher, analyst)  
- **Config-driven**: define agents (role, goal, LLM) and tasks (description, inputs, outputs) in YAML  
- **Sequential process**: run research → analyze → output polished Markdown report  
- **Extensible**: plug in new tools, agents, or tasks via `crewai` decorators  
- **Clear separation** of domain logic (`crew.py`), orchestration (`main.py`), and configuration (`agents.yaml`, `tasks.yaml`)  

---

##  Prerequisites

- Python 3.9+  
- Access to OpenAI API (or other LLMs supported by your `crewai` installation)  
- Environment variable `OPENAI_API_KEY` (or equivalent) set  



## 🛠️ Installation

1. **Clone the repo**  
   ```bash
   git clone https://github.com/your-username/financial_researcher.git
   cd financial_researcher
````

2. **Create & activate a virtual environment**

   ```bash
   python -m venv .venv
   source .venv/bin/activate   # Linux/macOS
   .\.venv\Scripts\activate    # Windows
   ```

3. **Install dependencies**

   ```bash
   pip install crewai crewai_tools pyyaml
   ```

---

## 🔧 Configuration

All agent and task definitions live in YAML under `src/financial_researcher`:

* **agents.yaml**

  ```yaml
  researcher:
    role: "Senior Financial Researcher for {company}"
    goal: "Research the company, news and potential for {company}"
    backstory: "You're a seasoned financial researcher..."
    llm: "openai/gpt-4o-mini"

  analyst:
    role: "Market Analyst and Report writer focused on {company}"
    goal: "Analyze company {company} and create a comprehensive report..."
    backstory: "You're a meticulous, skilled analyst..."
    llm: "groq/llama-3.1-70b-versatile"
  ```

* **tasks.yaml**

  ```yaml
  research_task:
    description: |
      Conduct thorough research on {company}, covering its history,
      financial performance, market positioning, and recent news.
      Summarize your findings in a structured format with clear sections.
    expected_output: |
      A comprehensive research document with well-organized sections,
      including specific facts, figures, and examples where relevant.
    agent: researcher

  analysis_task:
    description: |
      Analyze the research findings for {company} and craft a polished,
      professional report in Markdown. Present it in a clear, easy-to-read
      style with headings.
    expected_output: |
      A polished, professional report on {company}, complete with an
      executive summary, main sections, and conclusion.
    agent: analyst
    context:
      - research_task
    output_file: output/report.md
  ```

You can customize the `{company}` placeholder or add new agents/tasks as needed.

---

## 🎯 Usage

1. **Create your output directory** (the script will do this automatically):

   ```bash
   mkdir -p output
   ```

2. **Run the crew**:

   ```bash
   python src/financial_researcher/main.py
   ```

   By default, `main.py` kicks off the crew with:

   ```python
   inputs = { 'company': 'Tesla' }
   ```

   You can change the target company by editing `main.py` or extending it to accept a CLI argument.

3. **View the report**
   Once complete, you’ll see:

   ```
   === FINAL REPORT ===

   ... (Markdown content) ...

   Report has been saved to output/report.md
   ```

   Open `output/report.md` to review the full, formatted report.

---

## 📁 Project Structure

```text
.
├── src/
│   └── financial_researcher/
│       ├── crew.py          # CrewBase class wiring up agents & tasks
│       ├── main.py          # Entry point: creates output/, kicks off crew
│       ├── agents.yaml      # Agent definitions (role, goal, backstory, LLM)
│       └── tasks.yaml       # Task definitions (description, agent mapping)
├── output/                  # Generated reports (ignored via .gitignore)
└── README.md                # ← You are here
```

---

## ✨ Extending the Crew

1. **Add a new agent**:

   * Define its `role`, `goal`, `backstory`, and `llm` in `agents.yaml`.
   * Reference it in any task’s `agent` field.

2. **Add a new task**:

   * Specify `description`, `expected_output`, `agent`, and optional fields like `context` or `output_file` in `tasks.yaml`.
   * Decorate a method in `crew.py` with `@task` to override defaults or add custom logic.

3. **Use custom tools**:

   * Import and attach tools (e.g., `SerperDevTool()`, `YourCustomTool()`) in your agent factory methods in `crew.py`.

---

## 🤝 Contributing

1. Fork the repo
2. Create a feature branch (`git checkout -b feature/foo`)
3. Commit your changes (`git commit -am "Add foo"`)
4. Push to the branch (`git push origin feature/foo`)
5. Open a Pull Request

Please adhere to the existing code style and update tests where applicable.

---

## 📄 License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

```

---

**Next steps:**
- Add a `requirements.txt` listing `crewai`, `crewai_tools`, and `pyyaml`.
- Optionally configure CLI parsing in `main.py` to pass the target company at runtime.
- Enhance CI/CD to validate YAML schemas and lint generated reports.
```
