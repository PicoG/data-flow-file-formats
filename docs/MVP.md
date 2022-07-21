# MVP - Load and Save a (Simple) VI to G YAML

## Goals
### Demonstrate Feasibility of YAML as a format for G.
- Create (load) a VI from a *.vi.yaml file
- Export (save) a VI to a *.vi.yaml file

## What's Needed
The following items are needed to complete the objectives.
### G YAML Schema (Define YAML Schema for G)
- See [vi_lint.json](../yml/vi_lint.json) for the current prototype
- **JSON Schema** will be used for implementing the schema.
- Allows validation and easier editing of *.g.yaml in Visual Studio Code.
- Can be served up to users via a URL (vscode will pull from the server)
### Generate VI from YAML (Open VI saved as .g.yaml)
- Written in LabVIEW (see [nicolas_bats_lib_yaml_parser](https://www.vipm.io/package/nicolas_bats_lib_yaml_parser/))
- Reads a .g.yaml file into LabVIEW
- Generates a new LabVIEW VI and G Objects
### Export VI to YAML (Save a LabVIEW VI to .g.yaml format)
- Written in LabVIEW
- Traverse a VI for G Objects and Properties
- Save to G YAML

## Non-Goals (Ideas for the Future)
The following are ideas for other items that would be useful.

### Schema Auto-Generator for G YAML
Auto-generate YAML Schema from LabVIEW
What's Needed
- **Define Ruleset for Name Mappings**: define a ruleset for mapping LabVIEW object and property names to YAML. e.g. "Front Panel" --> "front_panel"
- Write the code

### Save Folder of VIs to G YAML
For each VI in a folder, export the VI as a *.g.yaml file right next to the original VI.

Benefits:
- Could allow committing VIs as YAML to git repo in a diffable text file format.
- Could run this utility automatically on push to git repo (pre-commit hook on developer's machine)
- Could run this in the cloud

### Robust YAML libary for LabVIEW
- Not sure how robust the [nicolas_bats_lib_yaml_parser](https://www.vipm.io/package/nicolas_bats_lib_yaml_parser/) library is.
- It would be great to have a more robust library for reading/writing YAML as the G YAML format and tools evolve.
