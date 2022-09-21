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



























































