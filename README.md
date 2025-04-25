# Fixing Transitive Dependencies in Java

This project demonstrates how to fix vulnerable transitive dependencies in a Java Maven project, specifically focusing on log4j vulnerabilities.

Your task is to update log4j-core to get rid of the vulnerabilities in this packages bill of materials. First review the vulnerabilities, then update the package.

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

## Scan for Vulnerabilities

We'll use `endorctl` to scan the project for known vulnerabilities.

#### Step-by-step:

1. **Initialize Endor Labs**  
   Run the following command to authenticate with Endor Labs and set up your environment:
   ```bash
   ./endorctl init --auth-mode <mode> --headless-mode
   ```
   Replace `<mode>` with your preferred authentication mode (e.g., `google`, `github`, etc.).

2. **Authenticate via Portal**  
   The command will output a URL. You can command-click (⌘+click) the link in your terminal to open the authentication portal.  
   Log in and copy the generated token.

3. **Complete Setup**  
   Paste the token back into your terminal.  
   You'll then be prompted to select a tenant—choose the one you just created.

4. **Run the Vulnerability Scan**  
   Once authenticated and configured, scan your codebase:
   ```bash
   ./endorctl scan
   ```
   This will analyze your project for security vulnerabilities.
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

## Verify the Fix

1. Rebuild the project:

```bash
mvn clean install
```

2. Check the dependency tree again:

```bash
mvn dependency:tree
```

3. Run vulnerability scan:

```bash
./endorctl scan
```

Ensure that log4j now appears with the safe version you specified.
