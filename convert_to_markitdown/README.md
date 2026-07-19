# Setup
The source of the codes is from [https://www.youtube.com/watch?v=UFEVQdJeWeQ].The original repo for markitdown is at [https://github.com/microsoft/markitdown].

### 1. Downloading the packages
- Create the folder where the project will reside
- Create and activate environment for the package:
  ```bash
  uv venv
  .venv\Scripts\activate
  ```
- Install the package
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
  Deactivate enviroment and exit
  ```bash
  deactivate
  exit
  ```
