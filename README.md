# LLM Code Documentation Generator

`llmdocgen` is a library for automatically documenting code using large language models (LLMs). It takes code files as
input, sends them to an LLM to generate documentation comments, and returns the code with the generated documentation
added.

## Features

- Automatically documents code using LLMs
- Supports a wide variety of programming languages out of the box
- Easily configurable to add support for additional file types
- Can be used as a library or run as a command line tool
- Integrates with [LiteLLM](https://docs.litellm.ai) to work with OpenAI, Anthropic, and other LLM providers

## Installation

Install from PyPI using pip:

```bash
pip install llmdocgen
```

## Setup

You can use the default environment file path (`~/.llmdocgen`) or specify a custom environment file path
by setting the `LLMDOCGEN_ENV_PATH` environment variable.


`llmdocgen` requires a few environment variables to be set in order to authenticate with the LLM provider. The easiest
way to set these variables is to create your environment file with the following variables:

```bash
# Required for LiteLLM

# For Anthropic
LITELLM_MODEL=claude-3-opus-20240229  # or another supported model
ANTHROPIC_API_KEY=your_api_key

# For OpenAI
OPENAI_API_KEY=your_api_key
LITELLM_MODEL=gpt-4-turbo  # or another supported model

# llmdocgen settings
PROMPT_FILE=default_prompt.txt
ESCAPE_CHARACTERS=%"%"%"

# See settings.py for additional optional variables
```

See the LiteLLM docs for more details on authenticating.

## Usage

There are two main ways to use llmdocgen:

### As a library

```python
from llmdocgen import enrich
import pathlib

file_path = pathlib.Path("path/to/file.py")
completion = enrich.parsed_file_completion(file_path)
print(completion)
```

To run and save completion to the original file:
```python
from llmdocgen import executor
import pathlib

file_path = pathlib.Path("path/to/file.py")
executor.inline_document_file(file_path, save=True)

```

To run and save to a different file:
```python
from llmdocgen import executor
import pathlib

original_file_path = pathlib.Path("path/to/file.py")
new_file_path = pathlib.Path("path/to/new_file.py")
executor.inline_document_file(original_file_path, save=True, new_file_path=new_file_path)

```


This will print out the code from file_path with documentation comments added.

### As a command line tool

You can also run llmdocgen as a module:

```bash
python -m llmdocgen /path/to/directory --recursive
```

This will process all supported files in /path/to/directory. Pass --recursive to also process subdirectories.

## Configuration

### Supported File Types

By default, llmdocgen supports a wide variety of file types (see settings.DEFAULT_EXTENSIONS). You can add additional
file types by setting the LLMDOCGEN_EXTRA_EXTENSIONS environment variable to a comma-separated list of extensions.
To override the default list entirely, set the LLMDOCGEN_EXTENSIONS environment variable.

### Prompts

`llmdocgen` uses a prompt defined in default_prompt.txt to instruct the LLM on how to generate the documentation. You can
modify this prompt or provide a different file by setting the PROMPT_FILE environment variable.
The prompt should include %"%"% where the generated code should be inserted. llmdocgen will use this to extract the
generated code from the LLM's response.

### Authenticating with LLMs

As mentioned above, `llmdocgen` uses LiteLLM to integrate with LLM providers. All authentication happens through LiteLLM
by setting the appropriate environment variables.
See settings.LITELLM for the full list of supported settings. These map directly to arguments to LiteLLM's
litellm.completion() function.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request on the GitHub repository.

## License

`llmdocgen` is released under the MIT License. See [LICENSE](LICENSE) for more information.
