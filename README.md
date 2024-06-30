<div align="center">
  <h1>Mutahunter</h1>

  Open-Source Language Agnostic LLM-based Mutation Testing for Automated Software Testing
  
  Maintained by [CodeIntegrity](https://www.codeintegrity.ai). Anyone is welcome to contribute. 🌟

  [![GitHub license](https://img.shields.io/badge/License-AGPL_3.0-blue.svg)](https://github.com/yourcompany/mutahunter/blob/main/LICENSE)
  [![Twitter](https://img.shields.io/twitter/follow/CodeIntegrity)](https://twitter.com/CodeIntegrity)
  [![Discord](https://badgen.net/badge/icon/discord?icon=discord&label&color=purple)](https://discord.gg/K96jUJ3g)
  [![Unit Tests](https://github.com/codeintegrity-ai/mutahunter/actions/workflows/test.yaml/badge.svg)](https://github.com/codeintegrity-ai/mutahunter/actions/workflows/test.yaml)
  <a href="https://github.com/codeintegrity-ai/mutahunter/commits/main">
  <img alt="GitHub" src="https://img.shields.io/github/last-commit/codeintegrity-ai/mutahunter/main?style=for-the-badge" height="20">
  </a>
</div>


<div align="center">
<video src="https://github.com/codeintegrity-ai/mutahunter/assets/37044660/3aca86e0-593e-44a0-8d9e-d22ee808ca75"></video>
</div>


## Table of Contents

- [Overview](#overview)
- [Installation and Usage](#installation-and-usage)
- [Roadmap](#roadmap)

## Overview

If you don't know what mutation testing is, you must be living under a rock! 🪨

Mutation testing is a way to verify how good your test cases are. It involves creating small changes, or "mutants," in the code and checking if the test cases can catch these changes. Line coverage only tells you how much of the code has been executed, not how well it's been tested. We all know line coverage is bullshit.

Mutahunter leverages LLM models to inject context-aware faults into your codebase. As the first AI-based mutation testing tool, it surpasses traditional “dumb” AST-based methods. Mutahunter’s AI-driven approach provides full contextual understanding of the entire codebase, enabling it to identify and inject mutations that closely resemble real vulnerabilities. This ensures comprehensive and effective testing, significantly enhancing software security and quality.

Mutation testing is used by big tech companies like [Google](https://research.google/pubs/state-of-mutation-testing-at-google/) to ensure the robustness of their test suites. With Mutahunter, we want other companies and developers to use this powerful tool to enhance their test suites and improve software quality.

<!-- For more detailed technical information, engineers can visit: [Mutahunter Documentation](https://docs.mutahunter.ai) (WIP) -->

We added examples for JavaScript, Python, and Go (see [/examples](/examples)). It can theoretically work with any programming language that provides a coverage report in Cobertura XML format (more supported soon) and has a language grammar available in [TreeSitter](https://github.com/tree-sitter/tree-sitter).

## Installation and Usage

### Requirements

- LLM API Key (OpenAI, Anthropic, and others): Follow the instructions [here](https://litellm.vercel.app/docs/) to set up your environment.
- Cobertura XML code coverage report for a specific test suite.
- Python to install the Mutahunter package.

#### Python Pip

To install the Python Pip package directly via GitHub:

```bash
# Work with GPT-4o on your repo. See litellm for other models.
export OPENAI_API_KEY=your-key-goes-here

# Or, work with Anthropic's models. See litellm for other models.
export ANTHROPIC_API_KEY=your-key-goes-here

pip install git+https://github.com/codeintegrity-ai/mutahunter.git
```

### How to Execute Mutahunter

To use MutaHunter, you first need a Cobertura XML line coverage report of a specific test file. MutaHunter currently supports mutating on a per-test-file basis.

For more detailed examples and to understand how Mutahunter works in practice, please visit the [/examples](/examples/python_fastapi/) directory in our GitHub repository.

To see the available options for the Mutahunter run command, use the following command:

```bash
mutahunter run -h
```

Example command to run Mutahunter on a Python FastAPI [application](/examples/python_fastapi/):

```bash
mutahunter run --test-command "pytest test_app.py" --test-file-path "test_app.py" --code-coverage-report-path "coverage.xml" --only-mutate-file-paths "app.py"
```

The mutahunter run command has the following options:

```plaintext
Options:
  --model <MODEL>
      Description: LLM model to use for mutation testing. We use LiteLLM to call the model.
      Default: `gpt-4o`
      Required: Yes
      Example: `--model gpt-4o`

  --test-command <COMMAND>
      Description: The command used to execute the tests. Specify a single test file to run the tests on.
      Required: Yes
      Example: `--test-command pytest test_app.py`

  --code-coverage-report-path <PATH>
      Description: Path to the code coverage report of the test suite.
      Required: Yes
      Example: `--code-coverage-report-path /path/to/coverage.xml`

  --test-file-path <PATH>
      Description: Path to the test file to run the tests on.
      Required: Yes
      Example: `--test-file-path /path/to/test_file.py`

  --exclude-files <FILES>
      Description: Files to exclude from analysis.
      Required: No
      Example: `--exclude-files file1.py file2.py`

  --only-mutate-file-paths <FILES>
      Description: Specifies which files to mutate. This is useful when you want to focus on specific files and it makes the mutations faster!
      Required: No
      Example: `--only-mutate-file-paths file1.py file2.py`
```

#### Mutation Testing Report

Check the logs directory to view the report:

- `mutants_killed.json` - Contains the list of mutants that were killed by the test suite.
- `mutants_survived.json` - Contains the list of mutants that survived the test suite.
- `mutation_coverage.json` - Contains the mutation coverage report.

An example survived mutant information would be like so:

```json
[
  {
    "id": "4",
    "source_path": "src/mutahunter/core/analyzer.py",
    "mutant_path": "/Users/taikorind/Documents/personal/codeintegrity/mutahunter/logs/_latest/mutants/4_analyzer.py",
    "status": "SURVIVED",
    "error_msg": "",
    "test_file_path": "tests/test_analyzer.py",
    "diff": "                for line in range(start_line, end_line + 1):\n-                    function_executed_lines.append(line - start_line + 1)\n+                    function_executed_lines.append(line - start_line) # Mutation: Change the calculation of executed lines to start from 0 instead of 1.\n"
  },
]
```

## Roadmap

### Automatic Mutation Testing

- [x] **Fault Injection:** Utilize advanced LLM models to inject context-aware faults into the codebase, ensuring comprehensive mutation testing.
- [x] **Language Support:** Expand support to include various programming languages.

### Enhanced Mutation Coverage

- [ ] **Support for Other Coverage Report Formats:** Add compatibility for various coverage report formats.
- [ ] **PR Changeset Focus:** Generate mutations specifically targeting pull request changesets or modified code based on commit history.

### Usability Improvements

- [ ] **CI/CD Integration:** Develop connectors for popular CI/CD platforms like GitHub Actions, Jenkins, CircleCI, and Travis CI.
- [ ] **Dashboard:** Create a user-friendly dashboard for visualizing test results and coverage metrics.
- [ ] **Data Integration:** Integrate with databases, APIs, OpenTelemetry, and other data sources to extract relevant information for mutation testing.

---

## Acknowledgements

Mutahunter makes use of the following open-source libraries:

- [aider](https://github.com/paul-gauthier/aider) by Paul Gauthier, licensed under the Apache-2.0 license.
- [TreeSitter](https://github.com/tree-sitter/tree-sitter) by TreeSitter, MIT License.
- [LiteLLM](https://github.com/BerriAI/litellm) by BerriAI, MIT License.

For more details, please refer to the LICENSE file in the repository.
