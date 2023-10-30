# 6. praktikum
6. praktikumis õppisin, kuidas hallata protsesse unix süsteemis. Sain teada, kuidas leida protsesse üles ning mis käskude abil saab neid peatada, magama panna või tõsta tagataustale. Lisaks õppisin, kuidas pipe-ida commande ning kust leida protsesside sisendeid ja väljundeid.
   
**Ül. 3** Käsk: ps -axu | grep daemon | tr -s "" | cut -68-

**Ül. 4** Käsk: ip a | grep inet | sed '3!d' | cut -d '/' -f 1 | tr -d inet > ipaddress.txt
<img width="960" alt="Screenshot 2023-10-30 182929" src="https://github.com/Siim0u/ops-steemid/assets/112852891/27bd5f83-16c1-41ae-a7a8-4db9c3f73837">
<img width="960" alt="Screenshot 2023-10-30 174842" src="https://github.com/Siim0u/ops-steemid/assets/112852891/9c7ef9bc-557a-4834-b827-cd04392af13a">
<img width="960" alt="Screenshot 2023-10-30 171402" src="https://github.com/Siim0u/ops-steemid/assets/112852891/808526e8-cb8d-4808-a986-85ff535ab977">
<img width="960" alt="Screenshot 2023-10-30 170433" src="https://github.com/Siim0u/ops-steemid/assets/112852891/9e924bae-a339-4b52-8fa3-9395cedd3ef4">
