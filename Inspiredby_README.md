# Technique to do powerline configuration role is inspired by this site:  https://stribny.name/blog/ansible-dev/

```yaml
---
- hosts: localhost
  become: yes
  vars:
    - user: 'pstribny'
    - dotfiles_repo: 'git@github.com:stribny/dotfiles.git'
    - ssh_key: '.ssh/id_rsa'

  tasks:
    - name: "Install system packages"
      dnf: 
        name:
          - vim 
          - bat
          - exa
          - podman
        state: latest

    - name: "Install Python packages"
      pip:
        name:
          - poetry
        state: latest

    - name: "Check out dotfiles from repository"
      git:
        repo: "{{ dotfiles_repo }}"
        dest: ./tmp-dotfiles
        accept_hostkey: yes
        force: yes
        recursive: no
        key_file: "/home/{{ user }}/{{ ssh_key }}"
      delegate_to: localhost
      become: no
      run_once: true

    - name: "Copy .vimrc"
      copy:
        src: ./tmp-dotfiles/.vimrc
        dest: "/home/{{ user }}/.vimrc"
        owner: "{{ user }}"
        group: "{{ user }}"
        mode: '0644'

    - name: "Copy .aliases"
      copy:
        src: ./tmp-dotfiles/.aliases
        dest: "/home/{{ user }}/.aliases"
        owner: "{{ user }}"
        group: "{{ user }}"
        mode: '0644'

    - name: "Load aliases in .bashrc file"
      blockinfile:
        path: "/home/{{ user }}/.bashrc"
        block: |
          if [ -f ~/.aliases ]; then
            source ~/.aliases
          fi
```