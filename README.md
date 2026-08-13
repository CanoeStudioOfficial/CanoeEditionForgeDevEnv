# CanoeEditionForgeDevEnv

Multi-module template for Minecraft 1.12.2 Forge mod development.

This template runs on **Java 25**, **Gradle 9.7.0**, **RetroFuturaGradle 2.0.3**, and **Forge 14.23.5.2847**.

## Start using the template

1. Create a repository from this template and clone it locally.
2. Configure IntelliJ IDEA to use Java 25 for Gradle.
3. Open the repository root and import the root `build.gradle` as a Gradle project.
4. Run `listMods` to confirm that the example module was discovered.

The root project is only an aggregator. Each mod lives under `modules/<modid>/` and gets the shared build logic from `gradle/scripts/mod-build.gradle`.

## Module layout

The included example module is deliberately complete and can be copied as a starting point:

```text
modules/
└── modid/
    ├── gradle.properties
    ├── tags.properties
    ├── CHANGELOG.md
    └── src/main/
        ├── java/com/example/modid/ExampleMod.java
        └── resources/
            ├── mcmod.info
            └── pack.mcmeta
```

`settings.gradle` automatically includes every directory under `modules/` that contains a `gradle.properties` file. The module's `gradle.properties` contains its mod ID, name, package, version, mappings, and publishing options.

## Add another mod

Copy `modules/modid/` to a new directory, then update at least these values in the copied module's `gradle.properties`:

```properties
mod_id = examplemod
mod_name = Example Mod
root_package = com.example.examplemod
```

Also update the Java package and resource metadata placeholders. After reloading Gradle, the new module is available as `:examplemod`.

For module-specific dependencies or Gradle customization, create `modules/examplemod/extra.gradle`:

```groovy
repositories {
    maven { url = 'https://example.invalid/maven' }
}

dependencies {
    implementation project(':modid')
    // compileOnly rfg.deobf('curse.maven:some-mod-123456:7890123')
}
```

The `implementation project(':modid')` line makes the example module available during compilation and runtime. Use `compileOnly` for optional integrations that should not be bundled as a required dependency.

## Common commands

```powershell
# List modules discovered from modules/
.\gradlew.bat listMods

# Build every module
.\gradlew.bat buildAllMods

# Build one module
.\gradlew.bat :modid:build

# Run one module in the development client/server
.\gradlew.bat :modid:runClient
.\gradlew.bat :modid:runServer
```

Artifacts are written to `modules/<modid>/build/libs/`.

Shared dependency and publishing behavior is documented in `gradle/scripts/dependencies.gradle` and `gradle/scripts/publishing.gradle`. Mixin and coremod options remain configurable per module in its `gradle.properties`.
