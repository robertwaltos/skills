# Skills Integration Guide

This document explains how to integrate these skills across all major AI platforms.

---

## Quick Start: Universal Instructions

Copy this to ANY AI platform's custom instructions/system prompt:

```
You have access to expert-level skills for specialized tasks. Each skill has a SKILL.md file with workflows, best practices, and reference materials.

SKILL ROUTING:
- PDF/forms/extraction → pdf skill
- Word documents/tracked changes → docx skill  
- Presentations/slides → pptx skill
- Spreadsheets/Excel/financial models → xlsx skill
- Visual art/canvas/graphics → canvas-design skill
- Web UI/frontend → frontend-design skill
- Generative/algorithmic art → algorithmic-art skill
- Animated GIFs → slack-gif-creator skill
- Theming/color palettes → theme-factory skill
- MCP servers → mcp-builder skill
- Testing/Playwright → webapp-testing skill
- React artifacts → web-artifacts-builder skill
- Creating new skills → skill-creator skill
- Brand identity → brand-guidelines skill
- Document collaboration → doc-coauthoring skill
- Internal communications → internal-comms skill

WORKFLOW:
1. Identify which skill(s) match the task
2. Read the complete SKILL.md file
3. Follow documented workflows exactly
4. Use bundled scripts/references when available
5. Check "Related Skills" for complementary capabilities
```

---

## VS Code Integration

### GitHub Copilot
1. **Custom Instructions**: Place `.github/copilot-instructions.md` in your repo ✅ Already created
2. **Workspace Settings**: Copilot automatically reads this file
3. **Chat Context**: Reference skills with `@workspace` to include skill files

### Cursor
Create `.cursorrules` in project root:
```
You have access to expert skills in the skills/ directory. Before specialized tasks:
1. Check if a relevant skill exists (each has SKILL.md)
2. Read the complete SKILL.md file
3. Follow workflows and best practices exactly
4. Use bundled scripts and references

Skill domains: pdf, docx, pptx, xlsx (documents), canvas-design, frontend-design, algorithmic-art (design), mcp-builder, webapp-testing, web-artifacts-builder (development), brand-guidelines, internal-comms, doc-coauthoring (enterprise)
```

### Continue.dev
Add to `~/.continue/config.json`:
```json
{
  "customCommands": [
    {
      "name": "skill",
      "description": "Load a skill for the task",
      "prompt": "Read the SKILL.md file at skills/{input}/SKILL.md and follow its guidelines"
    }
  ]
}
```

### Windsurf
Add to `.windsurfrules`:
```
Expert skills available in skills/ directory. Read relevant SKILL.md before: document creation (pdf/docx/pptx/xlsx), design work (canvas-design/frontend-design), development (mcp-builder/webapp-testing).
```

### Codeium
Add to workspace settings or `.codeiumrc`:
```json
{
  "contextFiles": ["skills/*/SKILL.md"],
  "instructions": "Reference skill files before specialized tasks"
}
```

---

## Claude Integration

### Claude.ai Projects
1. Create a new Project
2. Upload the entire `skills/` folder to Project Knowledge
3. Add Universal Instructions (above) to Project Instructions

### Claude API
```python
import anthropic

client = anthropic.Anthropic()

# Load relevant skill based on task
def get_skill_content(skill_name):
    with open(f"skills/{skill_name}/SKILL.md") as f:
        return f.read()

response = client.messages.create(
    model="claude-sonnet-4-20250514",
    system=f"""You are an expert assistant with access to specialized skills.
    
Current skill loaded:
{get_skill_content("pdf")}

Follow the skill's workflows and best practices exactly.""",
    messages=[{"role": "user", "content": "..."}]
)
```

### Claude MCP Server
Create an MCP server that exposes skills as tools (see `skills/mcp-builder`):
```python
@server.tool()
async def get_skill(skill_name: str) -> str:
    """Load expert skill guidelines for: pdf, docx, pptx, xlsx, canvas-design, frontend-design, mcp-builder, webapp-testing, etc."""
    return read_file(f"skills/{skill_name}/SKILL.md")
```

---

## ChatGPT / OpenAI Integration

### Custom GPTs (ChatGPT Plus)
1. Go to **Explore GPTs** → **Create**
2. Upload all `SKILL.md` files to **Knowledge**
3. Add to **Instructions**:
```
You have expert-level skills uploaded in your knowledge base. Before performing specialized tasks:

1. Search your knowledge for relevant SKILL.md files based on the task:
   - Documents: pdf.md, docx.md, pptx.md, xlsx.md
   - Design: canvas-design.md, frontend-design.md, algorithmic-art.md
   - Development: mcp-builder.md, webapp-testing.md, web-artifacts-builder.md
   - Communications: internal-comms.md, brand-guidelines.md

2. Read the complete skill file
3. Follow the documented workflows exactly
4. Reference bundled scripts and examples

Always cite which skill you're using when performing specialized work.
```

### OpenAI Assistants API
```python
from openai import OpenAI
import os

client = OpenAI()

# Upload skill files
skill_files = []
for skill in os.listdir("skills"):
    skill_path = f"skills/{skill}/SKILL.md"
    if os.path.exists(skill_path):
        file = client.files.create(file=open(skill_path, "rb"), purpose="assistants")
        skill_files.append(file.id)

# Create assistant with skills
assistant = client.beta.assistants.create(
    name="Skills Expert",
    instructions="""You have expert-level skills in your knowledge. Before specialized tasks:
1. Search for relevant SKILL.md files
2. Follow documented workflows exactly
3. Use bundled scripts and references""",
    tools=[{"type": "file_search"}],
    tool_resources={"file_search": {"vector_stores": [{"file_ids": skill_files}]}}
)
```

### ChatGPT Custom Instructions (Free tier)
Go to **Settings** → **Personalization** → **Custom Instructions**:
```
When I ask about: PDF manipulation, Word documents, presentations, spreadsheets, visual design, frontend development, testing, or communications - I have expert skill guides I can paste. Ask me to share the relevant skill guide before proceeding with complex tasks in these domains.
```

---

## Google Gemini Integration

### Gemini Advanced (Google One AI Premium)
Go to **Settings** → **Extensions** or use **Gems**:

1. Create a new **Gem**
2. Add to instructions:
```
You are an expert assistant with specialized skills. When tasks involve:

DOCUMENTS: Follow professional standards for PDF forms, Word tracked changes, PowerPoint design, Excel financial modeling
DESIGN: Apply museum-quality visual principles, typography excellence, color theory
DEVELOPMENT: Use proper testing patterns, MCP protocols, React best practices
COMMUNICATIONS: Follow SBAR framework, audience-aware writing

Ask clarifying questions to identify the skill domain, then apply expert-level workflows.
```

### Gemini API
```python
import google.generativeai as genai

genai.configure(api_key="YOUR_API_KEY")

def create_skilled_model(skill_name):
    with open(f"skills/{skill_name}/SKILL.md") as f:
        skill_content = f.read()
    
    model = genai.GenerativeModel(
        model_name="gemini-pro",
        system_instruction=f"""You are an expert with this skill:

{skill_content}

Follow the workflows and best practices exactly."""
    )
    return model

# Use for PDF tasks
pdf_expert = create_skilled_model("pdf")
response = pdf_expert.generate_content("Extract tables from this PDF...")
```

### Google AI Studio
1. Create new prompt
2. Add skill content to **System Instructions**
3. Save as template for reuse

---

## Grok (X/Twitter) Integration

### Grok 2 Custom Instructions
In Grok settings or conversation:
```
I want you to act as an expert assistant with specialized skills. When I ask about:
- PDF/documents → Apply professional document processing standards
- Presentations → Use expert design principles (color theory, typography, visual hierarchy)
- Spreadsheets → Follow financial modeling best practices (color coding, formula standards)
- Web development → Apply production-grade frontend patterns
- Testing → Use comprehensive Playwright testing strategies

Ask which domain before complex tasks, then apply expert-level workflows.
```

### Grok API (when available)
```python
# Similar pattern to other APIs
system_prompt = f"Expert skill loaded:\n{skill_content}\nFollow workflows exactly."
```

---

## DeepSeek Integration

### DeepSeek Chat
Add to conversation or system prompt:
```
You have expert-level capabilities in these domains:

1. DOCUMENT PROCESSING: PDF manipulation (pypdf, pdfplumber, reportlab), Word documents (tracked changes, OOXML), Presentations (html2pptx, design principles), Spreadsheets (pandas, openpyxl, financial modeling)

2. DESIGN: Canvas art (Gestalt principles, color theory, typography), Frontend (12 aesthetic directions, CSS animations), Algorithmic art (noise functions, particle systems)

3. DEVELOPMENT: MCP servers (Python/Node patterns), Playwright testing (selectors, assertions, network interception), React artifacts (TypeScript, Tailwind, shadcn)

When asked about these domains, apply professional-grade workflows and best practices.
```

### DeepSeek API
```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_DEEPSEEK_KEY",
    base_url="https://api.deepseek.com"
)

response = client.chat.completions.create(
    model="deepseek-chat",
    messages=[
        {"role": "system", "content": skill_content},
        {"role": "user", "content": "..."}
    ]
)
```

---

## Perplexity Integration

### Perplexity Spaces
1. Create a new **Space**
2. Upload skill files or add URLs if hosted
3. Add to Space instructions:
```
This space contains expert-level skills for specialized tasks. Search the uploaded files for relevant SKILL.md documents before answering questions about: document processing, visual design, frontend development, testing, or communications.
```

### Perplexity API
```python
# Use with skill content in system message
headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}

payload = {
    "model": "llama-3.1-sonar-large-128k-online",
    "messages": [
        {"role": "system", "content": skill_content},
        {"role": "user", "content": "..."}
    ]
}
```

---

## Manus AI Integration

### Manus Custom Instructions
```
You have access to expert-level skills for complex tasks:

DOCUMENT SKILLS: Professional PDF processing, Word document workflows with tracked changes, presentation design with color palettes and typography, spreadsheet financial modeling standards

DESIGN SKILLS: Museum-quality visual art principles, production frontend interfaces, generative algorithmic art, consistent theming systems

DEVELOPMENT SKILLS: MCP server development, comprehensive Playwright testing, React/TypeScript artifact building

ENTERPRISE SKILLS: Brand identity application, document collaboration workflows, internal communications frameworks

Apply these expert workflows when tasks fall within these domains.
```

---

## Other Platforms

### Mistral (Le Chat / API)
```python
from mistralai.client import MistralClient

client = MistralClient(api_key="YOUR_KEY")
response = client.chat(
    model="mistral-large-latest",
    messages=[
        {"role": "system", "content": skill_content},
        {"role": "user", "content": "..."}
    ]
)
```

### Cohere
```python
import cohere

co = cohere.Client("YOUR_KEY")
response = co.chat(
    model="command-r-plus",
    preamble=skill_content,
    message="..."
)
```

### Hugging Face (Chat UI)
Add to system prompt in model settings:
```
You are an expert assistant. [Paste relevant skill content here]
```

### Local Models (Ollama, LM Studio, etc.)
Create a Modelfile or system prompt:
```
FROM llama3.1
SYSTEM """
[Paste skill content here]
"""
```

---

## Hosting Skills for Easy Access

### Option 1: GitHub Raw URLs
Reference skills directly:
```
https://raw.githubusercontent.com/anthropics/skills/main/skills/pdf/SKILL.md
```

### Option 2: GitHub Pages
Enable GitHub Pages to serve skills as a website:
```
https://anthropics.github.io/skills/pdf/SKILL.md
```

### Option 3: Create a Skills API
```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/skill/{name}")
def get_skill(name: str):
    with open(f"skills/{name}/SKILL.md") as f:
        return {"skill": name, "content": f.read()}
```

---

## Multi-Agent Frameworks

### LangGraph / LangChain
```python
from langchain.tools import Tool

def load_skill(skill_name: str) -> str:
    with open(f"skills/{skill_name}/SKILL.md") as f:
        return f.read()

skill_tool = Tool(
    name="load_skill",
    func=load_skill,
    description="Load expert skill guidelines. Use before: document creation (pdf/docx/pptx/xlsx), design (canvas-design/frontend-design), development (mcp-builder/webapp-testing)"
)
```

### CrewAI
```python
from crewai import Agent, Tool

@Tool
def get_skill(skill_name: str) -> str:
    """Load expert skill documentation"""
    with open(f"skills/{skill_name}/SKILL.md") as f:
        return f.read()

document_expert = Agent(
    role="Document Specialist",
    goal="Create professional documents following expert workflows",
    backstory="Expert trained on pdf, docx, pptx, xlsx skills",
    tools=[get_skill]
)

design_expert = Agent(
    role="Design Specialist", 
    goal="Create visually stunning artifacts",
    backstory="Expert trained on canvas-design, frontend-design, theme-factory skills",
    tools=[get_skill]
)
```

### AutoGen
```python
from autogen import AssistantAgent, UserProxyAgent

skill_agent = AssistantAgent(
    name="SkillExpert",
    system_message="""You have access to expert skills. When tasks involve:
- Documents → load pdf/docx/pptx/xlsx skills
- Design → load canvas-design/frontend-design skills
- Development → load mcp-builder/webapp-testing skills
Always read the full SKILL.md before proceeding."""
)
```

### Semantic Kernel
```python
import semantic_kernel as sk

kernel = sk.Kernel()

@kernel.function
def get_skill(skill_name: str) -> str:
    """Load expert skill for specialized tasks"""
    with open(f"skills/{skill_name}/SKILL.md") as f:
        return f.read()
```

---

## Platform Quick Reference

| Platform | Method | Where to Configure |
|----------|--------|-------------------|
| **VS Code Copilot** | `.github/copilot-instructions.md` | Repository root |
| **Cursor** | `.cursorrules` | Repository root |
| **Windsurf** | `.windsurfrules` | Repository root |
| **Continue.dev** | `config.json` | `~/.continue/` |
| **Claude Projects** | Project Knowledge + Instructions | claude.ai |
| **Claude API** | System prompt | Code |
| **ChatGPT Custom GPT** | Knowledge + Instructions | GPT Builder |
| **ChatGPT Free** | Custom Instructions | Settings |
| **OpenAI API** | Assistants + File Search | Code |
| **Gemini Gems** | Gem Instructions | gemini.google.com |
| **Gemini API** | System Instruction | Code |
| **Grok** | Custom Instructions | Settings |
| **DeepSeek** | System prompt | Chat or API |
| **Perplexity Spaces** | Space Knowledge + Instructions | perplexity.ai |
| **Manus** | Custom Instructions | Settings |
| **Mistral** | System prompt | Le Chat or API |
| **Cohere** | Preamble | API |
| **Local (Ollama)** | Modelfile SYSTEM | Modelfile |

---

## Best Practices

### 1. Lazy Loading
Don't load all skills upfront—load relevant skills on-demand based on task type.

### 2. Skill Routing
```python
import re

SKILL_ROUTING = {
    r"pdf|document|form|extract": "pdf",
    r"word|docx|track|redline": "docx", 
    r"presentation|slides|pptx|deck": "pptx",
    r"excel|spreadsheet|xlsx|financial": "xlsx",
    r"design|visual|art|canvas": "canvas-design",
    r"frontend|web|ui|interface": "frontend-design",
    r"test|playwright|e2e": "webapp-testing",
    r"mcp|server|protocol|tool": "mcp-builder",
    r"gif|animation|slack": "slack-gif-creator",
    r"theme|palette|styling": "theme-factory",
    r"brand|identity|logo": "brand-guidelines",
    r"memo|announcement|internal": "internal-comms",
}

def route_to_skill(query: str) -> list[str]:
    """Return relevant skills for a query"""
    query_lower = query.lower()
    return [skill for pattern, skill in SKILL_ROUTING.items() 
            if re.search(pattern, query_lower)]
```

### 3. Context Window Management
For large skills, load sections progressively:
1. Load YAML frontmatter first (metadata)
2. Load overview/workflow sections
3. Load detailed references only when needed

### 4. Skill Chaining
Some tasks benefit from multiple skills:

| Task | Skills to Combine |
|------|-------------------|
| Presentation design | `pptx` + `canvas-design` + `theme-factory` |
| Document collaboration | `docx` + `doc-coauthoring` |
| Web application | `web-artifacts-builder` + `frontend-design` + `webapp-testing` |
| Brand materials | `brand-guidelines` + `canvas-design` + `pptx` |
| Financial reports | `xlsx` + `pdf` + `docx` |
| Generative visuals | `algorithmic-art` + `canvas-design` |

### 5. Skill Validation
Before using a skill, verify it matches the task:
```python
def validate_skill_match(skill_name: str, task: str) -> bool:
    """Check if skill keywords match the task"""
    with open(f"skills/{skill_name}/SKILL.md") as f:
        content = f.read()
    
    # Extract keywords from YAML frontmatter
    import yaml
    frontmatter = content.split("---")[1]
    metadata = yaml.safe_load(frontmatter)
    keywords = metadata.get("description", "").lower()
    
    return any(kw in task.lower() for kw in keywords.split())
```

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Skill not being followed | Ensure full SKILL.md is in context, not just summary |
| Conflicting advice | Load only one skill at a time, or specify priority |
| Token limits exceeded | Use skill routing to load only relevant skills |
| Platform doesn't support files | Paste skill content directly into system prompt |
| Skill outdated | Pull latest from `anthropics/skills` repository |
