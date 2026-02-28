# 🚀 My Java Project

A clean, well-structured Java application built with Java 17 and Maven. This project is designed to be easy to set up, straightforward to extend, and simple to contribute to — whether you're picking up Java for the first time or arriving from another ecosystem.

---

## Table of Contents

- [Prerequisites](#prerequisites)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Configuration](#configuration)
- [Running the Application](#running-the-application)
- [Testing](#testing)
- [Code Quality & Validation](#code-quality--validation)
- [Packaging & Distribution](#packaging--distribution)
- [Contributing](#contributing)
- [License](#license)

---

## Prerequisites

Before you begin, make sure you have the following installed:

| Tool | Minimum Version | Notes |
|------|----------------|-------|
| Java (JDK) | 17 | [Adoptium](https://adoptium.net/) or [Oracle JDK](https://www.oracle.com/java/technologies/downloads/) |
| Maven | 3.9+ | [Download](https://maven.apache.org/download.cgi) or use the wrapper (`./mvnw`) |
| Git | Any recent | For cloning and contributing |

Verify your setup with:

```bash
java -version     # should print: openjdk 17.x.x or similar
mvn -version      # should print: Apache Maven 3.x.x
```

> **Tip:** If you'd rather not install Maven globally, this project ships with the Maven Wrapper (`mvnw`). Just use `./mvnw` in place of `mvn` throughout this guide.

---

## Quick Start

Get up and running in under two minutes:

```bash
# 1. Clone the repository
git clone https://github.com/your-org/my-java-project.git
cd my-java-project

# 2. Build the project (downloads dependencies, compiles, runs tests)
mvn clean install

# 3. Run the application
mvn exec:java -Dexec.mainClass="com.example.Main"
```

That's it. If you hit any issues during setup, check the [Troubleshooting](#troubleshooting) section at the bottom of this file.

---

## Project Structure

```
my-java-project/
├── src/
│   ├── main/
│   │   ├── java/com/example/       # Application source code
│   │   │   ├── Main.java           # Entry point
│   │   │   ├── service/            # Business logic
│   │   │   └── util/               # Shared utilities
│   │   └── resources/
│   │       └── application.properties  # App configuration
│   └── test/
│       ├── java/com/example/       # Unit and integration tests
│       └── resources/              # Test fixtures and config
├── .github/
│   └── workflows/ci.yml            # CI pipeline (GitHub Actions)
├── checkstyle.xml                  # Code style rules
├── pom.xml                         # Maven build descriptor
└── README.md
```

Keeping this layout consistent makes it easy for any Java developer — and any build tool — to find their way around without guesswork.

---

## Configuration

Application behaviour is controlled through `src/main/resources/application.properties`:

```properties
# Application settings
app.name=My Java Project
app.version=1.0.0

# Logging level: TRACE, DEBUG, INFO, WARN, ERROR
app.log.level=INFO

# Example external service (replace with your own)
app.api.base-url=https://api.example.com
app.api.timeout-ms=5000
```

For local development, you can create a `application-local.properties` file (already in `.gitignore`) to override values without touching shared config.

---

## Running the Application

**During development** (direct Maven exec):

```bash
mvn exec:java -Dexec.mainClass="com.example.Main"
```

**With custom arguments:**

```bash
mvn exec:java -Dexec.mainClass="com.example.Main" -Dexec.args="--verbose --input data.csv"
```

**From a packaged JAR** (see [Packaging](#packaging--distribution)):

```bash
java -jar target/my-java-project-1.0.0.jar
```

---

## Testing

This project uses **JUnit 5** for unit tests and **Mockito** for mocking external dependencies.

**Run all tests:**

```bash
mvn test
```

**Run a specific test class:**

```bash
mvn test -Dtest=MyServiceTest
```

**Run a specific test method:**

```bash
mvn test -Dtest=MyServiceTest#shouldReturnExpectedResult
```

**Generate a test coverage report** (via JaCoCo):

```bash
mvn verify
# Open target/site/jacoco/index.html in your browser
```

A coverage report will appear at `target/site/jacoco/index.html`. The project enforces a minimum of **80% line coverage** — the build will fail if this threshold isn't met.

---

## Code Quality & Validation

Good code isn't just code that works — it's code that's readable, consistent, and maintainable. This project uses a small suite of automated checks to keep things tidy.

### Static Analysis — Checkstyle

Checkstyle enforces a consistent coding style (indentation, naming conventions, import ordering, etc.). The rules live in `checkstyle.xml` at the project root.

```bash
# Check for style violations
mvn checkstyle:check
```

If violations are found, the build fails and each one is printed with a file path and line number. Fix them manually or configure your IDE to apply the same rules automatically (IntelliJ IDEA and VS Code both support Checkstyle plugins).

### Bug Detection — SpotBugs

SpotBugs performs static bytecode analysis to catch common bugs — null pointer risks, resource leaks, improper synchronisation, and more.

```bash
mvn spotbugs:check
```

View the human-readable HTML report at `target/spotbugsXml.xml` or integrate the SpotBugs plugin into your IDE for inline warnings.

### Formatting — Spotless

Spotless handles mechanical formatting (trailing whitespace, blank lines, import sorting) so that code reviews stay focused on logic, not style:

```bash
# Check for formatting issues
mvn spotless:check

# Auto-fix formatting issues
mvn spotless:apply
```

Run `spotless:apply` before committing — it's non-destructive and idempotent.

### Run All Checks at Once

```bash
mvn clean verify
```

This single command compiles the project, runs all tests, enforces coverage thresholds, and runs all static analysis tools. It's the same command the CI pipeline runs on every pull request.

---

## Packaging & Distribution

To produce a standalone executable JAR that bundles all dependencies:

```bash
mvn clean package -DskipTests
```

Your JAR will appear at `target/my-java-project-1.0.0.jar`. Run it anywhere Java 17 is available:

```bash
java -jar target/my-java-project-1.0.0.jar
```

> **Skipping tests during packaging** (`-DskipTests`) is fine for release builds if your CI has already validated the test suite. Avoid skipping tests locally during development.

---

## Contributing

Contributions are welcome! Here's a quick guide to get your changes merged smoothly:

1. **Fork** the repository and create a feature branch from `main`:
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Write your code** — add or update tests as needed.

3. **Validate locally** before pushing:
   ```bash
   mvn clean verify
   ```
   The build must pass, all tests must be green, and no Checkstyle or SpotBugs violations should remain.

4. **Open a Pull Request** with a clear description of what changed and why.

Pull requests that break the build or reduce test coverage below the threshold will not be merged. This isn't gatekeeping — it's just keeping the codebase in a state everyone can depend on.

### Commit Message Style

Use short, present-tense summaries:

```
Add input validation to UserService
Fix null pointer in ReportGenerator
Refactor config loading for testability
```

---

## Troubleshooting

**`java: error: release version 17 not supported`**
Your `JAVA_HOME` is pointing to an older JDK. Run `java -version` and make sure you're on Java 17+. On macOS with Homebrew: `export JAVA_HOME=$(/usr/libexec/java_home -v 17)`.

**`BUILD FAILURE` with no obvious error**
Run with `-e` for a full stack trace: `mvn clean verify -e`.

**Tests pass locally but fail in CI**
Check for time-zone or locale dependencies in your tests. Use `ZoneOffset.UTC` explicitly and avoid `Locale.getDefault()` in production code paths.

**Checkstyle is failing on auto-generated code**
Add a `@SuppressWarnings("checkstyle:...")` annotation or use a `// CHECKSTYLE:OFF` block — but use these sparingly and always leave a comment explaining why.

---

## License

This project is licensed under the **MIT License**. See [LICENSE](./LICENSE) for the full text.

---

*Built with care. Questions, ideas, or bug reports? Open an issue — they're always welcome.*
