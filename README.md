# NexusFlow: A Python-based Data Pipeline Orchestration Framework

NexusFlow is a lightweight, yet powerful Python framework designed to simplify the creation, scheduling, and monitoring of complex data pipelines. It provides a declarative approach to pipeline definition, allowing developers to focus on the logic of their data transformations rather than the intricacies of scheduling and dependency management. NexusFlow aims to bridge the gap between simple scripting and heavyweight orchestration tools, offering a scalable and maintainable solution for data processing tasks of varying complexity.

NexusFlow empowers data scientists and engineers to build robust data pipelines with minimal overhead. It excels in scenarios requiring scheduled data extraction, transformation, and loading (ETL) processes. The framework provides a flexible mechanism to define dependencies between tasks, ensuring that data flows correctly and consistently. Its modular design allows for easy extension and integration with existing data infrastructure, making it a versatile tool for a wide range of data processing needs. By providing built-in logging and monitoring capabilities, NexusFlow simplifies the debugging and maintenance of complex pipelines.

This framework allows for easier pipeline management as it decouples the definition of the pipeline tasks from the execution engine. Pipelines are defined using Python code, resulting in increased readability and maintainability. The ability to define tasks as self-contained units with input and output dependencies allows for the easy reuse of components across multiple pipelines. Furthermore, NexusFlow incorporates a modular architecture, allowing for the easy addition of new task types and execution engines to meet the evolving needs of the data environment.

## Key Features

*   **Declarative Pipeline Definition:** Define pipelines using Python code, specifying tasks and their dependencies in a clear and concise manner.
    *   Technical Detail: Pipelines are defined as directed acyclic graphs (DAGs) where nodes represent tasks and edges represent dependencies. The `NexusFlow.Pipeline` class provides an interface for constructing these DAGs.

*   **Task Dependency Management:** Automatically manage task execution order based on defined dependencies.
    *   Technical Detail: NexusFlow uses a topological sort algorithm to determine the execution order of tasks within a pipeline. Tasks with no dependencies are executed first, and subsequent tasks are executed only after their dependencies have been successfully completed.

*   **Flexible Task Scheduling:** Schedule pipelines to run automatically at specific intervals using cron-like expressions.
    *   Technical Detail: NexusFlow integrates with scheduling libraries such as `APScheduler` to provide flexible scheduling options. Users can specify schedules using standard cron expressions, allowing for precise control over pipeline execution timing.

*   **Built-in Logging and Monitoring:** Track pipeline execution status and log detailed information about each task.
    *   Technical Detail: NexusFlow utilizes the Python `logging` module to provide detailed logging of pipeline execution. Logs include timestamps, task names, status updates, and any error messages. These logs can be configured to output to various destinations, such as files or databases.

*   **Extensible Task Types:** Support for various task types, including Python functions, shell commands, and database queries.
    *   Technical Detail: NexusFlow defines an abstract `Task` class that serves as the base for all task types. Developers can extend this class to create custom task types that execute specific logic. Built-in task types include `PythonTask` for executing Python functions and `ShellTask` for executing shell commands.

*   **Modular Architecture:** Easily extend the framework with custom task types and execution engines.
    *   Technical Detail: The modular architecture of NexusFlow allows for the easy addition of new task types and execution engines. Task types are defined as classes that inherit from the `Task` base class. Execution engines are responsible for executing tasks and managing dependencies.

## Technology Stack

*   **Python 3.7+:** The core language for the framework. Python provides a rich ecosystem of libraries for data processing and task scheduling.
*   **APScheduler:** Used for scheduling pipeline executions. It offers a flexible and powerful scheduling mechanism.
*   **NetworkX:** Utilized for representing and manipulating the pipeline as a directed acyclic graph (DAG). This library provides efficient algorithms for graph traversal and dependency management.
*   **Logging Module:** Python's built-in logging module provides a standardized way to record events during pipeline execution, facilitating debugging and monitoring.
*   **(Optional) Database Connector (e.g., psycopg2, SQLAlchemy):** For interacting with databases within tasks.

## Installation

1.  **Clone the repository:**
    `git clone https://github.com/jjfhwang/NexusFlow.git`

2.  **Navigate to the project directory:**
    `cd NexusFlow`

3.  **Create a virtual environment (recommended):**
    `python3 -m venv venv`

4.  **Activate the virtual environment:**
    *   On Linux/macOS: `source venv/bin/activate`
    *   On Windows: `venv\Scripts\activate`

5.  **Install the required dependencies:**
    `pip install -r requirements.txt`

## Configuration

NexusFlow uses environment variables for configuration. The following environment variables are supported:

*   `NEXUSFLOW_LOG_LEVEL`: Sets the logging level (e.g., DEBUG, INFO, WARNING, ERROR). Defaults to `INFO`.
*   `NEXUSFLOW_LOG_FILE`: Specifies the path to the log file. If not set, logs are printed to the console.
*   `NEXUSFLOW_DATABASE_URL`: (Optional) Connection string for the database used for storing pipeline metadata.

To set these environment variables, you can use the `export` command on Linux/macOS or the `set` command on Windows. For example:

export NEXUSFLOW_LOG_LEVEL=DEBUG
set NEXUSFLOW_LOG_LEVEL=DEBUG

## Usage

Here's an example of defining and running a simple pipeline:

from NexusFlow import Pipeline, PythonTask

def my_task_function(input_data):
    # Perform some data transformation here
    processed_data = input_data * 2
    return processed_data

task1 = PythonTask(name="Task 1", function=my_task_function, input_data=5)
task2 = PythonTask(name="Task 2", function=my_task_function, input_data=task1.output)

pipeline = Pipeline(name="My Pipeline")
pipeline.add_task(task1)
pipeline.add_task(task2)

pipeline.run()

This code defines a pipeline with two tasks, both executing the same `my_task_function`. The output of `task1` is used as the input for `task2`. The `pipeline.run()` method executes the pipeline, ensuring that tasks are executed in the correct order based on their dependencies.

Detailed API documentation for the `Pipeline` and `Task` classes is available in the `docs/` directory of the repository.

## Contributing

We welcome contributions to NexusFlow! To contribute, please follow these guidelines:

1.  Fork the repository.
2.  Create a new branch for your feature or bug fix.
3.  Implement your changes and write unit tests.
4.  Submit a pull request with a clear description of your changes.

## License

This project is licensed under the MIT License. See the [LICENSE](https://github.com/jjfhwang/NexusFlow/blob/main/LICENSE) file for details.

## Acknowledgements

We would like to thank the developers of APScheduler and NetworkX for their excellent libraries, which are essential components of NexusFlow.