name: Ansible CI/CD

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v2

      - name: Install Ansible
        run: |
          sudo apt update
          sudo apt install -y ansible

      - name: Setup SSH
        run: |
          mkdir -p ~/.ssh
          echo "${{ secrets.ANSIBLE_PRIVATE_KEY }}" > ~/.ssh/id_rsa
          chmod 600 ~/.ssh/id_rsa
          ssh-keyscan -H your.server.ip >> ~/.ssh/known_hosts

      - name: Run Ansible
        run: |
          ansible-playbook -i inventory playbook.yml
