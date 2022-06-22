- name: Install 'kubernetes'
  hosts: all
  become: true
  tasks:
  - name: "swap off"
    ansible.builtin.command:
      cmd: 'swapoff -a'
  - name: "change swap file"
    ansible.builtin.replace:
      path: /etc/fstab
      regexp: '^([^#].*?\sswap\s+sw\s+.*)$'
      replace: '# \1'
      backup: yes