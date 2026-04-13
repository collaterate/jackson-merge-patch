# Spring Boot & Dependency Upgrade Technical Report

**Project**: Jackson Merge Patch  
**Migration**: Step 3: Spring Boot & Dependency Upgrade  
**Date**: 2026-04-13  
**Status**: ✅ SUCCESS

---

## Upgrade Summary

### Files Modified
**Modified Files:** 7
- `pom.xml` — Spring Boot 3.5.13 BOM import, all dependency/plugin versions updated, removed explicit mockito/byte-buddy overrides
- `jackson-merge-patch-core/pom.xml` — parent version 4.0.0, scope annotations added for test deps
- `jackson-merge-patch-validations/pom.xml` — parent version 4.0.0, javax.validation:validation-api replaced with jakarta.validation:jakarta.validation-api, hibernate-validator updated to BOM-managed
- `jackson-merge-patch-core/src/test/java/.../PatchFieldDTOControllerTest.java` — JUnit 5, Mockito 5 openMocks, Spring 6 HttpStatusCode API
- `jackson-merge-patch-validations/src/test/java/.../PatchNotNullValidatorTest.java` — JUnit 5
- `jackson-merge-patch-validations/src/test/java/.../PatchNumberValidationsControllerTest.java` — JUnit 5, Mockito 5 openMocks, Spring 6 HttpStatusCode API, jakarta.validation.Valid
- `jackson-merge-patch-validations/src/test/java/.../PatchStringValidatorTest.java` — JUnit 5

**Source Files (main):** 8 files — javax.validation → jakarta.validation imports + message key updates

### Version Changes
- **Spring Boot**: 1.5.22.RELEASE → 3.5.13
- **Spring Cloud**: N/A (not used)
- **Dependencies Updated**: 15
- **Plugins Updated**: 6

### Jakarta EE Migration
- **Trigger**: Spring Boot < 3.0 detected (1.5.22.RELEASE)
- **Scope**: 9 source files (8 main + 1 test) with javax.validation imports
- **Changes**: javax.validation.Constraint → jakarta.validation.Constraint, javax.validation.Payload → jakarta.validation.Payload, javax.validation.ConstraintValidator → jakarta.validation.ConstraintValidator, javax.validation.ConstraintValidatorContext → jakarta.validation.ConstraintValidatorContext, javax.validation.Valid → jakarta.validation.Valid
- **Message Keys**: javax.validation.constraints.Min.message → jakarta.validation.constraints.Min.message, javax.validation.constraints.Max.message → jakarta.validation.constraints.Max.message
- **Verification**: Zero javax.validation references remain in source

### Additional Migrations
- **JUnit 4 → 5**: 4 test files — org.junit.Test → org.junit.jupiter.api.Test, org.junit.Before → org.junit.jupiter.api.BeforeEach, org.junit.Assert → org.junit.jupiter.api.Assertions (parameter order reversed for message)
- **Mockito**: MockitoAnnotations.initMocks → MockitoAnnotations.openMocks (2 test files)
- **Spring MVC**: ResponseEntityExceptionHandler.handleExceptionInternal and handleMethodArgumentNotValid parameter HttpStatus → HttpStatusCode (2 test files)

---

## Dependency Updates

### Spring Boot & Cloud
| Component | Before | After | Status |
|-----------|--------|-------|:------:|
| Spring Boot | 1.5.22.RELEASE | 3.5.13 | ✅ |
| Spring Cloud | N/A | N/A | ✅ |

### Updated Dependencies
| Dependency | Before | After | Status | Notes |
|------------|--------|-------|:------:|-------|
| **Spring Boot Starters** | | | | |
| spring-boot-starter-test | 1.5.22.RELEASE | 3.5.13 | ✅ | BOM-managed |
| spring-boot-starter-web | 1.5.22.RELEASE | 3.5.13 | ✅ | BOM-managed |
| **Core Dependencies** | | | | |
| jackson-databind | 2.15.4 | 2.21.2 | ✅ | BOM-managed |
| lombok | 1.18.38 | 1.18.44 | ✅ | |
| **Validation** | | | | |
| javax.validation:validation-api | 2.0.1.Final | — | ℹ️ | Replaced by jakarta |
| jakarta.validation:jakarta.validation-api | — | 3.0.2 | ✅ | BOM-managed |
| hibernate-validator | 6.2.5.Final | 8.0.3.Final | ✅ | BOM-managed |
| **Utility Libraries** | | | | |
| commons-lang3 | 3.5 | 3.20.0 | ✅ | |
| guava | 28.0-jre | 33.5.0-jre | ✅ | |
| commons-io | 2.6 | 2.21.0 | ✅ | |
| **Test Dependencies** | | | | |
| mockito-core | 4.11.0 | 5.17.0 | ✅ | BOM-managed |
| byte-buddy | 1.12.23 | 1.17.8 | ✅ | BOM-managed |
| byte-buddy-agent | 1.12.23 | 1.17.8 | ✅ | BOM-managed |
| junit | 4.12 | junit-jupiter 5.12.2 | ✅ | BOM-managed |
| **Plugins** | | | | |
| maven-compiler-plugin | 3.11.0 | 3.15.0 | ✅ | |
| maven-surefire-plugin | 3.1.2 | 3.5.5 | ✅ | |
| maven-jar-plugin | 3.3.0 | 3.5.0 | ✅ | |
| maven-source-plugin | 3.3.0 | 3.4.0 | ✅ | |
| jacoco-maven-plugin | 0.8.12 | 0.8.14 | ✅ | |
| maven-toolchains-plugin | 3.1.0 | 3.2.0 | ✅ | |

---

## Build Comparison

### Maven Commands
```bash
# Validation
./mvnw validate

# Build
./mvnw clean compile
./mvnw package -DskipTests

# Test
./mvnw test

# Runtime (if applicable)
# Skipped — library project, no spring-boot-maven-plugin
```

### Build Results
| Aspect | Before | After | Status |
|--------|--------|-------|:------:|
| Validation | SUCCESS (0.317s) | SUCCESS (0.626s) | ✅ |
| Compilation | SUCCESS (3.864s) | SUCCESS (4.489s) | ✅ |
| Tests | SUCCESS (8.657s, 22 tests, 0 failures) | SUCCESS (8.389s, 22 tests, 0 failures) | ✅ |
| Runtime | Skipped (library) | Skipped (library) | ✅ |

---

## Knowledge Outcomes

- **New Lessons Discovered**: 0
- **Existing Lessons Applied**: 0
- **Total Framework Knowledge**: 8

| Applied Lesson | Usage Frequency | Issue Resolved | Solution Applied | Promotion Status |
|----------------|-----------------|----------------|------------------|------------------|
| N/A — No issues encountered during this migration | — | — | — | — |

---

## Next Steps

### Immediate Technical Actions
- Update CI/CD pipelines for Spring Boot 3.5.13
- Update developer documentation with new dependency versions
- Verify in all environments (dev, staging, production)
- Notify downstream consumers of jakarta.validation namespace change (breaking API change)

### Production Deployment
- Publish 4.0.0 artifacts to artifact repository
- Downstream consumers must update imports: javax.validation → jakarta.validation
- No runtime deployment needed (library project)

---

**Generated by**: Amazon Q Migration Framework
