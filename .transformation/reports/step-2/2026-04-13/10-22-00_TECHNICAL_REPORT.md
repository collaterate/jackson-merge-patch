# Java Upgrade Technical Report

**Project**: Jackson Merge Patch  
**Migration**: Step 2.1-2.2: Java 8 to Java 21 Upgrade  
**Date**: 2026-04-13T10:22:00-06:00  
**Status**: ✅ SUCCESS

---

## Upgrade Summary

### Files Modified
**Modified Files:** 3
- `pom.xml` — Compiler properties (source/target → release 21), dependency version updates, toolchain profile enabled, Mockito/ByteBuddy overrides added
- `jackson-merge-patch-core/pom.xml` — Parent version 2.0.0 → 3.0.0
- `jackson-merge-patch-validations/pom.xml` — Parent version 2.0.0 → 3.0.0, hibernate-validator 5.3.6.Final → 6.2.5.Final, validation-api 1.1.0.Final → 2.0.1.Final

### Configuration Changes
- Compiler configuration migrated from `<source>1.8</source>/<target>1.8</target>` to `<release>21</release>`
- Toolchain profile uncommented and activated with JDK version range `[21,22)` vendor `amazon`
- Toolchain resolves to Amazon Corretto 21.0.8 at `/Users/francisco.acevedo/.sdkman/candidates/java/21.0.8-amzn`
- JaCoCo plugin updated 0.8.10 → 0.8.12 for Java 21 bytecode support
- Project version updated 2.0.0 → 3.0.0 across all POMs

### Dependency Updates
- `jackson-databind`: 2.8.11 → 2.15.4 (Java 21 module system compatibility)
- `spring-boot`: 1.5.4.RELEASE → 1.5.22.RELEASE (latest 1.5.x patch release)
- `mockito-core`: 1.10.19 → 4.11.0 (Java 21 ByteBuddy compatibility — learned lesson applied)
- `byte-buddy`: added 1.12.23 (Java 21 class definition support — learned lesson applied)
- `byte-buddy-agent`: added 1.12.23 (Java 21 instrumentation support — learned lesson applied)
- `hibernate-validator`: 5.3.6.Final → 6.2.5.Final (Java 21 reflection compatibility)
- `validation-api`: 1.1.0.Final → 2.0.1.Final (required by hibernate-validator 6.2.5.Final)
- Spring Boot 1.5.x preserved — no Spring Boot major version change

### Code Changes Summary
- Zero source code modifications required
- No import changes, no API replacements, no deprecated code fixes
- All javax.validation imports remain unchanged (provided by explicit validation-api dependency, not JDK)
- Lombok annotation processing works identically on Java 21

---

## Compatibility Analysis

- **Total Dependencies Analyzed**: 11
- **Compatible As-Is**: 4
- **Updated for Java 21**: 7
- **Compatibility Issues**: 0

### Dependency Compatibility Analysis

| Dependency | Java 8 Version | Java 21 Version | Status | Notes |
|------------|-----------------|------------------|:------:|-------|
| **Core Dependencies** | | | | |
| jackson-databind | 2.8.11 | 2.15.4 | ℹ️ | Updated for Java 21 module system |
| lombok | 1.18.38 | 1.18.38 | ✅ | Compatible as-is |
| **Test Dependencies** | | | | |
| spring-boot-starter-test | 1.5.4.RELEASE | 1.5.22.RELEASE | ℹ️ | Latest 1.5.x patch |
| spring-boot-starter-web | 1.5.4.RELEASE | 1.5.22.RELEASE | ℹ️ | Latest 1.5.x patch |
| guava | 28.0-jre | 28.0-jre | ✅ | Compatible as-is |
| commons-io | 2.6 | 2.6 | ✅ | Compatible as-is |
| mockito-core | 1.10.19 | 4.11.0 | ℹ️ | Java 21 ByteBuddy compatibility (learned lesson) |
| byte-buddy | (transitive) | 1.12.23 | ℹ️ | Added override for Java 21 class definition |
| byte-buddy-agent | (transitive) | 1.12.23 | ℹ️ | Added override for Java 21 instrumentation |
| **Validations Module** | | | | |
| commons-lang3 | 3.5 | 3.5 | ✅ | Compatible as-is |
| validation-api | 1.1.0.Final | 2.0.1.Final | ℹ️ | Required by hibernate-validator 6.2.5 |
| hibernate-validator | 5.3.6.Final | 6.2.5.Final | ℹ️ | Java 21 reflection compatibility |
| **Maven Plugins** | | | | |
| jacoco-maven-plugin | 0.8.10 | 0.8.12 | ℹ️ | Java 21 bytecode support |
| maven-compiler-plugin | 3.11.0 | 3.11.0 | ✅ | Compatible as-is |
| maven-surefire-plugin | 3.1.2 | 3.1.2 | ✅ | Compatible as-is |

---

## Build Comparison

```bash
# Maven build command (same for both Java versions via toolchain)
./mvnw clean package -DskipTests
```

| Aspect | Java 8 (Baseline) | Java 21 | Status | Notes |
|--------|-------------------|---------|:------:|-------|
| Build Result | SUCCESS | SUCCESS | ✅ | Both versions build successfully |
| Build Time | 6.080s | 5.771s | ℹ️ | Java 21 5% faster (0.309s improvement) |

---

## Unit Test Comparison

```bash
# Maven test command (same for both Java versions via toolchain)
./mvnw test
```

| Aspect | Java 8 (Baseline) | Java 21 | Status | Notes |
|--------|-------------------|---------|:------:|-------|
| Test Result | SUCCESS | SUCCESS | ✅ | Both versions pass all tests |
| Test Time | 8.774s | 5.139s | ℹ️ | Java 21 41% faster (3.635s improvement) |

---

## Runtime Comparison

```bash
# N/A — library project, no bootable application
```

| Aspect | Java 8 (Baseline) | Java 21 | Status | Notes |
|--------|-------------------|---------|:------:|-------|
| Startup Result | N/A | N/A | ✅ | Library project — no bootable application |
| Startup Time | N/A | N/A | ✅ | Library project — no bootable application |

---

## Performance Improvements

- Build packaging: Java 21 5% faster (6.080s → 5.771s, 0.309s improvement)
- Test execution: Java 21 41% faster (8.774s → 5.139s, 3.635s improvement)
- Compilation: Java 21 42% faster (5.363s → 3.105s, 2.258s improvement)
- Bytecode: Upgraded from major version 52 (Java 8) to 65 (Java 21)

### JVM Optimizations Applied
- Maven toolchain configured to use Amazon Corretto 21.0.8 JDK
- Compiler `release` property used instead of `source/target` for complete Java 21 version control
- No `--add-opens` JVM arguments needed (library project, no Spring Boot runtime)

### Configuration Updates Summary
- Compiler: `<source>1.8</source>/<target>1.8</target>` → `<release>21</release>`
- Toolchain: Enabled with `[21,22)` version range, `amazon` vendor
- Profile: Toolchain profile uncommented and activated by default

---

## Knowledge Outcomes

- **New Lessons Discovered**: 0
- **Existing Lessons Applied**: 1
- **Promotion Candidates**: 0
- **Total Framework Knowledge**: 6

| Applied Lesson | Usage Frequency | Issue Resolved | Solution Applied | Promotion Status |
|----------------|-----------------|----------------|------------------|------------------|
| mockito-java21-spring-boot2-bytebuddy-compatibility | 4 | Mockito 1.10.19 / ByteBuddy incompatible with Java 21 class definition | Override mockito-core to 4.11.0 and byte-buddy to 1.12.23 in parent POM | Approaching promotion (4/5) |

---

## Next Steps

### Immediate Technical Actions
- **Update CI/CD pipelines**: Configure build systems to use Java 21 JDK (Amazon Corretto 21.0.8)
- **Update developer documentation**: Replace Java 8 references with Java 21 in README
- **Verify runtime environments**: Test Java 21 deployment in all environments (dev, staging, production)
- **Update IDE configurations**: Configure development environments for Java 21 project settings

### Production Deployment Considerations
- **JDK Installation**: Ensure Java 21 JDK available in all production environments
- **Toolchain Configuration**: Developers need `~/.m2/toolchains.xml` with Java 21 entry
- **Maven Independence**: Maven can run on any Java version — project uses toolchain-specified Java 21
- **No --add-opens Required**: Library project requires no module system workarounds

### Step 3 Preparation
- **Spring Boot Compatibility**: Current Spring Boot 1.5.22 → target Spring Boot 3.x
- **Jakarta EE Migration**: 17 javax.validation imports will need migration to jakarta.validation
- **Dependency Assessment**: jackson-databind 2.15.4 compatible with Spring Boot 3.x

---

**Generated by**: Amazon Q Migration Framework  
**Technical Contact**: Development Team
