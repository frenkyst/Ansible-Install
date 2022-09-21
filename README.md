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
         gather_facts: false
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














































