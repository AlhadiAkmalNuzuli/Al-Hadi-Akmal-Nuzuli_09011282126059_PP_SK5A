# MPI-Bubblesort menggunakan Python
Tugas Pemrosesan Paralel MPI Bubblesort
1. Al Hadi Akmal Nuzuli       09011282126059
2. Muhammad Azrell Samudra    09011282126063
3. Hatta Efrizal              09011282126041 
4. Raihan                     09011182126023 

# Langkah-Langkah Membuat Cluster MPI pada 1 Master dan 3 Worker yaitu :
1. Siapkan 4 device laptop yang dimana 1 laptop bertindak sebagai master, 3 lainnya sebagai worker. Pastikan keempat device ini terhubung pada 1 jaringan internet yang sama.
2. Lakukan update sistem OS Linux dengan menggunakan command sudo apt update && sudo apt upgrade pada terminal Linux.
3. Lakukan konfigurasi hosts pada master dan worker dengan menggunakan command “sudo nano /etc/hosts”, setelahnya, masukkan ip master dan masing-masing worker ke dalam dokumen tersebut:

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/824b6f68-e71d-4729-a038-c1a909cc49fb)

Diatas adalah konfigurasi hosts pada master,Sedangkan konfigurasi hosts pada worker hanya  memasukan ip master dan worker sendiri.

4. Buatlah sebuah user baru. Disini Master dan ketiga worker menggunakan username yang sama, yaitu “mpiuser”. Setelahnya, barulah user berpindah ke username barunya.

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/ea9d6fef-b0e8-422a-99e6-8eadd5d9eb75)

5. Setelahnya dilakukan konfigurasi SSH dengan menginstall SSH dengan menggunakan command “sudo apt install openssh-server”

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/f41415de-9814-45ac-b30b-7c2e39b6fae3)

6. Setelah itu, master akan melakukan pengecekan ssh pada masing-masing worker dengan menggunakan command “ssh mpiuser@worker1”. Pada kasus ini, master menggunakan worker1 sebagai sampel pengujian ssh:

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/4c88b2be-23ba-461d-b8a8-9790333cdb8b)

7. Selanjutnya, master akan melakukan create keygen pada ssh dengan menggunakan command “ssh-keygen -t rsa”.

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/4f0a02d2-304b-46a5-9d65-16c20c5ddbe5)

8.Selanjutnya, ssh keygen yang telah teregenerate akan dibagikan dari master ke workerdengan menggunakan command berikut:
cd .ssh
cat id_rsa.pub | ssh <username worker>@<server worker> “mkdir .ssh; cat >> 
.ssh/authorized_keys”
Disini, master menggunakan worker2 sebagai sampel untuk membagikan ssh-keygen yang telah dibuat

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/a9a03473-eb1e-48b7-a719-d19955b3e5b9)

9. Selanjutnya, master dan worker akan membuat sebuah direktori yang akan dibagikan satusama-lain. Disini, master dan worker membuat direktori yang memiliki nama yang sama, 
yaitu “cloud”:

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/d83d013c-57d1-41b6-b402-2040af35da41)

10. Langkah selanjutnya ialah master menginstall NFS server dengan menggunakan command  “sudo apt install nfs-kernel-server”.Sedangkan pada worker masing-masing akan dilakukan installasi nfs common.

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/6ecce75e-54fd-4637-b462-85f1fb7bbcb1)

11.Lalu, master akan melakukan konfigurasi pada /etc/exports untuk memastikan bahwa folder cloud dapat dibagikan kepada seluruh worker dengan menggunakan command “sudo 
nano /etc/exports”. Setelah itu, masukkan perintah <Alamat direktori master yang ingin dibagikan> *(rw,sync,no_root_squash,no_subtree_check)

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/3e894166-9dea-420a-a50e-5b9247f9f2c0)

12.Selanjutnya, master memasukkan command “sudo exportfs -a” dan “sudo systemctl restartkernel-server”

13.Pada Worker, mereka akan melakukan mounting kepada direktori cloud milik master,Untuk memastikan bahwa mounting telah berjalan, maka dapat digunakan perintah “df”:

14. Selanjutnya, worker dan master akan menginstall MPI dengan menggunakan command “sudo apt install openmpi-bin libopenmpi-dev”

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/7d0ae81f-308d-4a80-9ffe-89fdc54be5e0)

15. Selanjutnya, master dan worker akan menginstall python dan mpi4py

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/c5e04f05-a24c-4995-aa02-936c478d0b93)

16. Lalu, master akan membuat program bubblesort pada direktori cloud dengan menggunakan perintah “sudo nano /cloud/bubblesort.py”

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/e81a3fda-1572-48a9-b52a-5c00474b2ee7)

17. Selanjutnya, file bubblesort.py akan dijalankan oleh master dengan menggunakan fungsi mpirun:

![image](https://github.com/AlhadiAkmalNuzuli/Al-Hadi-Akmal-Nuzuli_09011282126059_PP_SK5A/assets/150991839/fe1ed80e-cf95-4b0b-bdcf-300652c989b9)



