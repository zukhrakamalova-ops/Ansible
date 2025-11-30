# Ansible Homework - Netology

## Inventory файл
`inventory/inventory.ini`:
```ini
[netology-ml]
localhost ansible_connection=local
---
- name: Netology Ansible Homework
  hosts: netology-ml
  become: yes
  vars:
    packages:
      - net-tools
      - git
      - tree
      - htop
      - mc
      - vim
    user_groups:
      - name: devops_1
        user: devops_1
        home: /home/devops_1
      - name: test_1  
        user: test_1
        home: /home/test_1

  tasks:
    - name: Test connection to hosts
      ansible.builtin.ping:

    - name: Install required packages (skip update)
      package:
        name: "{{ packages }}"
        state: present

    - name: Copy test.txt file
      copy:
        src: files/test.txt
        dest: /tmp/test.txt
        owner: root
        group: root
        mode: '0644'

    - name: Create groups
      group:
        name: "{{ item.name }}"
        state: present
      loop: "{{ user_groups }}"

    - name: Create users
      user:
        name: "{{ item.user }}"
        group: "{{ item.name }}"
        home: "{{ item.home }}"
        create_home: yes
        shell: /bin/bash
      loop: "{{ user_groups }}"

    - name: Create home directories
      file:
        path: "{{ item.home }}"
        state: directory
        owner: "{{ item.user }}"
        group: "{{ item.name }}"
        mode: '0755'
      loop: "{{ user_groups }}"
[WARNING]: Invalid characters were found in group names but not replaced, use -vvvv to see details

PLAY [Netology Ansible Homework] ***********************************************

TASK [Gathering Facts] *********************************************************
ok: [localhost]

TASK [Test connection to hosts] ************************************************
ok: [localhost]

TASK [Install required packages (skip update)] *********************************
ok: [localhost]

TASK [Copy test.txt file] ******************************************************
ok: [localhost]

TASK [Create groups] ***********************************************************
ok: [localhost] => (item={'name': 'devops_1', 'user': 'devops_1', 'home': '/home/devops_1'})
ok: [localhost] => (item={'name': 'test_1', 'user': 'test_1', 'home': '/home/test_1'})

TASK [Create users] ************************************************************
ok: [localhost] => (item={'name': 'devops_1', 'user': 'devops_1', 'home': '/home/devops_1'})
ok: [localhost] => (item={'name': 'test_1', 'user': 'test_1', 'home': '/home/test_1'})

TASK [Create home directories] *************************************************
ok: [localhost] => (item={'name': 'devops_1', 'user': 'devops_1', 'home': '/home/devops_1'})
ok: [localhost] => (item={'name': 'test_1', 'user': 'test_1', 'home': '/home/test_1'})

PLAY RECAP *********************************************************************
localhost                  : ok=7    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
=== ПРОВЕРКА УСТАНОВЛЕННЫХ ПАКЕТОВ ===
/usr/bin/git
/usr/bin/tree
/usr/bin/htop
/usr/bin/mc
/usr/bin/vim

=== ПРОВЕРКА СКОПИРОВАННОГО ФАЙЛА ===
-rw-r--r-- 1 root root 65 Nov 30 17:00 /tmp/test.txt
Содержимое файла:
This is a test file for Ansible homework
Created by: Netology ML
Date: 2024

=== ПРОВЕРКА СОЗДАННЫХ ПОЛЬЗОВАТЕЛЕЙ ===
uid=1001(devops_1) gid=1001(devops_1) groups=1001(devops_1)
uid=1002(test_1) gid=1002(test_1) groups=1002(test_1)
devops_1:x:1001:1001::/home/devops_1:/bin/bash
test_1:x:1002:1002::/home/test_1:/bin/bash

=== ПРОВЕРКА СОЗДАННЫХ ГРУПП ===
devops_1:x:1001:
test_1:x:1002:

=== ПРОВЕРКА ДОМАШНИХ ДИРЕКТОРИЙ ===
Директория devops_1:
total 8
drwxr-xr-x 2 devops_1 devops_1 4096 Nov 30 17:00 .
drwxr-xr-x 4 root     root     4096 Nov 30 17:00 ..
-rw-r--r-- 1 devops_1 devops_1  220 Nov 30 17:00 .bash_logout
-rw-r--r-- 1 devops_1 devops_1 3771 Nov 30 17:00 .bashrc
-rw-r--r-- 1 devops_1 devops_1  807 Nov 30 17:00 .profile
Директория test_1:
total 8
drwxr-xr-x 2 test_1 test_1 4096 Nov 30 17:00 .
drwxr-xr-x 4 root   root   4096 Nov 30 17:00 ..
-rw-r--r-- 1 test_1 test_1  220 Nov 30 17:00 .bash_logout
-rw-r--r-- 1 test_1 test_1 3771 Nov 30 17:00 .bashrc
-rw-r--r-- 1 test_1 test_1  807 Nov 30 17:00 .profile
