# Ansible-Install


1. Update sistem terlebih dulu

       sudo apt update
       sudo apt upgrade

   ![image](https://user-images.githubusercontent.com/40049149/191546792-a9ee4267-e288-425e-8168-ffb778bc2f65.png)

2. Jalankan perintah berikut untuk install ansible

       sudo apt install ansible

   ![image](https://user-images.githubusercontent.com/40049149/191548304-382ebd5e-18ce-4135-a855-2cb54fcf7fb4.png)

3. Buat file inventory

       [devops]
       103.189.234.3 ansible_user=menther

   ![image](https://user-images.githubusercontent.com/40049149/191549807-8b31a94b-df8f-4bfa-854c-004b77e16a9d.png)

4. Buat file berisi id_rsa key

       nano a.pem

   ![image](https://user-images.githubusercontent.com/40049149/191550579-75977770-819e-48c1-a237-a4a6e9739473.png)

5. Set file akses hanya bisa di baca

       sudo chmod 400 a.pem

   ![image](https://user-images.githubusercontent.com/40049149/191551032-b0b91e1f-b01e-47cc-aa56-26291687b2a1.png)

6. Buat file ansible.cfg 

       sudo nano ansible.cfg
       
       [defaults]
       inventory = inventory
       private_key_file = a.pem

   ![image](https://user-images.githubusercontent.com/40049149/191552622-043f9fcc-72e5-4e0d-92da-71577c05fccc.png)

7. Cek koneksi

       ansible all -m ping
       
       ansible all -i inventory -m ping

   ![image](https://user-images.githubusercontent.com/40049149/191553113-16e45375-28e4-469e-8bc9-0f6211b09060.png)

## Create user

1. Buat file user.yaml

       nano user.yaml

2. Paste ke file user.yaml

       - hosts: all
         become: true
         gather_facts: true
         vars:
         - username: frenky
         - password: $6$PbnDyrfjFsL8iISE$xuEhLnDkapwGxNBoxAJdSU5Zeno9NukfL5n9CbUderOe8.UgqitNkyAmDEAtl788rmFpNS4UCxNzBhK2KYv5T.
         tasks:

         - name: Create user
           user:
             name: "{{username}}"
             password: "{{password}}"
             groups: sudo
             state: present
             shell: /bin/bash
             system: no
             createhome: yes
             home: /home/{{username}}

         - name: Change Password Authentication
           lineinfile:
             path: /etc/ssh/sshd_config
             regexp: 'PasswordAuthentication no'
             line: PasswordAuthentication yes

         - name: Restart SSH Service
           service:
             name: ssh
             state: restarted

   ![image](https://user-images.githubusercontent.com/40049149/191566414-4d1a6cd1-3919-4a69-8588-4ae3e96143cf.png)

3. Encrypt password jalankan perintah berikut dan masukan password

       mkpasswd --method=sha-512

   ![image](https://user-images.githubusercontent.com/40049149/191561866-4bb68199-610d-42a4-9f49-db7581f91cd6.png)

4. Jalankan

       sudo ansible-playbook user.yaml

   ![image](https://user-images.githubusercontent.com/40049149/191567077-5a8e5ce0-1af2-403c-ac9e-f8fe38f60f47.png)

## Update Sistem

1. Buat file update.yaml

       - hosts: all
         become: true
         tasks:
         - name: update
           apt:
             update_cache: yes
             upgrade: dist

   ![image](https://user-images.githubusercontent.com/40049149/191568578-1f23db57-dbd3-4616-9c99-a2bf5f9aa21f.png)

2. Jalankan 

       sudo ansible-playbook update.yaml

   ![image](https://user-images.githubusercontent.com/40049149/191568768-cd4f6d73-641e-476c-b284-65131dda754b.png)

## Nginx Install

1. Buat file nginx.yaml
       
       - hosts: all
         become: true
         tasks:
           - name: Update system
             apt:
               update_cache: yes
               upgrade: dist

           - name: Install Nginx
             apt:
               name: nginx
               state: present
               update_cache: yes

   ![image](https://user-images.githubusercontent.com/40049149/191571182-84303e8e-6027-4203-bfcc-21ee6e46872d.png)

2. Jalankan

       sudo ansible-playbook nginx.yaml

   ![image](https://user-images.githubusercontent.com/40049149/191571602-dc0d7f7d-babe-4f4e-a619-464b4a81487c.png)

   ![image](https://user-images.githubusercontent.com/40049149/191571694-c18cd462-578e-48bf-a850-51bced2b9d10.png)

## Docker Install

1. Buat file docker.yaml

       - hosts: all
         become: true
         tasks:
           - name: "Update"
             apt:
               update_cache: yes

           - name: "Install docker dependencies"
             apt:
               name:
               - ca-certificates
               - curl
               - gnupg
               - lsb-release

           - name: "Add docker GPG key"
             apt_key:
               url: https://download.docker.com/linux/ubuntu/gpg
               state: present

           - name: "Add docker repository"
             apt_repository:
               repo: deb https://download.docker.com/linux/ubuntu focal stable
               state: present

           - name: "Install docker engine"
             apt:
               name:
               - docker-ce
               - docker-ce-cli
               - containerd.io
               - docker-compose-plugin

           # - name: "Bash Execusions"
           #   shell: exec bash

           - name: "Set docker comand without sudo"
             shell: sudo usermod -aG docker menther

2. Jalankan

       sudo ansible-playbook docker.yaml

   ![Screenshot from 2022-09-22 00-57-14](https://user-images.githubusercontent.com/40049149/191577634-7ceb63bf-a54e-49e9-8c63-dce3a05f7364.png)

   ![image](https://user-images.githubusercontent.com/40049149/191639960-587272b8-8613-4133-8037-eb98eefd794e.png)



















