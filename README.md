## Installation Guide (using UV  )
Here is how you configure a **pytest + Playwright** project specifically using **uv**.
## Install UV 
     https://docs.astral.sh/uv/getting-started/installation/

Use curl to download the script and execute it with sh:

curl -LsSf https://astral.sh/uv/install.sh | sh

or wget if you don't have curl

wget -qO- https://astral.sh/uv/install.sh | sh

---

## 1. Initialize and Add Dependencies
If you are starting fresh or migrating, use the following commands to let `uv` manage your environment and lockfile:

```bash
# Initialize a new project (if you haven't)
uv init

# Add pytest and the Playwright plugin
uv add pytest pytest-playwright

# Install the Playwright browsers into the uv-managed environment
uv run playwright install
```

> [!TIP]
> Using `uv run` ensures that the browsers are installed specifically for the virtual environment `uv` created, preventing "browser not found" errors in VS Code.

---

## 2. VS Code Integration with uv
To make sure VS Code uses the environment `uv` created:

1.  **Select the Interpreter:** Press `Ctrl+Shift+P`, type **"Python: Select Interpreter"**, and look for the one labeled `(.venv)`. `uv` always creates its environment in a `.venv` folder in your project root.
2.  **Detection:** Once selected, the **Testing** tab (Beaker icon) will automatically use the `uv` environment to discover your pytest tests.

---

## 3. Running Tests via uv
While you can use the VS Code UI, running from the terminal with `uv` is helpful for CI/CD or quick headless runs:

| Command | Purpose |
| :--- | :--- |
| `uv run pytest` | Runs all tests using the lockfile environment. |
| `uv run pytest --headed` | Runs tests with the browser window visible. |
| `uv run pytest -n auto` | Runs tests in parallel (if `pytest-xdist` is added). |

---

## 4. Example `pyproject.toml` Configuration
Since `uv` uses `pyproject.toml` as the source of truth, you can bake your pytest/playwright configurations directly into that file instead of having a separate `pytest.ini`.

```toml
[tool.pytest.ini_options]
testpaths = ["tests"]
addopts = [
    "--browser", "chromium",
    "--headed",
    "--slowmo", "500",
    "--base-url", "http://localhost:3000"
]
```

---

## Why use uv for this?
* **Performance:** `uv` installs your testing dependencies in milliseconds.
* **Reproducibility:** The `uv.lock` file ensures that everyone on your team (and your CI/CD pipeline) is using the exact same versions of Playwright and Pytest.
* **Tool Management:** You can run Playwright commands without "activating" an environment manually by using `uv run`.

Are you planning to run these tests in a CI/CD pipeline like GitHub Actions? I can show you how to set up the `uv` cache there to make your test runs lightning-fast.
