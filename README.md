# 🦊 CursorRules Architect V2

<div align="center">

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
[![OpenAI](https://img.shields.io/badge/OpenAI-o1%20%7C%20o3--mini%20%7C%20gpt--4o-blue.svg)](https://openai.com/)
[![Anthropic](https://img.shields.io/badge/Anthropic-claude--3.7--sonnet-purple.svg)](https://www.anthropic.com/)
[![DeepSeek](https://img.shields.io/badge/DeepSeek-deepseek--reasoner-red.svg)](https://deepseek.com/)
[![Built By](https://img.shields.io/badge/Built%20By-SlyyCooper-orange.svg)](https://github.com/SlyyCooper)

**Your Multi-Provider AI Code Analysis and .cursorrules Generator 🚀**

[Features](#-features) • [Requirements](#-requirements) • [Installation](#-installation) • [Usage](#-usage) • [Configuration](#-configuration) • [Architecture](#-architecture) • [Output](#-output) • [Contributing](#-contributing)

</div>

## 🌟 What's This All About?

CursorRules Architect V2 is an advanced multi-agent system that analyzes your codebase using a powerful combination of AI models from Anthropic, OpenAI, and DeepSeek. It performs a comprehensive six-phase analysis to understand your project's structure, dependencies, patterns, and architectural decisions. The result is a detailed report and automatically generated `.cursorrules` and `.cursorignore` files customized for your project.

## ✨ Features

- 🌐 **Multi-Provider Support** - Leverage AI models from Anthropic, OpenAI, and DeepSeek
- 🧠 **Enhanced Reasoning** - Different reasoning modes (enabled/disabled, low/medium/high, temperature)
- 🤖 **Dynamic Agents** - Creates specialized analysis agents based on your specific codebase
- 🔍 **Six-Phase Analysis** - Structured pipeline that builds comprehensive understanding
- 🔄 **Async Processing** - Parallel agent execution for faster analysis
- 📊 **Detailed Metrics** - Track analysis time and token usage
- 📝 **Comprehensive Documentation** - Generated reports for each phase and component
- 🎨 **Intelligent Rule Generation** - Creates optimal `.cursorrules` files for your coding style
- 📑 **Multi-Format Output** - Separate markdown files for each analysis phase
- 🚫 **Smart Exclusions** - Customizable patterns to focus analysis on relevant files
- 🔧 **Fully Configurable** - Easy to customize which models are used for each phase

## 🛠️ Requirements

- Python 3.8+
- API keys for at least one of the supported providers:
  - Anthropic API key with access to `claude-3-7-sonnet-20250219`
  - OpenAI API key with access to `o1`, `o3-mini`, or `gpt-4o`
  - DeepSeek API key with access to DeepSeek Reasoner
- Dependencies:
  - `anthropic` for Anthropic API access
  - `openai` for OpenAI API access
  - `rich` for beautiful terminal output
  - `click` for CLI interface
  - `pathlib` for path manipulation
  - `asyncio` for async operations

## 📦 Installation

1. **Clone the Repository**
   ```bash
   git clone https://github.com/slyycooper/cursorrules-architect.git
   cd cursorrules-architect
   ```

2. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Set Up API Keys**
   ```bash
   # Linux/macOS
   export ANTHROPIC_API_KEY='your-anthropic-api-key'
   export OPENAI_API_KEY='your-openai-api-key'
   export DEEPSEEK_API_KEY='your-deepseek-api-key'

   # Windows
   set ANTHROPIC_API_KEY=your-anthropic-api-key
   set OPENAI_API_KEY=your-openai-api-key
   set DEEPSEEK_API_KEY=your-deepseek-api-key
   ```

   Alternatively, create a `.env` file in the project root:
   ```
   ANTHROPIC_API_KEY=your-anthropic-api-key
   OPENAI_API_KEY=your-openai-api-key
   DEEPSEEK_API_KEY=your-deepseek-api-key
   ```

## 🚀 Usage

### Basic Usage

```bash
python main.py -p /path/to/your/project
```

### Advanced Options

```bash
# Use the new architecture (recommended)
python main.py -p /path/to/your/project -n

# Specify output location (deprecated, now uses standardized output)
python main.py -p /path/to/your/project -o output.txt
```

## ⚙️ Configuration

CursorRules Architect V2 allows you to customize which AI models are used for each analysis phase through the `config/agents.py` file.

### Model Configurations

The system defines several predefined model configurations you can use:

```python
# Anthropic Configurations
CLAUDE_BASIC = ModelConfig(
    provider=ModelProvider.ANTHROPIC,
    model_name="claude-3-7-sonnet-20250219",
    reasoning=ReasoningMode.DISABLED
)

CLAUDE_WITH_REASONING = ModelConfig(
    provider=ModelProvider.ANTHROPIC,
    model_name="claude-3-7-sonnet-20250219",
    reasoning=ReasoningMode.ENABLED
)

# OpenAI Configurations
O1_HIGH = ModelConfig(
    provider=ModelProvider.OPENAI,
    model_name="o1",
    reasoning=ReasoningMode.HIGH
)

O3_MINI_MEDIUM = ModelConfig(
    provider=ModelProvider.OPENAI,
    model_name="o3-mini",
    reasoning=ReasoningMode.MEDIUM
)

GPT4O_CREATIVE = ModelConfig(
    provider=ModelProvider.OPENAI,
    model_name="gpt-4o",
    reasoning=ReasoningMode.TEMPERATURE,
    temperature=0.9
)

# DeepSeek Configurations
DEEPSEEK_REASONER = ModelConfig(
    provider=ModelProvider.DEEPSEEK,
    model_name="deepseek-reasoner",
    reasoning=ReasoningMode.ENABLED
)
```

### Customizing Phase Models

To change which model is used for each phase, simply update the `MODEL_CONFIG` dictionary:

```python
MODEL_CONFIG = {
    "phase1": CLAUDE_WITH_REASONING,  # Use Claude with reasoning for Phase 1
    "phase2": O1_HIGH,                # Use OpenAI's o1 with high reasoning for Phase 2
    "phase3": CLAUDE_WITH_REASONING,  # Use Claude with reasoning for Phase 3
    "phase4": O1_HIGH,                # Use OpenAI's o1 with high reasoning for Phase 4
    "phase5": CLAUDE_WITH_REASONING,  # Use Claude with reasoning for Phase 5
    "final": O1_HIGH,                 # Use OpenAI's o1 with high reasoning for final analysis
}
```

### Exclusion Settings

You can customize which files and directories are excluded from analysis by modifying `config/exclusions.py`:

```python
EXCLUDED_DIRS = {
    'node_modules', '.next', '.git', 'venv', '__pycache__', 
    'dist', 'build', '.vscode', '.idea', 'coverage',
    # Add your custom directories here
}

EXCLUDED_FILES = {
    'package-lock.json', 'yarn.lock', '.DS_Store', '.env',
    # Add your custom files here
}

EXCLUDED_EXTENSIONS = {
    '.jpg', '.jpeg', '.png', '.gif', '.ico', '.svg', 
    '.pyc', '.pyo', '.pyd', '.so', '.db', '.sqlite',
    # Add your custom extensions here
}
```

## 🏗️ Architecture

CursorRules Architect V2 follows a sophisticated multi-phase analysis approach:

### 1. Base Architecture

The system is built on a `BaseArchitect` abstract class that standardizes how different AI model providers are integrated:

- `AnthropicArchitect` - Interface to Anthropic's Claude models
- `OpenAIArchitect` - Interface to OpenAI's models (o1, o3-mini, gpt-4o)
- `DeepSeekArchitect` - Interface to DeepSeek's reasoning models

Each architect implements standardized methods:
- `analyze()` - Runs general analysis
- `create_analysis_plan()` - Creates a detailed analysis plan (Phase 2)
- `synthesize_findings()` - Synthesizes findings from deep analysis (Phase 4)
- `consolidate_results()` - Consolidates all analysis results (Phase 5)
- `final_analysis()` - Provides final architectural insights

### 2. Analysis Pipeline

#### Phase 1: Initial Discovery
Performs initial exploration of the project structure, dependencies, and technology stack using specialized agents:
- Structure Agent: Analyzes directory and file organization
- Dependency Agent: Investigates package dependencies
- Tech Stack Agent: Identifies frameworks and technologies

#### Phase 2: Methodical Planning
Creates a detailed analysis plan using findings from Phase 1:
- Defines specialized agents with specific responsibilities
- Assigns files to relevant agents based on expertise
- Provides detailed instructions for deeper analysis
- Outputs an XML-structured plan that guides Phase 3

#### Phase 3: Deep Analysis
The heart of the system - dynamically creates specialized agents based on Phase 2's output:
- Each agent focuses on its assigned files and responsibilities
- Agents run in parallel for efficiency
- Performs in-depth analysis of code patterns, architecture, and dependencies
- Falls back to predefined agents if Phase 2 doesn't provide valid definitions

#### Phase 4: Synthesis
Synthesizes findings from Phase 3 into cohesive insights:
- Integrates agent findings into a holistic view
- Identifies relationships between components
- Highlights key architectural patterns
- Updates analysis directions

#### Phase 5: Consolidation
Consolidates results from all previous phases into a comprehensive report:
- Organizes findings by component/module
- Creates comprehensive documentation
- Prepares data for final analysis

#### Final Analysis
Provides high-level insights and recommendations:
- System structure mapping
- Architecture pattern identification
- Relationship documentation
- Improvement recommendations

### 3. Reasoning Modes

The system supports different reasoning modes depending on the model:

- For Anthropic models:
  - `ENABLED` - Use extended thinking capability
  - `DISABLED` - Standard inference

- For OpenAI models:
  - For O1 and O3-mini:
    - `LOW`/`MEDIUM`/`HIGH` - Different reasoning effort levels
  - For GPT-4o:
    - `TEMPERATURE` - Use temperature-based sampling

- For DeepSeek models:
  - Always uses `ENABLED` reasoning mode

## 📂 Project Structure

```
cursorrules-architect/
├── config/                      # Configuration settings
│   ├── agents.py                # Model and agent configuration
│   ├── exclusions.py            # Exclusion patterns for analysis
│   ├── prompts/                 # Centralized prompt templates
│       ├── phase_1_prompts.py   # Phase 1 agent prompts
│       ├── phase_2_prompts.py   # Phase 2 planning prompts
│       ├── phase_4_prompts.py   # Phase 4 synthesis prompts
│       ├── phase_5_prompts.py   # Phase 5 consolidation prompts
│       └── final_analysis_prompt.py # Final analysis prompts
├── core/                        # Core functionality
│   ├── agents/                  # Agent implementations
│   │   ├── anthropic.py         # Anthropic agent implementation
│   │   ├── base.py              # Base architect abstract class
│   │   ├── deepseek.py          # DeepSeek agent implementation
│   │   └── openai.py            # OpenAI agent implementation
│   ├── analysis/                # Analysis phase implementations
│   │   ├── final_analysis.py    # Final Analysis phase
│   │   ├── phase_1.py           # Initial Discovery phase
│   │   ├── phase_2.py           # Methodical Planning phase
│   │   ├── phase_3.py           # Deep Analysis phase
│   │   ├── phase_4.py           # Synthesis phase
│   │   └── phase_5.py           # Consolidation phase
│   └── types/                   # Type definitions
│       └── agent_config.py      # Agent configuration types
├── utils/                       # Utility functions
│   ├── file_creation/           # File creation utilities
│   │   ├── cursorignore.py      # .cursorignore management
│   │   ├── cursorrules.py       # .cursorrules management
│   │   └── phases_output.py     # Phase output saving
│   └── tools/                   # Tool utilities
│       ├── agent_parser.py      # Parser for Phase 2 output
│       ├── file_retriever.py    # File content retrieval
│       └── tree_generator.py    # Directory tree generation
├── main.py                      # Main entry point
└── requirements.txt             # Project dependencies
```

## 📊 Output

CursorRules Architect V2 generates a rich set of output files:

```
your-project/
├── .cursorrules                 # Generated rules file for Cursor IDE
├── .cursorignore                # Generated ignore patterns for Cursor IDE
└── phases_output/               # Detailed phase outputs
    ├── phase1_discovery.md      # Initial agent findings
    ├── phase2_planning.md       # Planning document with agent assignments
    ├── phase3_analysis.md       # Deep analysis results from dynamic agents
    ├── phase4_synthesis.md      # Synthesized findings
    ├── phase5_consolidation.md  # Consolidated report
    ├── final_analysis.md        # Final recommendations
    ├── complete_report.md       # Overview of all phases
    └── metrics.md                  # Analysis metrics
```

### Analysis Metrics

The system tracks performance metrics for the analysis:
- Total analysis time
- Token usage for phases using reasoning models
- Per-agent execution times

## 💡 Advanced Features

### Dynamic Agent Creation

The system's key innovation is the dynamic agent creation process:

1. **Phase 2 (Planning)**: 
   - Creates an XML-structured output defining specialized agents
   - Each agent is assigned responsibilities and specific files

2. **Agent Parser**:
   - Parses the XML output from Phase 2
   - Creates a structured representation of agent definitions
   - Includes fallback mechanisms for handling parsing issues

3. **Phase 3 (Dynamic Analysis)**:
   - Creates AI agents based on the extracted definitions
   - Each agent only analyzes its assigned files
   - Uses custom-formatted prompts for each agent's role

### Multi-Provider Flexibility

You can run the system with one or more AI providers:

- **Anthropic-only**: Set all phases to use Claude models
- **OpenAI-only**: Set all phases to use o1, o3-mini, or gpt-4o
- **DeepSeek-only**: Set all phases to use DeepSeek Reasoner
- **Mix and match**: Use different providers for different phases

### Customizing Prompts

For advanced users, you can modify the prompt templates in the `config/prompts/` directory to customize how agents analyze your code.

## 🤝 Contributing

We welcome contributions! Here's how you can help:

1. **Fork the Repository**: Create your own fork to work on
2. **Make Your Changes**: Implement your feature or bug fix
3. **Run Tests**: Ensure your changes don't break existing functionality
4. **Submit a Pull Request**: Send us your contributions for review

See [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines.

## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

---

<div align="center">

Built with 💙 using [Claude-3.7-Sonnet](https://www.anthropic.com/claude), [o1](https://openai.com/), and [DeepSeek Reasoner](https://deepseek.com/)

</div>