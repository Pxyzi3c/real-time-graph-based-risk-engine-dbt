> [!NOTE]
> This is a ``dbt`` repository for [Real-time Graph-based Risk Engine](https://github.com/Pxyzi3c/real-time-graph-based-risk-engine).
# ``dbt`` Setup Using WSL

## Why ``WSL`` for dbt and Not Native Windows?

* dbt was built for **Linux/Mac CLI** workflows — most Windows issues disappear inside WSL.
* It's lightweight and avoids Docker complexity.
* Aligns with FAANG/Linux-first production environments.

_Use ``wsl`` command line_

## ✅ Step 1: Enable and Install WSL2

1.  Open **PowerShell as an Administrator** and run.
    ```powershell
    wsl --install
    ```
    This installs WSL2 with Ubuntu by default.
2.  **Reboot** when prompted.
3.  After reboot, open Ubuntu from Start menu.

## ✅ Step 2: Set Up Python in WSL

Next, we'll install Python and its package manager within your Ubuntu WSL instance.

1.  Inside **Ubuntu terminal** (from the Start menu).
    ```bash
    sudo apt update && sudo apt upgrade -y
    sudo apt install python3 python3-pip python3-venv -y
    ```
2.  Check Python versions:
    ```bash
    python3 --version
    pip3 --version
    ```

## ✅ Step 3: Create a Virtual Environment for dbt

It's best practice to use a virtual environment to manage dbt's dependencies, keeping them isolated from your system-wide Python installation.

1.  Navigate to your dbt project directory in WSL.
    ```bash
    cd /mnt/c/Users/<YourUsername>/path-to-project
    ```
2.  Create and activate your virtual environment:
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    ```
3.  Upgrade `pip` to its latest version within the activated environment:
    ```bash
    pip install --upgrade pip
    ```

## ✅ Step 4: Install dbt

1.  Install `dbt-core` and `dbt-postgres` adapter:
    ```bash
    pip install dbt-core dbt-postgres
    ```
2.  Confirm the dbt installation:
    ```bash
    dbt --version
    ```
    Expected output:
    ```
    Core:
      - installed: x.x.x
      - latest:    x.x.x
    Plugins:
      - postgres: x.x.x
    ```

## ✅ Step 5: Set Up a Test dbt Project

Let's create a basic dbt project to start with.

1.  In your project directory (while your virtual environment is active), initialize a new dbt project:
    ```bash
    dbt init risk_engine_dbt
    ```
2.  Follow the prompts and select Postgres as your adapter.
3.  This generates a standard dbt project structure with `models/`, `dbt_project.yml` configuration file, etc.

## ✅ Step 6: Save `requirements.txt` (WSL Version)

After successful install:
```bash
pip freeze > requirements.txt
```
Sample output:
```
dbt-core==1.7.8
dbt-postgres==1.7.8
Jinja2==3.1.2
psycopg2-binary==2.9.9
# ... other dependencies
```

## ✅ Step 7: Connect to PostgreSQL from WSL

Finally, configure dbt to connect to your PostgreSQL database.

* **If PostgreSQL is running on Windows:**
    Use `localhost` as the host and `5432` (or your configured port) as the port in your dbt `profiles.yml` file.
    * **Important:** You might need to edit your PostgreSQL's `pg_hba.conf` file (e.g., located at `/etc/postgresql/14/main/pg_hba.conf` if installed on WSL, or a similar path if on Windows) to allow host-based connections from WSL.

* **Alternatively, install PostgreSQL directly in WSL:**
    ```bash
    sudo apt install postgresql postgresql-contrib -y
    ```
    This can simplify networking between dbt and PostgreSQL within the WSL environment.

## Check dbt Connection

To ensure dbt can communicate with your database, run the debug command:

```bash
dbt debug
```

If everything is configured correctly and your dbt project is set up, you can run your dbt models:

```bash
dbt run