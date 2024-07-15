# raspberry-ssh-connection

Pierwszy projekt z raspberry pi 4, w którym raspberry zostanie wykorzystane do połaczenia się z komputerem przez ssh.

# 1. sprawdzenie adresu IP za pomocą terminala na Raspberry Pi

Uruchomić komendę
```bash
hostname -I
```

2. Upewnienie się, że SSH jest włączone na Raspberry Pi

Sprawdzenie statusu usługi SSH:
```bash
sudo systemctl status ssh
```

Jeśli SSH jest wyłączone, włącz je:
```bash
sudo systemctl start ssh
sudo systemctl enable ssh
```

Sprawdź, czy SSH jest poprawnie uruchomione:
```bash
sudo systemctl status ssh
```

3. Sprawdzanie ustawień zapory (firewall) na Raspberry Pi

Sprawdź, czy zapora nie blokuje portu 22:
```bash
sudo ufw status
```

Jeśli zapora jest włączona i blokuje port 22, otwórz go:
```bash
sudo ufw allow 22
sudo ufw reload
sudo ufw status
```

4. Przygotowanie klucza SSH na komputerze z Ubuntu do łączenia się z raspberry

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa_raspberry
```

Skopiowanie klucza publicznego na Raspberry Pi
```bash
ssh-copy-id -i ~/.ssh/id_rsa_raspberry nazwa_użytkownika@ip
```

5. Konfiguracja połączenia SSH z użyciem klucza
```bash
nano ~/.ssh/config
```

```
Host raspberrypi4
    HostName ip
    User nazw_użytkownika
    IdentityFile ~/.ssh/id_rsa_raspberry
```


