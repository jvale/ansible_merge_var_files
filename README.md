# merge_var_files
[![Build Status](https://travis-ci.org/jvale/ansible_merge_vars.svg?branch=master)](https://travis-ci.org/jvale/ansible_merge_vars)

Ansible [action plugin](https://docs.ansible.com/ansible/latest/plugins/action.html) that, given a list of files and/or paths, recursively merges the contents of those files.

Heavily derived from Ansible's [include_vars](https://docs.ansible.com/ansible/2.8/modules/include_vars_module.html) action plugin (that disguises itself as a module), so all its rules and functionality should apply seamlessly, with the exception that the parameter `file` (string) is replaced by `files` (list).

## Merge strategy
When merging, the following strategy is applied:
 * non-existing items are added
 * if data types differ, the new item overrides the previous one
 * dictionaries are merged recursively
 * all other values are overridden by the newer item

## Testing
Tests are executed using `tox`, and the following combinations are tested:

| Python    |    Ansible    |
|-----------|---------------|
| 2.7       | 2.8 (latest)  |
| 2.7       | 2.9 (latest)  |
| 3.7       | 2.8 (latest)  |
| 3.7       | 2.9 (latest)  |


## Example usage
```yaml
- name: load and merge vars from all files
  merge_var_files:
    name: my_merged_data
    files:
      - default.yml
      - '{{ ansible_os_family }}.yml'
      - '{{ ansible_os_distribution }}.yml'
```
