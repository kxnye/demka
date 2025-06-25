# demka

![image](https://github.com/user-attachments/assets/4ac2c6e0-c0c0-4773-8940-aa9368388615)

![image](https://github.com/user-attachments/assets/416bc84b-ab63-46f2-a5f7-81c65b941301)

![image](https://github.com/user-attachments/assets/d0ea4009-a5c5-470a-907b-cfbd5b5d4583)

![image](https://github.com/user-attachments/assets/90aeabf4-44c6-4758-8449-2ea2190a947c)

![image](https://github.com/user-attachments/assets/ff430dcb-951e-4715-a4a3-807909358df5)

![image](https://github.com/user-attachments/assets/6bd43bef-8e86-4344-9dd1-ef9f4bb791b0)

![image](https://github.com/user-attachments/assets/2b29e2d7-00be-46b6-a30a-aaf8ea15bcca)

![image](https://github.com/user-attachments/assets/1e3951c1-8435-4e73-ac28-2233fb39be3e)

![image](https://github.com/user-attachments/assets/b2967a64-a54e-4812-bfcb-5b2dce212486)

![image](https://github.com/user-attachments/assets/48eab446-ddf4-4b53-9b1c-7efcd78ec6a5)



//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////

interface global

ip add 172.16.4.2/24

service-instance global

encapsulation untagged

connect ip int global

ip route 0.0.0.0.0/0 172.16.4.1


int lan

ip add 10.10.10.1/24

service-instance lan

encapsulation untagged

connect ip int lan


//////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


