
# Operational System Design

This document provides design guidelines for an operational weather prediction workflow using the Cylc workflow engine. While the recommendations are based on Cylc, the general concepts also apply to other workflow engines.

## Code Style Guidelines
  - **Bash:** Adhere to the [Google Style Guide](https://google.github.io/styleguide/shell.xml).
  - **Python:** Follow [PEP8](https://www.python.org/dev/peps/pep-0008/).
  - **Cylc:** Follow [Cylc style guide](https://cylc.github.io/cylc-doc/latest/html/workflow-design-guide/style-guide.html)
  - **Formatters:** Use formatter plugins in your code editors to enforce code formatting e.g. [Black](https://black.readthedocs.io/en/stable/) for python.

## Maintainability
### Cylc configuration Sharing
- Share Cylc configurations using inheritance. Follow the [Cylc guideline](https://cylc.github.io/cylc-doc/latest/html/workflow-design-guide/efficiency.html#sharing-by-inheritance)

### DRY
- Follow Do Not Repeat Yourself principle to avoid redundancy and repetition

### Directory Structure
- Use a meaningful and organised directory structure.

### Dependencies
A weather forecasting workflow can have various dependencies, including Python packages, climate data tools (CDO, NCO, ECCODES), and model components (WRF, MITgcm, WW3, etc.). Here are some best practices for managing these dependencies:
- Use a script to check out the component models from its repository (e.g. GitHub) or include it as a git submodule.
- Use an environment.yaml file for mamba/conda environment creation and package installation. Pip packages can also be specified in this file.
- Specify explicit versions for all dependencies to prevent issues caused by updates.

    
## Portability

### Follow Cylc Guidelines
- Ensure the workflow is easily portable by following the [Cylc Portable Workflows Guide](https://cylc.github.io/cylc-doc/latest/html/workflow-design-guide/portable-workflows.html).

### Testing Across Sites
- Test the workflow at least at two different sites, e.g., KAUST-Shaheen and NCM.

### Testing Within Sites
- Test the workflow in at least two settings at the same site, e.g., different projects or users within Shaheen.

### Core Logic
- Ensure core logic is not repeated during porting.

## Version control and Development Cycle
Use the organization's GitHub/Bitbucket account.

### Branching Strategy
1. Main Branches
	- `main`: This is the production-ready branch. It should always be stable and contain thoroughly tested code.
	- `develop`: This branch contains the latest delivered development changes for the next release. It’s a staging area for features and bug fixes before they are merged into the main branch.
2.	Supporting Branches
- Feature Branches: Created for new features and enhancements.
  	- Naming: feature/feature-name
	- Base: develop
	- Merge back to: develop
- Bugfix Branches: Created for bug fixes that are found in the develop branch.
  	- Naming: bugfix/bug-name
	- Base: develop
	- Merge back to: develop
- Hotfix Branches: Created for urgent fixes in the production code.
	- Naming: hotfix/hotfix-name
  	- Base: main
	- Merge back to: main and develop
- Release Branches: Created to prepare for a new production release.
  	- Naming: release/release-name
	- Base: develop
  	- Merge back to: main and develop
    
3. Workflow
- Creating a Feature Branch
	-  Branch off from develop.
	- Work on your feature, commit changes, and push to the remote repository.
- Code Review Process
	-  Open a Pull Request (PR) from your feature branch to the develop branch.
	-  Request reviews from team members.
	- Address any review comments by pushing updates to your feature branch.
	-  Once approved, merge the PR into develop.
- Merging Feature Branches
  	- Ensure the feature branch is up-to-date with develop before merging.
	- Merge the feature branch into develop using a merge commit or rebasing to maintain a clean history.
  	- Delete the feature branch after merging to keep the repository tidy.
- Release Process
  	- When the develop branch is stable and ready for a release, create a release branch.
	- Perform final testing and bug fixes on the release branch.
  	- Merge the release branch into main and tag the release.
	- Merge the release branch back into develop to ensure any changes are included in future development.
- Hotfix Process
  	- Create a hotfix branch from main.
	- Apply the necessary fix, commit, and push to the remote repository.
  	- Open a PR from the hotfix branch to main.
	- After approval, merge the hotfix branch into main and tag the hotfix release.
  	- Merge the hotfix branch back into develop to ensure the fix is included in future development.

## Documentation

### User Guide
- Provide a step-by-step procedure for installing and running the workflow, including:
  - Compilation of source code
  - Installation of external dependencies
  - Configuration settings

### Component Interface Documentation
- For each component (e.g., WRF, MITGCM, WW3), document the following:
  1. Compilation procedure, compiler options and environment
  2. Input-output file format, naming conventions, and other relevant information
  3. Input namelists and other relevant parameters
  4. Scripts used
  5. Directory structure
