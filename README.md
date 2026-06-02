# Java Method Extractor

A Java-based tool that uses JavaParser to extract method metadata from Java codebases and export to Excel/CSV formats.

## Features

- Recursively scans Java source files in a directory
- Extracts method information including:
  - File path
  - Class name
  - Annotations (Step Definitions)
  - Method name
  - Return type
  - Parameters
  - Access modifiers
- Exports to Excel (.xlsx) and/or CSV formats

## Prerequisites

- Java 21 or higher
- Gradle 9.2.0+ (for Gradle setup) OR Maven 3.6+ (for Maven setup)

## ⚠️ Important Notes

- **All main classes have hardcoded file paths that MUST be updated before running**
- **Performance Warning**: Excel export auto-resizes columns on every row insertion, which can be slow for large codebases
- **Known Issue**: Variable name typo `accesModifiers` (missing 's') exists throughout the codebase
- **Testing**: Only one test exists (greeting message verification), no tests for parsing functionality

## Project Setup

### Option 1: Gradle Setup (Current Implementation)

This project is currently configured with Gradle.

#### Build the Project
```bash
./gradlew build
```

#### Run Tests
```bash
./gradlew test
```

#### Run Specific Test
```bash
./gradlew test --tests javaparser.test.AppTest
```

#### Run the Application
```bash
./gradlew run
```

**Note:** Before running, you must update the hardcoded paths in the main class files (see Configuration section below).

#### Clean Build
```bash
./gradlew clean build
```

### Option 2: Maven Setup

To convert this project to Maven, create a `pom.xml` file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>javaparser.test</groupId>
    <artifactId>javaparser</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>Java Method Extractor</name>
    <description>Extract method metadata from Java codebases</description>

    <properties>
        <maven.compiler.source>21</maven.compiler.source>
        <maven.compiler.target>21</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <javaparser.version>3.28.2</javaparser.version>
        <poi.version>5.3.0</poi.version>
        <poi-ooxml.version>5.5.1</poi-ooxml.version>
        <junit.version>5.12.1</junit.version>
        <guava.version>33.4.6-jre</guava.version>
    </properties>

    <dependencies>
        <!-- JavaParser -->
        <dependency>
            <groupId>com.github.javaparser</groupId>
            <artifactId>javaparser-core-serialization</artifactId>
            <version>${javaparser.version}</version>
        </dependency>

        <!-- Apache POI for Excel -->
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi</artifactId>
            <version>${poi.version}</version>
        </dependency>
        <dependency>
            <groupId>org.apache.poi</groupId>
            <artifactId>poi-ooxml</artifactId>
            <version>${poi-ooxml.version}</version>
        </dependency>

        <!-- Guava -->
        <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>${guava.version}</version>
        </dependency>

        <!-- JUnit for testing -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <sourceDirectory>app/src/main/java</sourceDirectory>
        <testSourceDirectory>app/src/test/java</testSourceDirectory>
        
        <plugins>
            <!-- Maven Compiler Plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.13.0</version>
                <configuration>
                    <source>21</source>
                    <target>21</target>
                </configuration>
            </plugin>

            <!-- Maven Surefire Plugin for tests -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.5.2</version>
            </plugin>

            <!-- Exec Maven Plugin to run the application -->
            <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>3.5.0</version>
                <configuration>
                    <mainClass>javaparser.test.App</mainClass>
                </configuration>
            </plugin>

            <!-- Maven JAR Plugin -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.4.2</version>
                <configuration>
                    <archive>
                        <manifest>
                            <mainClass>javaparser.test.App</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

#### Maven Commands

```bash
# Build the project
mvn clean install

# Run tests
mvn test

# Run specific test
mvn test -Dtest=AppTest

# Run the application
mvn exec:java

# Package as JAR
mvn package

# Run the packaged JAR
java -jar target/javaparser-1.0-SNAPSHOT.jar
```

## Configuration

Before running the application, you must update the hardcoded file paths in the main class files:

### App.java (Excel export only)
Edit `app/src/main/java/javaparser/test/App.java` lines 35-36:
```java
String dirPath = "PATH/TO/PROJECT";  // Directory containing Java files to analyze
String xlsxFile = "PATH/TO/SAVE/FILE/methods.xlsx";  // Output Excel file path
```

## Available Main Classes

The project contains three main classes with different functionality:

1. **App.java** (default) - Exports to Excel only

### Running Different Main Classes

#### Gradle
```bash
# Run App.java (default)
./gradlew run
```

#### Maven
```bash
# Run App.java (default)
mvn exec:java
```

## Output Format

### Excel Output
Creates a spreadsheet with 7 columns:
- File Path
- ClassName
- Step Definitions (Annotations)
- Methods
- Method Return Type
- Method Parameters
- Access Modifiers


## Known Issues

- **Performance**: The Excel export auto-resizes columns on every row insertion, which can be slow for large codebases
- **Error Handling**: Exceptions are caught and printed to console but don't stop execution
- **Row Numbering**: Inconsistent between App.java (starts at 0)

## Dependencies

- JavaParser 3.28.2 (core-serialization)
- Apache POI 5.3.0 and 5.5.1 (Excel support)
- Google Guava 33.4.6-jre
- JUnit Jupiter 5.12.1 (testing)

## Project Structure

```
javaparser/
├── app/
│   ├── build.gradle
│   └── src/
│       ├── main/java/javaparser/test/
│       │   ├── App.java                    # Main class (Excel only)
│       └── test/java/javaparser/test/
│           └── AppTest.java
├── gradle/
├── build.gradle
├── settings.gradle
└── gradlew / gradlew.bat
```
