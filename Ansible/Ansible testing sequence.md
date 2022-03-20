1. yamllint
	1. Just to start with `yamllint .` then you can create a .yamllint file to bypass some syntax. yamllint only check if the yaml file is correct, not ansible. Here is a common example
```
---
extends: default
rules:
	truthy:
		allowed-values:
			- 'true'
			- 'false'
			- 'yes'
			- 'no'
```
2. `ansible-playbook playbook.yml --syntax-check` This only check if it can find all imports and playbooks. Checks if ansible compile your code.
3. `ansible-lint playbook.yml` Helps stay within best practices and avoid pit falls.
4. molecule test (integration)
5. ansible-playbook --check (against prod)
6. Parallel infastructure)