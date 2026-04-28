[![Java CI with Gradle](https://github.com/lucformalmethodscourse/lifoqueue-jqwik-maven/actions/workflows/gradle.yml/badge.svg)](https://github.com/lucformalmethodscourse/lifoqueue-jqwik-maven/actions/workflows/gradle.yml)

## Stateful unit testing using Jqwik actions

This project provides an example of stateful unit testing with and without [jqwik](https://jqwik.net/).

## Build System

This project has been migrated to Gradle while maintaining Maven compatibility. See [doc/GRADLE-MIGRATION.md](doc/GRADLE-MIGRATION.md) for complete migration details and command reference.

### Quick Start with Gradle

```bash
# Run tests (including jqwik property-based tests)
./gradlew test

# Run tests with coverage verification
./gradlew check
```

## References

- [java.util.Queue interface](https://docs.oracle.com/en/java/javase/25/docs/api/java.base/java/util/Queue.html)
