# real-time-graph-based-risk-engine-dbt
dbt repository for https://github.com/Pxyzi3c/real-time-graph-based-risk-engine
# Setting Up dbt with WSL2 for Data Analytics

---

## Why WSL for dbt and Not Native Windows?

Leveraging **Windows Subsystem for Linux (WSL)** for dbt development offers significant advantages over a native Windows installation:

* **Linux-Native Workflow:** dbt was designed with Linux and macOS command-line interface (CLI) workflows in mind. Many common Windows-specific issues simply disappear when running dbt within a WSL environment.
* **Lightweight and Efficient:** WSL provides a complete Linux environment without the overhead and complexity of containerization solutions like Docker.
* **Production Alignment:** This setup mirrors typical FAANG (Facebook, Amazon, Apple, Netflix, Google) and other enterprise environments that often prioritize Linux-first production deployments.
* **Seamless CLI Experience:** You'll work directly within a familiar Linux command line, which is ideal for dbt's powerful CLI tools.

---

## Installation Guide: Setting Up dbt in WSL2

Follow these steps to get dbt up and running efficiently within your WSL2 environment.

### ✅ Step 1: Enable and Install WSL2

First, ensure WSL2 is enabled and installed on your Windows machine.

1.  Open **PowerShell as an Administrator**.
2.  Run the following command:
    ```powershell
    wsl --install
    ```
    This command installs WSL2 and sets up Ubuntu as its default Linux distribution.
3.  **Reboot your computer** when prompted to finalize the installation.
4.  After rebooting, open the **Ubuntu application** from your Start menu to complete the initial setup of your Linux environment.

### ✅ Step 2: Set Up Python in WSL

Next, we'll install Python and its package manager within your Ubuntu WSL instance.

1.  Open your **Ubuntu terminal** (from the Start menu).
2.  Update your package lists and upgrade existing packages:
    ```bash
    sudo apt update && sudo apt upgrade -y
    ```
3.  Install Python 3, pip (Python package installer), and venv (for virtual environments):
    ```bash
    sudo apt install python3 python3-pip python3-venv -y
    ```
4.  Verify the installed Python versions:
    ```bash
    python3 --version
    pip3 --version
    ```

### ✅ Step 3: Create a Virtual Environment for dbt

It's best practice to use a virtual environment to manage dbt's dependencies, keeping them isolated from your system-wide Python installation.

1.  Navigate to your dbt project directory within WSL. To access your Windows drives, they are mounted under `/mnt/`. For example:
    ```bash
    cd /mnt/c/Users/<YourUsername>/path-to-project
    ```
    *Replace `<YourUsername>` and `path-to-project` with your actual Windows username and project path.*
2.  Create and activate your virtual environment:
    ```bash
    python3 -m venv venv
    source venv/bin/activate
    ```
3.  Upgrade `pip` to its latest version within the activated environment:
    ```bash
    pip install --upgrade pip
    ```

### ✅ Step 4: Install dbt

Now, install dbt Core along with the PostgreSQL adapter within your virtual environment.

1.  Install `dbt-core` and `dbt-postgres`:
    ```bash
    pip install dbt-core dbt-postgres
    ```
2.  Confirm the dbt installation:
    ```bash
    dbt --version
    ```
    You should see output similar to this, confirming both `dbt-core` and the `postgres` plugin are installed:
    ```
    Core:
      - installed: x.x.x
      - latest:    x.x.x
    Plugins:
      - postgres: x.x.x
    ```

### ✅ Step 5: Set Up a Test dbt Project

Let's create a basic dbt project to start with.

1.  In your project directory (while your virtual environment is active), initialize a new dbt project:
    ```bash
    dbt init risk_engine_dbt
    ```
2.  Follow the interactive prompts, making sure to **select `postgres` as your database adapter** when asked.
    This command generates a standard dbt project structure, including the `models/` directory, `dbt_project.yml` configuration file, and other essential components.

### ✅ Step 6: Save `requirements.txt` (WSL Version)

After successfully installing dbt and its adapter, capture all the installed Python packages into a `requirements.txt` file. This is crucial for replicating your environment.

1.  With your virtual environment still active, run:
    ```bash
    pip freeze > requirements.txt
    ```
    A sample `requirements.txt` output might look like this:
    ```
    dbt-core==1.7.8
    dbt-postgres==1.7.8
    Jinja2==3.1.2
    psycopg2-binary==2.9.9
    # ... other dependencies
    ```

### ✅ Step 7: Connect to PostgreSQL from WSL

Finally, configure dbt to connect to your PostgreSQL database.

* **If PostgreSQL is running on Windows:**
    Use `localhost` as the host and `5432` (or your configured port) as the port in your dbt `profiles.yml` file.
    * **Important:** You might need to edit your PostgreSQL's `pg_hba.conf` file (e.g., located at `/etc/postgresql/14/main/pg_hba.conf` if installed on WSL, or a similar path if on Windows) to allow host-based connections from WSL.

* **Alternatively, install PostgreSQL directly in WSL:**
    ```bash
    sudo apt install postgresql postgresql-contrib -y
    ```
    This can simplify networking between dbt and PostgreSQL within the WSL environment.

### Check dbt Connection

To ensure dbt can communicate with your database, run the debug command:

```bash
dbt debug