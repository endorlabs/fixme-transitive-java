# Fixing Transitive Dependencies in Java

This project demonstrates how to fix vulnerable transitive dependencies in a Java Maven project, specifically focusing on log4j vulnerabilities.

## Quick Start

1. Fork and clone the repository:

```bash
git clone https://github.com/endorlabs/fixme-transitive-java.git
cd fixme-transitive-java
```

1. Build the project:

```bash
mvn clean install
```

## Identifying the Problem

1. View the dependency tree:

```bash
mvn dependency:tree
```

Key findings:

- `anteros-core` is a direct dependency
- It brings in potentially vulnerable `log4j` versions as transitive dependencies

1. (Optional) Scan for vulnerabilities:

```bash
curl -LO https://github.com/google/osv-scanner/releases/download/v1.9.2/osv-scanner_linux_amd64
mv osv-scanner_linux_amd64 osv-scanner
chmod +x ./osv-scanner
./osv-scanner scan -L pom.xml
```

## Fixing the Issue

### Option 1: Upgrade Direct Dependency

1. Update `anteros-core` version in `pom.xml`:

```xml
<dependency>
    <groupId>com.anteros</groupId>
    <artifactId>anteros-core</artifactId>
    <version>INSERT_LATEST_VERSION</version>
</dependency>
```

1. Rebuild and verify:

```bash
mvn clean install
mvn dependency:tree
```

### Option 2: Override Transitive Dependency

If Option 1 doesn't resolve the issue, add this to your `pom.xml`:

```xml
    <dependencies>
        <dependency>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
            <version>2.17.2</version>
        </dependency>
    </dependencies>
```

Then add the direct dependency:

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.17.2</version>
    </dependency>
</dependencies>
```

## Verify the Fix

1. Rebuild the project:

```bash
mvn clean install
```

1. Check the dependency tree again:

```bash
mvn dependency:tree
```

1. Run vulnerability scan:

```bash
./osv-scanner scan -f pom.xml
```

Ensure that log4j now appears with the safe version you specified.
