# learn-ansible


# - name: Installing Nginx Server
  ansible.builtin.yum:
    name: nginx
    state: latest

- name: Remove directory
  ansible.builtin.file:
    path: /usr/share/nginx/html
    state: absent

- name: Create directory
  ansible.builtin.file:
    path: /usr/share/nginx/html
    state: directory

- name: Download and extract frontend content
  ansible.builtin.unarchive:
    src: https://roboshop-artifacts.s3.amazonaws.com/frontend.zip
    dest: /usr/share/nginx/html
    remote_src: yes

- name: Copy roboshop configuration
  ansible.builtin.copy:
    src: roboshop.conf
    dest: /etc/nginx/default.d/roboshop.conf

- name: Start Nginx Service
  ansible.builtin.systemd:
    state: restarted
    name: nginx
    enabled: true


## mongodb


- name: Copy MongoDB Repo file
  ansible.builtin.copy:
    src: mongodb.repo
    dest: /etc/yum.repos.d/mongodb.repo
- name: Install MongoDB
  ansible.builtin.yum:
    name: mongodb-org
    state: latest

- name: Update MongoDB Listen Address
  ansible.builtin.replace: 
    path: /etc/mongod.conf
    regexp: '127.0.0.1'
    replace: '0.0.0.0'

- name: Start MongoDB Service
  ansible.builtin.systemd:
    name: mongod
    enabled: true

    nodejs.yml


 #name: Configuring NodeJS Repos
  ansible.builtin.shell: curl -sL https://rpm.nodesource.com/setup_lts.x | bash

- name: Installed NodeJS
  ansible.builtin.yum:
    name: nodejs
    state: installed