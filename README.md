# ansible-git

This role can be used to do a `git pull` and `git push` within a playbook. Below are the variables and sample playbook. 

| Variable | Defaults | Comments |
|-|-|-|
| `git_url` | | (required) The URL of the git repository in the form https://server.com/namespace/repo.git |
| `git_token` | | (required) Tower custom credential required |
| `git_email` | ansible_git@ansible.com | The email for git to use for commits |
| `git_username` | ansible_git | The username for git to use for commits |
| `git_branch` | master | Branch in the repository |
| `git_msg` | 'update files with ansible' | Git commit message |
| `git_remove_local` | false | Remove local copy of repository |
| `base_working_dir` |  | (required) The base working path for git repo operations

## Example
```
---
- hosts: localhost
  vars:
   git_url: 'https://github.com/heatmiser/ansible-git.git'
   git_token: "{{ lookup('env', 'MY_PA_TOKEN') }}"

  tasks:
  - name: git pull
    include_role:
      name: ansible-git
      tasks_from: pull

  - name: create file in repo
    copy:
      dest: ansible-git/time.yml
      content: "{{ ansible_date_time | to_nice_yaml}}"

  - name: git push
    include_role:
      name: ansible-git
      tasks_from: push
```

**Tested:**
git 1.8.3/2.15.1 
