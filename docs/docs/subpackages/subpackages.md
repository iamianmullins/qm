## QM Sub-packages

The qm project is designed to provide a flexible and modular environment for managing
Quality Management (QM) software in containerized environments. One of the key features
of the qm package is its support for sub-package(s), such as the qm-dropin sub-packages.
These sub-packages are not enabled by default and are optional. However,  allow users
to easily extend or customize their QM environment by adding specific configurations,
tools, or scripts to the containerized QM ecosystem by simple installing or uninstalling
a RPM package into the system.

## Key Features of QM Sub-Packages

### Modularity

- No configuration change, no typo or distribution rebuild/update.
- Just dnf install/remove from the tradicional rpm schema.

### Customizability

- Users can easily add specific configurations to enhance or modify the behavior of their QM containers.

### Maintainability

- Sub-packages ensure that the base qm package remains untouched, allowing easy updates without breaking custom configurations.

### Simplicity

- Like qm-dropin provide a clear directory structure and templates to guide users in customizing their QM environment.
