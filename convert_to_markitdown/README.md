# Setup
(The source of the codes is from [https://www.youtube.com/watch?v=UFEVQdJeWeQ].

### 1. Install CLAUDE

Open a standard Windows PowerShell window (you don't even need to run it as Administrator) and paste the following network command to initiate the secure download:

```bash
irm https://claude.ai/install.ps1 | iex
```

### 1. Downloading the packages

- Create the folder where the project will reside
- Create and activate environment for the package:
  ```bash
  uv venv
  .venv\Scripts\activate
  ```
- Install the packages
```bash 
uv pip install markitdown[all]
```

### 2. Get into the environment
- Get into folder where the project file will resides
- Activate the environment:
```bash
.venv\Scripts\activate
```
- Run the following to convert source file to markdown:
```bash
uv run markitdown .\raw\[name of source file].pdf > [name of output file].md
```
