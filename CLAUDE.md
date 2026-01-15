# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build Commands

```bash
# Build the plugin (generates JAR in Towny/target/)
mvn clean package

# Build with tests enabled
mvn clean package -DskipTests=false

# Run specific test class
mvn test -DskipTests=false -Dtest=WorldCoordTests

# Generate JavaDoc
mvn javadoc:jar
```

## Project Structure

Multi-module Maven project for the Towny Minecraft plugin (Java 17, Paper API 1.19+).

```
Towny/                      # Parent POM
├── Towny/                  # Main plugin module
│   └── src/main/java/com/palmergames/bukkit/towny/
└── providers/              # Server implementation abstraction
    ├── BaseProviders/      # Interface definitions
    ├── PaperProvider/      # Paper-specific implementation
    └── FoliaProvider/      # Folia threading support
```

## Architecture

**Core Data Objects** (`object/`): `Resident`, `Town`, `Nation`, `TownBlock`, `TownyWorld`, `WorldCoord`

**Singletons**:
- `TownyAPI` - Public API for external plugins
- `TownyUniverse` - Internal registry managing all game objects

**Database Layer** (`db/`): `TownyDataSource` interface with `TownyFlatFileSource` (YAML) and `TownySQLSource` (MySQL/MariaDB via HikariCP) implementations

**Command System** (`command/`): `BaseCommand` base class. Major command classes: `TownCommand`, `NationCommand`, `PlotCommand`, `TownyAdminCommand`, `ResidentCommand`

**Event System** (`event/`): Custom Towny events for town/nation/economy/teleport actions. Listeners in `listeners/` package handle Bukkit events.

**Task Scheduling**: Provider abstraction (`providers/BaseProviders/scheduling/`) supports Bukkit, Paper, and Folia threading models. Background tasks in `tasks/` package.

**Hooks** (`hooks/`): Economy (Vault), permissions (LuckPerms, GroupManager), PlaceholderAPI integration

## Code Style

- **Indentation**: Tabs for Java, spaces for YAML/XML/MD (enforced via `.editorconfig`)
- **Line endings**: CRLF (Windows)
- **Imports**: No wildcards (configured in `.editorconfig`)
- **API**: Spigot API only - no NMS, reflection, or direct protocol access
- **Dependencies**: Must be pre-approved; use `provided` scope (no embedded deps)

## Testing

Tests are in `Towny/src/test/java/` using JUnit 5 and MockBukkit. Tests are skipped by default.

## Key Files

- `Towny/src/main/resources/plugin.yml` - Plugin manifest, commands, permissions
- `Towny/src/main/resources/lang/` - Translation files (managed via Crowdin)
- `Towny/src/main/resources/config-migration.json` - Config upgrade paths

## License

CC BY-NC-ND 3.0 - Contributions must agree to license terms via PR.