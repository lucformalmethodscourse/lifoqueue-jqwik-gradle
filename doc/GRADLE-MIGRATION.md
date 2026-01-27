# Gradle Migration Documentation

## Overview

This project has been migrated from Apache Maven to Gradle, utilizing the current stable release of Gradle (9.3.0). The migration maintains all functionality while preserving the original testing-focused structure of this project.

## Version Information

- **Gradle Version**: 9.3.0 (current stable release as of January 2026)
- **Java Version**: 21 (using Java Toolchain)
- **Build Tool**: Gradle with Wrapper (platform-independent)

## Migration Details

### Build Files Created

1. **build.gradle.kts** - Main build configuration file (replaces pom.xml)
2. **settings.gradle.kts** - Project settings and name configuration
3. **gradlew / gradlew.bat** - Gradle wrapper scripts for Unix/Windows
4. **gradle/wrapper/** - Gradle wrapper JAR and properties

### Dependencies Migrated

All Maven dependencies have been successfully migrated to Gradle format:

| Dependency | Version | Scope | Notes |
|------------|---------|-------|-------|
| junit-jupiter-api | 5.11.4 | test | JUnit 5 API for writing tests |
| junit-jupiter-engine | 5.11.4 | testRuntime | JUnit 5 test execution engine |
| junit-platform-launcher | 1.11.4 | testRuntime | Required for Gradle 9.x JUnit integration |
| jqwik | 1.9.2 | test | Property-based testing framework |

### Plugins Configured

1. **java** - Core Java compilation and build support
2. **jacoco** - Code coverage reporting (version 0.8.12)

### Build Configuration

#### Java Toolchain
The project uses Gradle's Java Toolchain feature to ensure Java 21 is used consistently:
```gradle
java {
    toolchain {
        languageVersion = JavaLanguageVersion.of(21)
    }
}
```

#### JUnit Platform Configuration
The test task is configured to use both JUnit Jupiter and jqwik test engines:
```gradle
tasks.test {
    useJUnitPlatform {
        includeEngines("jqwik", "junit-jupiter")
    }
    finalizedBy(tasks.jacocoTestReport)
}
```

#### JaCoCo Coverage
Code coverage is configured to:
- Run automatically after tests
- Exclude `Main.java` from coverage metrics (pedagogical reasons)
- Generate both XML and HTML reports
- Enforce coverage thresholds matching the Maven configuration:
  - INSTRUCTION: 90%
  - METHOD: 100%
  - BRANCH: 90%
  - COMPLEXITY: 90%
  - LINE: 90%
  - CLASS: 100%

## Command Reference

### Maven vs Gradle Command Mapping

| Maven Command | Gradle Equivalent | Description |
|---------------|-------------------|-------------|
| `mvn clean` | `./gradlew clean` | Clean build artifacts |
| `mvn compile` | `./gradlew compileJava` | Compile source code |
| `mvn test` | `./gradlew test` | Run tests (including jqwik) |
| `mvn package` | `./gradlew build` | Build project |
| `mvn verify` | `./gradlew check` | Run verification tasks (includes coverage check) |

### Common Gradle Commands

#### Build Commands
```bash
# Clean and build the project
./gradlew clean build

# Build without tests
./gradlew build -x test
```

#### Testing Commands
```bash
# Run all tests (JUnit and jqwik)
./gradlew test

# Run tests with coverage report
./gradlew test jacocoTestReport

# Run tests with coverage verification
./gradlew check

# Run specific test class
./gradlew test --tests TestLifoQueue

# Run tests continuously (on file changes)
./gradlew test --continuous
```

#### Dependency Commands
```bash
# View dependency tree
./gradlew dependencies

# View test dependencies only
./gradlew dependencies --configuration testRuntimeClasspath

# Check for dependency updates
./gradlew dependencyUpdates
```

#### Information Commands
```bash
# List all tasks
./gradlew tasks

# List all tasks including detailed descriptions
./gradlew tasks --all

# View project properties
./gradlew properties
```

## Project Structure

The source directory structure remains unchanged from Maven:

```
lifoqueue-jqwik-maven/
├── build.gradle.kts          # Gradle build configuration
├── settings.gradle.kts        # Gradle project settings
├── gradlew                    # Unix Gradle wrapper script
├── gradlew.bat               # Windows Gradle wrapper script
├── gradle/
│   └── wrapper/
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── src/
│   └── test/
│       └── java/
│           └── edu/
│               └── luc/
│                   └── cs271/
│                       └── arrayqueue/
│                           ├── TestLifoQueue.java
│                           └── TestLifoQueueJqwik.java
└── build/                    # Gradle build output (gitignored)
    ├── classes/
    ├── reports/              # Test and coverage reports
    └── test-results/
```

## Build Output Locations

| Artifact Type | Maven Location | Gradle Location |
|---------------|----------------|-----------------|
| Compiled test classes | `target/test-classes/` | `build/classes/java/test/` |
| Test reports | `target/surefire-reports/` | `build/reports/tests/test/` |
| Coverage reports | `target/site/jacoco/` | `build/reports/jacoco/test/` |

## Advanced Features

### Configuration Cache

Gradle 9.x supports configuration cache for faster builds. Enable it by adding to `gradle.properties`:
```properties
org.gradle.configuration-cache=true
```

### Build Scans

Generate detailed build analysis:
```bash
./gradlew build --scan
```

### Parallel Execution

Enable parallel test execution in `build.gradle.kts`:
```gradle
test {
    maxParallelForks = Runtime.runtime.availableProcessors().intdiv(2) ?: 1
}
```

## Migration Benefits

1. **Faster Builds**: Gradle's incremental compilation and build cache
2. **Better Dependency Management**: Transitive dependency resolution
3. **Flexible Scripting**: Kotlin DSL for complex build logic
4. **Rich Plugin Ecosystem**: Easy integration of new build tools
5. **Modern Tooling**: Active development and Java 21+ support
6. **jqwik Integration**: Native support for property-based testing

## Troubleshooting

### Gradle Daemon Issues
```bash
# Stop all Gradle daemons
./gradlew --stop

# Check daemon status
./gradlew --status
```

### Clean Build
```bash
# Force clean build
./gradlew clean build --no-build-cache
```

### Wrapper Update
```bash
# Update to latest Gradle version
./gradlew wrapper --gradle-version=9.3.0
```

### Dependency Conflicts
```bash
# View dependency insight
./gradlew dependencyInsight --dependency junit-jupiter
```

### jqwik Tests Not Running
If jqwik tests aren't being discovered, verify that:
1. The test task includes the jqwik engine: `includeEngines("jqwik", "junit-jupiter")`
2. Test classes are annotated with `@Property` or other jqwik annotations
3. The jqwik dependency is in the testImplementation configuration

## Compatibility Notes

- **Java 21**: Required by project configuration
- **Gradle 9.3.0**: Uses standard Java and JaCoCo plugins
- **JUnit 5**: Platform launcher explicitly included for Gradle 9.x
- **jqwik 1.9.2**: Full property-based testing support
- **IDE Support**: Works with IntelliJ IDEA, Eclipse, VS Code with Gradle extensions

## References

- [Gradle Documentation](https://docs.gradle.org/9.3.0/userguide/userguide.html)
- [Gradle Java Plugin](https://docs.gradle.org/current/userguide/java_plugin.html)
- [JUnit 5 User Guide](https://junit.org/junit5/docs/current/user-guide/)
- [jqwik Documentation](https://jqwik.net/docs/current/user-guide.html)
