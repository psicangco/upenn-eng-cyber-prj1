# upenn-eng-cyber-prj1
Week 13 - Project 1

For this project, the following network topology was created:

![Net_Topology](Capture.PNG)

For the deployment (as well as certain configurations), the process was automated using Ansible. Utilizing Ansible's YAML playbook, specific tasks are defined. For example, to deploy Filebeat, which was used for logging, the following playbook was used:

```yml
- name: Filebeat Deployment
  hosts: elk
  become: yes
  tasks:

    # Use curl to get .deb software package
  - name: Get Filebeat Package
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

    # Extraction/installation of the package
  - name: Install Filebeat Package
    command: dpkg -i filebeat-7.6.1-amd64.deb

    # Copy reference YAML for modification
  - name: Copy Reference YAML
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml
 
    # Enable the Filebeat Logstash module
  - name: Enable Logstash
    command: filebeat modules enable logstash

    # Initiate the Filebeat setup command
  - name: Setup filebeat
    command: filebeat setup

    # Initiate the Filebeat service
  - name: Start filebeat
    command: sudo service filebeat start
  ```

