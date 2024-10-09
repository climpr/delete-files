# Delete files

This action deletes files based on an input file containing file paths and file hashes.

## How to use this action

To use this action, implement the steps as shown below in your workflow.

It requires the repository to be checked out first.

```yaml
# ...
steps:
  - name: Checkout repository
    uses: actions/checkout@v4

  - name: Delete files
    uses: climpr/delete-files@v1
    with:
      configuration-path: delete-files.config.json
# ...
```

## Prerequisites

This action requires you to create a configuration file.
This file is used to specify which files to delete and which files to skip.

To create this file, start with an empty file and add the following content:

```json
{
  "$schema": "https://raw.githubusercontent.com/climpr/climpr-schemas/main/schemas/v1.0.0/delete-files/config.json#"
}
```

This will ensure you have auto-complete and validation for the configuration file.

### Configuration file schema

An example file looks like this:

> [!TIP]
> You can use either the `hash` property for a single file hash, or `hashes` property for a list of file hashes. This can be useful if you want to support multiple versions of the same file.

```jsonc
{
  "$schema": "https://raw.githubusercontent.com/climpr/climpr-schemas/main/schemas/v1.0.0/delete-files/config.json#",
  "directoriesToExclude": ["\\.git"], // A list of directory relative paths that should be excluded from processing. The '.git' directory is always excluded.
  "filesToDelete": [
    {
      "path": "string", // Relative path to file
      "hash": "3A888546831AE05A0EC1D040DE396262284E4B4FC0066A00D56016BF3955C90E" // File hash
    },
    {
      "path": "string", // Relative path to file
      "hashes": [
        // List of file hashes
        "3A888546831AE05A0EC1D040DE396262284E4B4FC0066A00D56016BF3955C90E"
      ]
    }
  ]
}
```

### Generating file hashes

Generating file hashes is done by using the `Get-FileHash` command in PowerShell.

#### Example

In this example, the file hash is the string under `Hash` below.

```powershell
Get-FileHash "./directory/file.txt"

# Result
# Algorithm       Hash                                                                   Path
# ---------       ----                                                                   ----
# SHA256          E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855       <root-path>/directory/file.txt
```

## Parameters

### Required

#### `configuration-path`

The path to the configuration file containing the file paths and hash values.

### Optional

#### `path`

The root directory from where to enumerate files.

**Default:** `'.'`

#### `depth`

This parameter specifies how many levels to enumerate in the directory tree.

**Default:** `10`

#### `delete-empty-directories`

Specifying this will delete empty directories. This is usually not necessary when working with git, hence the default is set to `false`.

**Default:** `false`
