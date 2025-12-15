[![YouTube Video](https://img.youtube.com/vi/RRVQuJPllUs/maxresdefault.jpg)](https://www.youtube.com/watch?v=RRVQuJPllUs&t=7s)


✅ 1. ARXITEKTURA (ENG TO‘G‘RI UCHTA USUL)
Variant A — VirtualBoxda ikki VM

Ubuntu Server (IDS/IPS) — brigded adapter yoki host-only

Kali Linux (Attacker) — xuddi shu networkga ulangan

Bu eng qulay va eng ko‘p ishlatiladigan usul.

Variant B — Ubuntu Serverni host, Kali-ni VM

Agar Ubuntu Serverni real mashinaga o‘rnatgan bo‘lsang, ustiga VirtualBox orqali Kali qo‘ya olasan. Ammo bu kam qo‘llanadi.

Variant C — Ikkalasi ham Dockerda

Bu murakkabroq, yangi boshlovchi uchun tavsiya emas.

✅ 2. UBUNTU SERVERGA IDS/IPS O‘RNATISH

Eng qulay va kuchlisi — Suricata
(IPS + IDS + Firewallga integratsiya qiladi)

Suricata o‘rnatish
sudo apt update
sudo apt install suricata -y

Status tekshirish:
sudo systemctl status suricata

✅ 3. SURICATA IPS REJIMINI YOQISH

Default Suricata faqat IDS (kuzatuv) bo‘ladi. IPS qilish uchun:

1. NFQUEUE yoqiladi
sudo apt install iptables-persistent

2. IPS qoidalarini qo‘yish
sudo iptables -I INPUT -j NFQUEUE --queue-num 0
sudo iptables -I OUTPUT -j NFQUEUE --queue-num 0

3. Suricata configni o‘zgartirish
sudo nano /etc/suricata/suricata.yaml


Bundan:

mode: "af-packet"


shuni quyidagi bilan almashtir:

mode: "nfqueue"


Saqlab qayta ishga tushir:

sudo systemctl restart suricata

✅ 4. TEST HUJUMI (KALI LINUXDAN)

Masalan, ping flood:

hping3 -1 -d 120 -w 64 -p 80 --flood <ubuntu-ip>


Yoki ssh brute-force:

hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://<ubuntu-ip>


Agar IPS qoidasi bo‘lsa — Kali IP bloklanadi.

✅ 5. BLOKLANGAN IP’NI QO‘LDA KO‘RISH

Agar Fail2ban ishlatyotgan bo‘lsa:

sudo fail2ban-client status sshd

Bloklangan IP ro‘yxati:
sudo iptables -L -n

✅ 6. IP-NI BLOKDAN OCHISH

Agar Suricata NFQUEUE bilan blok qilayotgan bo‘lsa, qayta qoida qo‘yish kerak.

Agar Fail2ban bo‘lsa:

sudo fail2ban-client set sshd unbanip <ip>


Agar iptables bo‘lsa:

sudo iptables -D INPUT -s <ip> -j DROP

💯 7. QISQASI — HA, QILSA BO‘LADI:
Amaliyot	Mavjudmi?
Ubuntu Serverga IDS/IPS o‘rnatish	✅
Kali Linuxdan hujum qilish	✅
Hujumni aniqlash	✅
IP avtomatik bloklash	✅
Blokdan qo‘lda chiqarish	✅