# Building a Data Pipeline with Airflow and PostgreSQL

In this article, I will walk you through building an automated ETL pipeline using Apache Airflow, Docker, and PostgreSQL. This pipeline will fetch, clean, and store data, showcasing the power of Airflow for orchestrating complex workflows. Let's dive in!

---

## Step 1: Initialize Python and Airflow Environments

### Setting Up a Python Virtual Environment
Before starting with Airflow, it's good practice to set up a virtual environment to manage dependencies.

1. **Create a virtual environment:**
   ```bash
   python -m venv venv
   ```
2. **Activate the virtual environment:**
   ```bash
   .venv\Scripts\activate  # On Windows
   source venv/bin/activate  # On macOS/Linux
   ```

### Setting Up Airflow with Docker
Airflow's Docker setup simplifies installation and environment management. Here's how to get started:

1. **Download the Airflow Docker Compose YAML file:**
   ```bash
   curl 'https://airflow.apache.org/docs/apache-airflow/2.10.3/docker-compose.yaml' -o "docker-compose.yaml"
   ```

2. **Create necessary directories:**
   ```bash
   mkdir -p ./dags ./logs ./plugins ./config
   ```

3. **Initialize the Airflow Docker image:**
   ```bash
   docker compose up airflow-init
   ```

4. Ensure Docker is installed on your machine before proceeding. Once initialized, Airflow will be ready for use on port `8080`.

---

## Step 2: Connect Airflow with PostgreSQL

### Configuring PostgreSQL
We’ll use PostgreSQL as our database for this project. Airflow allows easy integration with external databases, and here’s how to configure it:

1. The Airflow UI runs on port `8080` by default. Login with the default credentials (`airflow`/`airflow`).

2. **Set up PostgreSQL database using pgAdmin:**
   - Add a PostgreSQL service in the YAML configuration file.
   - Set the database port to `5432` for easier connection management.

3. **Restart Docker containers:**
   ```bash
   docker compose down
   docker compose up airflow-init
   ```

4. **Create a new database:**
   - Use `pgAdmin` to create the database.
   - Identify the IP address of the PostgreSQL Docker container:
     ```bash
     docker container ls
     docker inspect <container_id> | grep IPAddress
     ```

5. **Add a connection in Airflow:**
   - Go to the Airflow UI and set up a new connection with the following details:
     - Connection Name: `book_connection`
     - Host Address: IP of PostgreSQL container
     - Port: `5432`
     - Database: The name of your PostgreSQL database

---

## Step 3: Create DAGs

### Designing the DAG
Airflow uses Directed Acyclic Graphs (DAGs) to represent workflows. Here's how to create a simple ETL pipeline DAG:

1. **Create a Python DAG script:** Save your DAG file in the `./dags` directory. Airflow will automatically detect changes.

2. **Define the workflow:**
   - Schedule interval (e.g., `@daily`).
   - Use `PythonOperator` to fetch and clean data.
   - Use `PostgresOperator` to interact with the database.

3. **Fetching Data:**
   - Use Python libraries such as `requests` and `BeautifulSoup` for web scraping.
   - Add necessary headers to requests to avoid bot detection.

4. **Cleaning Data:**
   - Use `pandas` to clean and process the data.
   - Pass cleaned data using Airflow’s XCom mechanism for inter-task communication.

5. **Inserting Data:**
   - Use Airflow’s `PostgresHook` to connect to the database and execute SQL queries.

## Conclusion
By following these steps, you’ve built a robust ETL pipeline using Airflow and PostgreSQL. This setup can be extended to include more complex workflows and integrations. Whether you're fetching data from APIs, cleaning it with Python, or storing it in a database, Airflow provides the tools to automate and scale your data pipeline effectively.

Feel free to experiment and enhance your workflows—the possibilities are endless!

Reference: https://github.com/sunjana2199/amazon_books_data_pipeline/tree/main
