# Hitorro Maven Repository

This is a GitHub-hosted Maven repository for Hitorro project artifacts.

Artifacts are deployed to this repository and pushed to GitHub, allowing consumers to reference them directly via HTTPS without cloning the repository.

## Repository Structure

This repository follows the standard Maven repository layout:
```
com/hitorro/
├── hitorro-util/
│   └── 3.0.0/
│       ├── hitorro-util-3.0.0.jar
│       ├── hitorro-util-3.0.0.pom
│       └── maven-metadata.xml
├── hitorro-base/
│   └── 3.0.0/
│       └── ...
└── ...
```

## Publishing Artifacts to This Repository

From the Hitorro project directory (`/Users/chris/hitorro`):

```bash
# Build and deploy all modules to this repository
mvn clean deploy

# Or deploy without running tests
mvn clean deploy -DskipTests
```

This will compile all modules and install them to this repository.

## Using This Repository in Other Projects

To use artifacts from this repository in another Maven project, add the following to your `pom.xml`:

### Option 1: GitHub Repository (Recommended)

Add this `<repositories>` section to your project's `pom.xml`:

```xml
<repositories>
    <repository>
        <id>hitorro-maven</id>
        <name>Hitorro Maven Repository</name>
        <url>https://raw.githubusercontent.com/geekychris/hitorro-maven/main</url>
        <releases>
            <enabled>true</enabled>
        </releases>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
</repositories>
```

### Option 2: Local File Repository (For local development)

If you have this repository cloned locally:

```xml
<repositories>
    <repository>
        <id>hitorro-local</id>
        <name>Hitorro Local Maven Repository</name>
        <url>file://${user.home}/code/hitorro-maven</url>
        <releases>
            <enabled>true</enabled>
        </releases>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
    </repository>
</repositories>
```

Then add dependencies as normal:

```xml
<dependencies>
    <dependency>
        <groupId>com.hitorro</groupId>
        <artifactId>hitorro-util</artifactId>
        <version>3.0.0</version>
    </dependency>
    <dependency>
        <groupId>com.hitorro</groupId>
        <artifactId>hitorro-base</artifactId>
        <version>3.0.0</version>
    </dependency>
    <!-- Add other hitorro modules as needed -->
</dependencies>
```

### Option 3: Add to Maven Settings (Global)

Alternatively, you can add this repository to your Maven settings file (`~/.m2/settings.xml`) to make it available to all projects:

```xml
<settings>
    <profiles>
        <profile>
            <id>hitorro-maven-repo</id>
            <repositories>
                <repository>
                    <id>hitorro-maven</id>
                    <name>Hitorro Maven Repository</name>
                    <url>https://raw.githubusercontent.com/geekychris/hitorro-maven/main</url>
                    <releases>
                        <enabled>true</enabled>
                    </releases>
                    <snapshots>
                        <enabled>true</enabled>
                    </snapshots>
                </repository>
            </repositories>
        </profile>
    </profiles>
    
    <activeProfiles>
        <activeProfile>hitorro-maven-repo</activeProfile>
    </activeProfiles>
</settings>
```

## Version Management

The Hitorro project includes scripts for version management:

### Bump Major Version (3.0.0 → 4.0.0)
```bash
cd /Users/chris/hitorro
./bump-major-version.sh
mvn clean deploy
```

### Bump Minor Version (3.0.0 → 3.1.0)
```bash
cd /Users/chris/hitorro
./bump-minor-version.sh
mvn clean deploy
```

### Bump Patch Version (3.0.0 → 3.0.1)
```bash
cd /Users/chris/hitorro
./bump-patch-version.sh
mvn clean deploy
```

### Set Specific Version
```bash
cd /Users/chris/hitorro
./set-version.sh 3.2.5
mvn clean deploy
```

## Deployment Process

This repository is automatically updated when deploying from the Hitorro project:

1. **Deploy artifacts** from the Hitorro project:
   ```bash
   cd ~/hitorro
   mvn clean deploy -DskipTests
   ```

2. **Commit and push** the deployed artifacts:
   ```bash
   cd ~/code/hitorro-maven
   git add .
   git commit -m "Deploy hitorro artifacts version X.Y.Z
   
   Co-Authored-By: Warp <agent@warp.dev>"
   git push
   ```

3. **Artifacts are now available** at:
   ```
   https://raw.githubusercontent.com/geekychris/hitorro-maven/main
   ```

### Customizing Deployment Path

The default deployment path is `${user.home}/code/hitorro-maven`. You can customize it:

- **Command line**: `mvn deploy -Dhitorro.deploy.repo.path=/custom/path`
- **POM property**: Edit `<hitorro.deploy.repo.path>` in the root `pom.xml`
- **Maven settings**: Define the property in `~/.m2/settings.xml`

## Available Modules

The following Hitorro modules are available in this repository:

- `hitorro-util`
- `hitorro-base`
- `hitorro-unittime`
- `hitorro-features`
- `hitorro-jsonsql`
- `hitorro-objretrieval`
- `hitorro-text-core`
- `hitorro-text-persistence`
- `hitorro-basedms`
- `hitorro-dedupe`
- `hitorro-analysis`
- `hitorro-logdigest`
- `hitorro-dataaquisition`
- `hitorro-conversation`
- `hitorro-baseui`
- `hitorro-test`
- `hitorro-app`
- `hitorro-jsonts-mongo`
- `hitorro-spring-boot`
- `hitorro-example-springboot`

All modules share the same version number (currently `3.0.0`).

## Notes

- This is a file-based repository, so it's lightweight and requires no server
- Maven automatically generates metadata files when deploying
- The repository can be committed to Git and shared with your team
- For production use, consider a proper repository manager like Nexus or Artifactory
