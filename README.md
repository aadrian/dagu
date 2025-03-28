<p align="center">
  <img src="./assets/images/dagu-logo.webp" width="960" alt="dagu-logo">
</p>

<p align="center">
  <a href="https://goreportcard.com/report/github.com/dagu-org/dagu">
    <img src="https://goreportcard.com/badge/github.com/dagu-org/dagu" />
  </a>
  <a href="https://codecov.io/gh/dagu-org/dagu">
    <img src="https://codecov.io/gh/dagu-org/dagu/branch/main/graph/badge.svg?token=CODZQP61J2" />
  </a>
  <a href="https://github.com/dagu-org/dagu/releases">
    <img src="https://img.shields.io/github/release/dagu-org/dagu.svg" />
  </a>
  <a href="https://godoc.org/github.com/dagu-org/dagu">
    <img src="https://godoc.org/github.com/dagu-org/dagu?status.svg" />
  </a>
  <img src="https://github.com/dagu-org/dagu/actions/workflows/ci.yaml/badge.svg" />
</p>

<div align="center">

[Installation](https://dagu.readthedocs.io/en/latest/installation.html) | [Community](https://discord.gg/gpahPUjGRk) | [Quick Start](https://dagu.readthedocs.io/en/latest/quickstart.html)

</div>

<h1><b>Dagu</b></h1>
Dagu is a compact, portable workflow engine implemented in Go. It provides a declarative model for orchestrating command execution across diverse environments, including shell scripts, Python commands, containerized operations, or remote commands.

Dagu’s design emphasizes minimal external dependencies: it operates solely as a single binary without requiring an external database. A browser-based graphical interface (UI) is provided for real-time monitoring, rendering the status and logs of workflows. This zero-dependency structure makes the system easy to install and well-suited to various infrastructures, including local or air-gapped systems. This local-first architecture also ensures that sensitive data or proprietary workflows remain secure.

<h2><b>Table of Contents</b></h2>

- [Key Attributes](#key-attributes)
- [Use Cases](#use-cases)
- [Community](#community)
- [Installation](#installation)
  - [Via Bash script](#via-bash-script)
  - [Via GitHub Releases Page](#via-github-releases-page)
  - [Via Homebrew (macOS)](#via-homebrew-macos)
  - [Via Docker](#via-docker)
  - [Quick Start](#quick-start)
- [Building from Source](#building-from-source)
  - [Prerequisites](#prerequisites)
  - [Steps to Build Locally](#steps-to-build-locally)
    - [1. Clone the repository](#1-clone-the-repository)
    - [2. Build the UI](#2-build-the-ui)
    - [3. Build the Binary](#3-build-the-binary)
  - [Run Locally from Source](#run-locally-from-source)
- [Quick Start Guide](#quick-start-guide)
  - [1. Launch the Web UI](#1-launch-the-web-ui)
  - [2. Create a New DAG](#2-create-a-new-dag)
  - [3. Edit the DAG](#3-edit-the-dag)
  - [4. Execute the DAG](#4-execute-the-dag)
- [Usage / Command Line Interface](#usage--command-line-interface)
- [Example DAG](#example-dag)
  - [Minimal Examples](#minimal-examples)
  - [Named Parameters](#named-parameters)
  - [Positional Parameters](#positional-parameters)
  - [Conditional DAG](#conditional-dag)
  - [Script Execution](#script-execution)
  - [Variable Passing](#variable-passing)
  - [Scheduling](#scheduling)
  - [Calling a sub-DAG](#calling-a-sub-dag)
  - [Running a docker image](#running-a-docker-image)
  - [Environment Variables](#environment-variables)
  - [Notifications on Failure or Success](#notifications-on-failure-or-success)
  - [HTTP Request and Notifications](#http-request-and-notifications)
  - [Execute commands over SSH](#execute-commands-over-ssh)
  - [Advanced Preconditions](#advanced-preconditions)
  - [Handling Various Execution Results](#handling-various-execution-results)
  - [JSON Processing Examples](#json-processing-examples)
- [Web UI](#web-ui)
  - [DAG Details](#dag-details)
  - [DAGs](#dags)
  - [Search](#search)
  - [Execution History](#execution-history)
  - [Log Viewer](#log-viewer)
- [Contributing](#contributing)
- [Contributors](#contributors)
- [License](#license)

## Key Attributes

- **Small Footprint**
Dagu is distributed as a single binary with minimal resource overhead. It does not require additional components such as external databases, message brokers, or other services.

- **Language Agnostic**: Workflows in Dagu are defined by specifying tasks (called “steps”) and their dependencies in YAML. A step can execute any command whether Python, Bash, Node.js, or other executables. This flexibility allows easy integration with existing scripts or tools.

- **Local-First Architecture**: Dagu was designed to run on a single developer workstation or server. By default, all tasks, logs, and scheduling run locally, allowing run offline or in air-gapped environments. This architecture ensures that sensitive data or proprietary workflows remain secure.

- **Declarative Configuration**: The workflow definition is contained in a YAML file. Dependencies, schedules, and execution details are declaratively expressed, making the workflow easy to comprehend and maintain.

- **No Complex Setup**: Unlike other orchestration platforms (e.g., `Airflow`) that often require substantial infrastructure, Dagu can be installed in minutes. Just `dagu start-all` command spins up both the scheduler and web UI, ready to run tasks immediately.

## Use Cases

- Data ingestion pipelines
- Data processing on small-scale/embedded systems
- Media file conversion tasks
- Automated workflows for employee onboarding and offboarding
- CI/CD automation

## Community

- Issues: [GitHub Issues](https://github.com/dagu-org/dagu/issues)
- Chat: [Discord](https://discord.gg/gpahPUjGRk)

## Installation

Dagu can be installed in multiple ways, such as using Homebrew or downloading a single binary from GitHub releases.

### Via Bash script

```sh
curl -L https://raw.githubusercontent.com/dagu-org/dagu/main/scripts/installer.sh | bash
```

### Via GitHub Releases Page

Download the latest binary from the [Releases page](https://github.com/dagu-org/dagu/releases) and place it in your `$PATH` (e.g. `/usr/local/bin`).

### Via Homebrew (macOS)

```sh
brew install dagu-org/brew/dagu
```

Upgrade to the latest version:

```sh
brew upgrade dagu-org/brew/dagu
```

### Via Docker

```sh
docker run \
--rm \
-p 8080:8080 \
-v ~/.config/dagu:/config \
-e DAGU_TZ=`ls -l /etc/localtime | awk -F'/zoneinfo/' '{print $2}'` \
ghcr.io/dagu-org/dagu:latest dagu start-all
```

Note: The environment variable `DAGU_TZ` is the timezone for the scheduler and server. You can set it to your local timezone (e.g. `America/New_York`).

See [Environment variables](https://dagu.readthedocs.io/en/latest/config.html#environment-variables) to configure those default directories.

### Quick Start

See the [Quick Start Guide](#quick-start-guide) to create and execute your first DAG!

## Building from Source

Dagu can be built and run locally from source.

### Prerequisites

Make sure you have the following installed on your system:

- [Go 1.23 or later](https://go.dev/doc/install)
- [Node.js (Latest LTS or Current)](https://nodejs.org/en/download/)
- [Yarn](https://yarnpkg.com/)

### Steps to Build Locally

#### 1. Clone the repository

- Clone the repository to your local machine using Git.
  ```sh
  git clone https://github.com/dagu-org/dagu.git
  cd dagu
  ```

#### 2. Build the UI
- Navigate to the `ui` directory.
  ```sh
  cd ui
  ```
- Install / Update Dependencies
  ```sh
  yarn
  ```
- Go back to the root directory.
  ```sh
  cd ..
  ```
- Build the UI
  ```sh
  make build-ui
  ```

#### 3. Build the Binary
- Build the binary
  ```sh
  make build-bin
  ```
  This produces the `dagu` binary in the `.local/bin` directory.

### Run Locally from Source

For a quick test of both server, scheduler, and UI:
```sh
# Runs "dagu start-all" with the `go run` command
make run
```
Once the server is running, visit `http://127.0.0.1:8080` to see the Web UI.

Continue with the [Quick Start Guide](#quick-start-guide) to create and execute your first DAG!

## Quick Start Guide

### 1. Launch the Web UI

Start the server and scheduler with the command `dagu start-all` and browse to `http://127.0.0.1:8080` to explore the Web UI.

### 2. Create a New DAG

Navigate to the DAG List page by clicking the menu in the left panel of the Web UI. Then create a DAG by clicking the `NEW` button at the top of the page. Enter `example` in the dialog.

_Note: DAG (YAML) files will be placed in `~/.config/dagu/dags` by default. See [Configuration Options](https://dagu.readthedocs.io/en/latest/config.html) for more details._

### 3. Edit the DAG

Go to the `SPEC` Tab and hit the `Edit` button. Copy & Paste the following example and click the `Save` button.

Example:

```yaml
schedule: "* * * * *" # Run the DAG every minute
params:
  - NAME: "Dagu"
steps:
  - name: Hello world
    command: echo Hello $NAME

  - name: Simulate unclean Command Output
    command: |
      cat <<EOF
      INFO: Starting process...
      DEBUG: Initializing variables...
      DATA: User count is 42
      INFO: Process completed successfully.
      EOF
    output: raw_output

  - name: Extract Relevant Data
    command: |
      echo "$raw_output" | grep '^DATA:' | sed 's/^DATA: //'
    output: cleaned_data
    depends:
      - Simulate unclean Command Output

  - name: Done
    command: echo Done!
    depends:
      - Hello world
```

### 4. Execute the DAG

You can execute the example by pressing the `Start` button. You can see "Hello Dagu" in the log page in the Web UI.

## Usage / Command Line Interface

```sh
# Runs the DAG
dagu start <file or DAG name>

# Runs the DAG with named parameters
dagu start <file or DAG name> [-- <key>=<value> ...]

# Runs the DAG with positional parameters
dagu start <file or DAG name> [-- value1 value2 ...]

# Displays the current status of the DAG
dagu status <file or DAG name>

# Re-runs the specified DAG run
dagu retry --request-id=<request-id> <file or DAG name>

# Stops the DAG execution
dagu stop <file or DAG name>

# Restarts the current running DAG
dagu restart <file or DAG name>

# Dry-runs the DAG
dagu dry <file or DAG name> [-- <key>=<value> ...]

# Launches both the web UI server and scheduler process
dagu start-all [--host=<host>] [--port=<port>] [--dags=<path to directory>]

# Launches the Dagu web UI server
dagu server [--host=<host>] [--port=<port>] [--dags=<path to directory>]

# Starts the scheduler process
dagu scheduler [--dags=<path to directory>]

# Shows the current binary version
dagu version
```

## Example DAG

### Minimal Examples

A simple example with a named parameter:

```yaml
params:
  - NAME: "Dagu"

steps:
  - name: Hello world
    command: echo Hello $NAME
  - name: Done
    command: echo Done!
    depends:
      - Hello world
```

Using a pipe:

```yaml
steps:
  - name: step 1
    command: echo hello world | xargs echo
```

Specifying a shell:

```yaml
steps:
  - name: step 1
    command: echo hello world | xargs echo
    shell: bash # The default shell is `$SHELL` or `sh`.
```

### Named Parameters

You can define named parameters in the DAG file and override them when running the DAG.

```yaml
# Default named parameters
params:
  NAME: "Dagu"
  AGE: 30

steps:
  - name: Hello world
    command: echo Hello $NAME
  - name: Done
    command: echo Done!
    depends: Hello world
```

Run the DAG with custom parameters:

```sh
dagu start my_dag -- NAME=John AGE=40
```

### Positional Parameters

You can define positional parameters in the DAG file and override them when running the DAG.

```yaml
# Default positional parameters
params: input.csv output.csv 60 # Default values for $1, $2, and $3

steps:
  # Using positional parameters
  - name: Installation
    command: pipx install pandas --include-deps

  - name: Data processing
    command: pipx run
    script: |
      import sys
      import pandas as pd

      input_file = "$1"    # First parameter
      output_file = "$2"   # Second parameter
      timeout = "$3"       # Third parameter

      print(f"Processing {input_file} -> {output_file} with timeout {timeout}s")
      # Add your processing logic here
    depends:
      - Installation
```

Run the DAG with custom parameters:

```sh
dagu start my_dag -- input.csv output.csv 120
```

### Conditional DAG

You can define conditions to run a step based on the output of a command.

```yaml
steps:
  - name: monthly task
    command: monthly.sh
    preconditions:
      - condition: "`date '+%d'`"
        expected: "re:0[1-9]" # Run only if the day is between 01 and 09
```

### Script Execution

You can run a script using the `script` field.

```yaml
steps:
  # Python script example
  - name: data analysis
    command: python
    script: |
      import json
      import sys
      
      data = {'count': 100, 'status': 'ok'}
      print(json.dumps(data))
      sys.stderr.write('Processing complete\n')
    output: RESULT
    stdout: /tmp/analysis.log
    stderr: /tmp/analysis.error

  # Shell script with multiple commands
  - name: cleanup
    command: bash
    script: |
      #!/bin/bash
      echo "Starting cleanup..."
      
      # Remove old files
      find /tmp -name "*.tmp" -mtime +7 -exec rm {} \;
      
      # Archive logs
      cd /var/log
      tar -czf archive.tar.gz *.log
      
      echo "Cleanup complete"
    depends: data analysis
```

### Variable Passing

You can pass the output of one step to another step using the `output` field.

```yaml
steps:
  # Basic output capture
  - name: generate id
    command: echo "ABC123"
    output: REQUEST_ID

  - name: use id
    command: echo "Processing request ${REQUEST_ID}"
    depends: generate id

# Capture JSON output
steps:
  - name: get config
    command: |
      echo '{"port": 8080, "host": "localhost"}'
    output: CONFIG

  - name: start server
    command: echo "Starting server at ${CONFIG.host}:${CONFIG.port}"
    depends: get config
```

### Scheduling

You can specify flexible schedules using the cron format.

```yaml
schedule: "5 4 * * *" # Run at 04:05.

steps:
  - name: scheduled job
    command: job.sh
```

Or you can set multiple schedules.

```yaml
schedule:
  - "30 7 * * *" # Run at 7:30
  - "0 20 * * *" # Also run at 20:00

steps:
  - name: scheduled job
    command: job.sh
```

If you want to start and stop a long-running process on a fixed schedule, you can define ``start`` and ``stop`` times:

```yaml
schedule:
  start: "0 8 * * *" # starts at 8:00
  stop: "0 13 * * *" # stops at 13:00
steps:
  - name: scheduled job
    command: job.sh
```

### Calling a sub-DAG

You can call another DAG from a parent DAG.

```yaml
steps:
  - name: parent
    run: sub-dag
    output: OUT
  - name: use output
    command: echo ${OUT.outputs.result}
    depends: parent
```

The sub-DAG `sub-dag.yaml`:

```yaml
steps:
  - name: sub-dag
    command: echo "Hello from sub-dag"
    output: result
```

THe parent DAG will call the sub-DAG and write the output to the log (stdout).
The output will be `Hello from sub-dag`.

### Running a docker image

You can run a docker image as a step:

```yaml
steps:
  - name: hello
    executor:
      type: docker
      config:
        image: alpine
        autoRemove: true
    command: echo "hello"
```

### Environment Variables

You can define environment variables and use them in the DAG.

```yaml
env:
  - DATA_DIR: ${HOME}/data
  - PROCESS_DATE: "`date '+%Y-%m-%d'`"

steps:
  - name: process logs
    command: python process.py
    dir: ${DATA_DIR}
    preconditions:
      - "test -f ${DATA_DIR}/logs_${PROCESS_DATE}.txt" # Check if the file exists
```

### Notifications on Failure or Success

You can send notifications on failure in various ways.

```yaml
env:
  - SLACK_WEBHOOK_URL: "https://hooks.slack.com/services/XXXXX/YYYYY/ZZZZZ"

dotenv:
  - .env

smtp:
  host: $SMTP_HOST
  port: "587"
  username: $SMTP_USERNAME
  password: $SMTP_PASSWORD

handlerOn:
  failure:
    command: |
      curl -X POST -H 'Content-type: application/json' \
      --data '{ "text": "DAG Failure ('${DAG_NAME}')" }' \
      ${SLACK_WEBHOOK_URL} 

steps:
  - name: critical process
    command: important_job.sh
    retryPolicy:
      limit: 3
      intervalSec: 60
    mailOn:
      failure: true # Send an email on failure
```

If you want to set it globally, you can create `~/.config/dagu/base.yaml` and define the common configurations across all DAGs.

```yaml
smtp:
  host: $SMTP_HOST
  port: "587"
  username: $SMTP_USERNAME
  password: $SMTP_PASSWORD

mailOn:
  failure: true                      
  success: true                      
```

You can also use mail executor to send notifications.

```yaml
params:
  - RECIPIENT_NAME: XXX
  - RECIPIENT_EMAIL: example@company.com
  - MESSAGE: "Hello [RECIPIENT_NAME]"

steps:
  - name: step1
    executor:
      type: mail
      config:
        to: $RECIPIENT_EMAIL
        from: dagu@dagu.com
        subject: "Hello [RECIPIENT_NAME]"
        message: $MESSAGE
          
```

### HTTP Request and Notifications

You can make HTTP requests and send notifications.

```yaml
dotenv:
  - .env

smtp:
  host: $SMTP_HOST
  port: "587"
  username: $SMTP_USERNAME
  password: $SMTP_PASSWORD

steps:
  - name: fetch data
    executor:
      type: http
      config:
        timeout: 10
    command: GET https://api.example.com/data
    output: API_RESPONSE

  - name: send notification
    executor:
      type: mail
      config:
        to: team@company.com
        from: team@company.com
        subject: "Data Processing Complete"
        message: |
          Process completed successfully.
          Response: ${API_RESPONSE}

    depends: fetch data
```

### Execute commands over SSH

You can execute commands over SSH.

```yaml
steps:
  - name: backup
    executor:
      type: ssh
      config:
        user: admin
        ip: 192.168.1.100
        key: ~/.ssh/id_rsa
    command: tar -czf /backup/data.tar.gz /data
```

### Advanced Preconditions

You can define complex conditions to run a step based on the output of a command.

```yaml
steps:
  # Check multiple conditions
  - name: daily task
    command: process_data.sh
    preconditions:
      # Run only on weekdays
      - condition: "`date '+%u'`"
        expected: "re:[1-5]"
      # Run only if disk space > 20%
      - condition: "`df -h / | awk 'NR==2 {print $5}' | sed 's/%//'`"
        expected: "re:^[0-7][0-9]$|^[1-9]$"  # 0-79% used (meaning at least 20% free)
      # Check if input file exists
      - condition: "test -f input.csv"

  # Complex file check
  - name: process files
    command: batch_process.sh
    preconditions:
      - condition: "`find data/ -name '*.csv' | wc -l`"
        expected: "re:[1-9][0-9]*"  # At least one CSV file exists
```

### Handling Various Execution Results

You can use `continueOn` to control when to fail or continue based on the exit code, output, or other conditions.

```yaml
steps:
  # Basic error handling
  - name: process data
    command: python process.py
    continueOn:
      failure: true  # Continue on any failure
      skipped: true  # Continue if preconditions aren't met

  # Handle specific exit codes
  - name: data validation
    command: validate.sh
    continueOn:
      exitCode: [1, 2, 3]  # 1:No data, 2:Partial data, 3:Invalid format
      markSuccess: true    # Mark as success even with these codes

  # Output pattern matching
  - name: api request
    command: curl -s https://api.example.com/data
    continueOn:
      output:
        - "no records found"      # Exact match
        - "re:^Error: [45][0-9]"  # Regex match for HTTP errors
        - "rate limit exceeded"    # Another exact match

  # Complex pattern
  - name: database backup
    command: pg_dump database > backup.sql
    continueOn:
      exitCode: [0, 1]     # Accept specific exit codes
      output:              # Accept specific outputs
        - "re:0 rows affected"
        - "already exists"
      failure: false       # Don't continue on other failures
      markSuccess: true    # Mark as success if conditions match

  # Multiple conditions combined
  - name: data sync
    command: sync_data.sh
    continueOn:
      exitCode: [1]        # Exit code 1 is acceptable
      output:              # These outputs are acceptable
        - "no changes detected"
        - "re:synchronized [0-9]+ files"
      skipped: true       # OK if skipped due to preconditions
      markSuccess: true   # Mark as success in these cases

  # Error output handling
  - name: log processing
    command: process_logs.sh
    stderr: /tmp/process.err
    continueOn:
      output: 
        - "re:WARNING:.*"   # Continue on warnings
        - "no logs found"   # Continue if no logs
      exitCode: [0, 1, 2]   # Multiple acceptable exit codes
      failure: true         # Continue on other failures too

  # Application-specific status
  - name: app health check
    command: check_status.sh
    continueOn:
      output:
        - "re:STATUS:(DEGRADED|MAINTENANCE)"  # Accept specific statuses
        - "re:PERF:[0-9]{2,3}ms"             # Accept performance in range
      markSuccess: true                       # Mark these as success
```

### JSON Processing Examples

You can use `jq` executor to process JSON data.

```yaml
# Simple data extraction
steps:
  - name: extract value
    executor: jq
    command: .user.name    # Get user name from JSON
    script: |
      {
        "user": {
          "name": "John",
          "age": 30
        }
      }

# Output: "John"

# Transform array data
steps:
  - name: get users
    executor: jq
    command: '.users[] | {name: .name}'    # Extract name from each user
    script: |
      {
        "users": [
          {"name": "Alice", "age": 25},
          {"name": "Bob", "age": 30}
        ]
      }

# Output:
# {"name": "Alice"}
# {"name": "Bob"}

# Calculate and format
steps:
  - name: sum ages
    executor: jq
    command: '{total_age: ([.users[].age] | add)}'    # Sum all ages
    script: |
      {
        "users": [
          {"name": "Alice", "age": 25},
          {"name": "Bob", "age": 30}
        ]
      }

# Output: {"total_age": 55}

# Filter and count
steps:
  - name: count active
    executor: jq
    command: '[.users[] | select(.active == true)] | length'
    script: |
      {
        "users": [
          {"name": "Alice", "active": true},
          {"name": "Bob", "active": false},
          {"name": "Charlie", "active": true}
        ]
      }

# Output: 2
```

More examples can be found in the [documentation](https://dagu.readthedocs.io/en/latest/yaml_format.html).

## Web UI

### DAG Details

Real-time status, logs, and configuration for each DAG. Toggle graph orientation from the top-right corner.

![example](assets/images/demo.gif?raw=true)

![Details-TD](assets/images/ui-details2.webp?raw=true)

### DAGs

View all DAGs in one place with live status updates.

![DAGs](assets/images/ui-dags.webp?raw=true)

### Search

Search across all DAG definitions.

![History](assets/images/ui-search.webp?raw=true)

### Execution History

Review past DAG executions and logs at a glance.

![History](assets/images/ui-history.webp?raw=true)

### Log Viewer

Examine detailed step-level logs and outputs.

![DAG Log](assets/images/ui-logoutput.webp?raw=true)

## Contributing

Contributions to Dagu are welcome. Refer to the [Contribution Guide](https://dagu.readthedocs.io/en/latest/contrib.html) for details on how to get started.

## Contributors

<a href="https://github.com/dagu-org/dagu/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=dagu-org/dagu" />
</a>

## License

Dagu is distributed under the [GNU GPLv3](./LICENSE.md).
