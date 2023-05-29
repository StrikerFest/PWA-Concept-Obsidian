# Overview

- The [[Buildpack]] library provides the tools you need to configure your environment and larger, overall workflows that a developer has to configure and control.
- These configurations differ across projects and different environments within those projects
- For example, environments for development, testing, staging, and production are configured to support different behaviors

## The `.env` file

- Like the rest of PWA Studio, buildpack uses environment variables as its central source of configuration settings
- A PWA Studio project using buildpack requires a [[.env file]] in its root directory.
- Each line in the file contains a configuration using the following form

```
	NAME=value
```

- In any script in any programming language, you can access these environment variables directly by sourcing the file as a legal POSIX shell script

## Command Line Interface (CLI)

- Buildpack provides a [[Buildpack CLI]] for creating `.env` files and validating environments
- It also provides library methods for connecting environment management workflows with other tools

- Using these provided tools, you can keep global configuration values in a central location and propagate them throughout your project.
- This lets you pass common settings down to different library functions without tightly coupling those settings together

## Configuration management rationale

- PWA Studio follows the principle that all configuration that can be environment variables, should be.

- Environment variables are portable, cross-platform, and reasonably secure. 
- They can be individually overridden to give the user a great deal of control over a complex system.
- The [[twelve-factor app]] methodology recommends storings config in the environment as its third factor 

- Many tools use environment variables strictly as edge- case overrides and store their canonical configuration in other formats because under the strict POSIX definition, environment variables have some limitations
	- An environment variable name is case insensitive
	- An environment variable's value can only be a string
	- Environment variables cannot be nested nor schematized, so they have no built-in data structures.
	- Environment variables all belong to a single namespace, and every running process has access to all of them

- These drawbacks are serious enough that some applications use alternate formats, such as
	- XML
	- JSON
	- YAML
	- INI/TOML
	- .properties file in Java
	- .plist files in MacOS
	- PHP associative arrays
	- Apache directives

- These formats have the following advantages over environment variables
	- They are a standard human-readable file format
	- They can support nesting and/or namespacing to organize values
	- They support data types and metadata

- However, none of these formats have `won` and become a replacement for environment variables.
- Each one has its own set of quirks and undefined behaviors.
- None of them are deeply integrated with OS, shell and container environments, and they often do not work consistenty across language runtimes

- PWA Studio chooses to use environment variables, while providing simple tools for file format, namespacing and validation

- A centralized configurator passes on formatted pieces of the environments to specific tools as parameters, so these tools do not need to know the specifics of the configuration scheme.
- Entry point scripts, such as `server.js` and `webpack.config.js` can use the [[loadEnvironment()]] tool to deserialize environment variables into any kind of data structure, while storing persistent values in an `.env` file in the project root directory

- Buildpack combines the features of several tools
	- [[dotenv]] for managing environment variables with `.env` files
	- [[envalid]] for describing, validating and making defaults for settings
	- [[camelspace]] for easily translating configuration between flat environment variables and namespaced objects

## Best pratices

- The config rule in the [[twelve-factor app]] methodology distinguishes configuration that "does not vary between deploys" from configuration that does 
- It requires that configuration that does change between deploys be stored in the environment.
- PWA Studio does not make such a distinction
- For config that must never vary, the PWA project maintainer can hardcode that configuration in the entrypoint scripts what use `loadEnvironment()`

- To have environment-variable-based configuration management and enjoy the benefits of file format, namespacing, and validation at the same time, it's important to use `loadEnvironment()` in a certain way

### Configuration object

- The purpose of a function such as [[loadEnvironment()]] is to keep configuration organized without tightly coupling a system to a manager object.
- To achieve this, it is important to use `loadEnvironment()` and the `Configuration` object it produces at the "top level" or entry point of a program
- Avoid passing a `Configuration` object directly to other tools
- These tools should be usable without `loadEnvironment()` 

# THERE's MORE