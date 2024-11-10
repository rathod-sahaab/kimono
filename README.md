# Kimono Monorepo

**Kimono** is a modular monorepo designed to facilitate the development of large-scale applications using a variety of tools and languages. The repository uses **Bazel** as the build tool, and supports **TypeScript**, **Go (Golang)**, and **Rust** as the primary programming languages for different services and components within the monorepo.

This setup enables efficient dependency management, fast builds, and consistent workflows across diverse programming environments.

## Table of Contents

- [Overview](#overview)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Building the Project](#building-the-project)
  - [Running the Project](#running-the-project)
- [Folder Structure](#folder-structure)
- [Development Workflow](#development-workflow)
  - [Adding a New Package](#adding-a-new-package)
  - [Running Tests](#running-tests)
  - [Building Packages](#building-packages)
  - [Linting & Formatting](#linting--formatting)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

## Overview

Kimono is a **polyglot** monorepo built to handle multiple projects developed in **TypeScript**, **Go**, and **Rust**. The repository is organized using **Bazel**, a fast and flexible build tool that works well with multiple languages and scales efficiently for large repositories.

Key Features:
- **Bazel** as the build system for managing dependencies, builds, and tests.
- Multi-language support, with **TypeScript**, **Go**, and **Rust** as the core languages.
- **Modularization**: Independent, reusable packages within the monorepo.
- Optimized build times and efficient testing across languages.

This repository structure ensures that each language (TypeScript, Go, Rust) can evolve independently, with a uniform build, test, and deployment process.

## Getting Started

### Prerequisites

Before you start, ensure you have the following tools installed:

- **Bazel** (version 5.x or later) — The primary build tool for the monorepo. Follow the [Bazel installation guide](https://bazel.build) for your platform.
- **Node.js** (version >= 14.x) — Required for TypeScript packages.
- **Go** (version >= 1.18) — Required for Go services.
- **Rust** (version >= 1.64) — Required for Rust services.
- **Git** — For version control.
- **Docker** (optional) — For containerized services.

### Installation

1. **Clone the repository**:

   ```bash
   git clone https://github.com/your-org/kimono.git
   cd kimono
   ```

2. **Install dependencies** for TypeScript, Go, and Rust using Bazel:

   ```bash
   bazel fetch //...
   ```

3. **Install Node.js dependencies** (for TypeScript):

   In the root of the repo, run:

   ```bash
   yarn install
   ```

4. **Install Go dependencies**:

   For Go packages, install dependencies via `go mod`:

   ```bash
   cd packages/go-service
   go mod tidy
   ```

5. **Install Rust dependencies**:

   For Rust packages, run:

   ```bash
   cd packages/rust-service
   cargo build
   ```

### Building the Project

Bazel handles the build process for all languages in the repository. To build everything in the repository:

```bash
bazel build //...
```

To build a specific package (e.g., a TypeScript service or a Go service):

- **TypeScript**:

  ```bash
  bazel build //packages/typescript-service:typescript-service
  ```

- **Go**:

  ```bash
  bazel build //packages/go-service:go-service
  ```

- **Rust**:

  ```bash
  bazel build //packages/rust-service:rust-service
  ```

### Running the Project

Bazel also facilitates running different services. To run a service, use the following command:

- **TypeScript**:

  ```bash
  bazel run //packages/typescript-service:typescript-service
  ```

- **Go**:

  ```bash
  bazel run //packages/go-service:go-service
  ```

- **Rust**:

  ```bash
  bazel run //packages/rust-service:rust-service
  ```

### Running Tests

Bazel also integrates with testing frameworks for TypeScript, Go, and Rust. To run tests for all packages:

```bash
bazel test //...
```

To run tests for a specific package:

- **TypeScript**:

  ```bash
  bazel test //packages/typescript-service:test
  ```

- **Go**:

  ```bash
  bazel test //packages/go-service:test
  ```

- **Rust**:

  ```bash
  bazel test //packages/rust-service:test
  ```

## Folder Structure

The `kimono` monorepo is structured to accommodate different services and components developed using TypeScript, Go, and Rust. Here’s an overview of the folder structure:

```
kimono/
├── WORKSPACE                     # Bazel workspace configuration file
├── BUILD.bazel                   # Root build file for configuring Bazel
├── packages/                     
│   ├── typescript-service/        # TypeScript service package
│   │   ├── BUILD.bazel           # Bazel build file for TypeScript service
│   │   └── src/                  # TypeScript source files
│   ├── go-service/               # Go service package
│   │   ├── BUILD.bazel           # Bazel build file for Go service
│   │   └── src/                  # Go source files
│   ├── rust-service/             # Rust service package
│   │   ├── BUILD.bazel           # Bazel build file for Rust service
│   │   └── src/                  # Rust source files
│   └── ...
├── .gitignore                    # Git ignore file
├── .bazelrc                       # Bazel configuration file
├── package.json                  # Node.js package manager file for TypeScript packages
├── yarn.lock                     # Yarn lock file for TypeScript dependencies
└── README.md                     # This file
```

- **`WORKSPACE`**: The root file that tells Bazel that this directory is a Bazel workspace.
- **`BUILD.bazel`**: Build configuration files used by Bazel for each package.
- **`packages/`**: Contains the TypeScript, Go, and Rust services. Each service is a self-contained package.
- **`src/`**: Source code for each service.
- **`.bazelrc`**: Configuration file for Bazel settings.
  
## Development Workflow

### Adding a New Package

To add a new service or package to the monorepo:

1. **Create a new directory** under `packages/` for your new service.
2. **Create a `BUILD.bazel` file** to define the build rules for your package. For example, a TypeScript service might include:

   ```bazel
   load("@build_bazel_typescript//:index.bzl", "ts_library")

   ts_library(
       name = "typescript-service",
       srcs = glob(["src/**/*.ts"]),
       deps = [
           # List dependencies here
       ],
   )
   ```

3. **Install any necessary dependencies** using Bazel, Go, Yarn, or Cargo, depending on the language of the package.

### Running Tests

Tests for all services can be executed via Bazel:

```bash
bazel test //...
```

You can also run tests for specific services:

- **TypeScript**:

  ```bash
  bazel test //packages/typescript-service:test
  ```

- **Go**:

  ```bash
  bazel test //packages/go-service:test
  ```

- **Rust**:

  ```bash
  bazel test //packages/rust-service:test
  ```

### Linting & Formatting

For TypeScript, you can use [ESLint](https://eslint.org/) for linting, and [Prettier](https://prettier.io/) for code formatting. You can add these configurations in each package’s `package.json` or use the `tslint` command via Bazel.

For Go, use `golangci-lint` for linting, and for Rust, use `clippy` for Rust-specific linting and formatting.

### Building and Deploying

Bazel provides support for continuous integration and deployment pipelines. To deploy the monorepo, create a deployment pipeline using Bazel’s build and test functionality.

For example, you could trigger deployment after tests pass and builds are successful, either by deploying to Docker containers or cloud services (AWS, GCP, etc.).

## Contributing

We welcome contributions to Kimono! To contribute:

1. Fork the repository and clone it locally.
2. Create a new branch for your feature or bug fix.
3. Install dependencies and build the project using Bazel.
4. Run tests and ensure everything works as expected.
5. Open a pull request with a clear description of your changes.

## License

Kimono is licensed under the [MIT License](LICENSE).

---

If you have any questions or issues, feel free to open an issue on the repository or reach out to the maintainers.
