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


Instrukcja: Konfiguracja połączenia Wi-Fi na Raspberry Pi z poziomu terminala na innym komputerze
Krok 1: Przygotowanie karty SD
Wyjmij kartę SD z Raspberry Pi i włóż ją do komputera.

Zamontuj kartę SD – w większości systemów Linux karta SD powinna automatycznie się zamontować, tworząc dwie partycje: boot (lub bootfs) i rootfs.

Krok 2: Konfiguracja połączenia Wi-Fi przy użyciu NetworkManager
Przejdź do katalogu system-connections na zamontowanej partycji rootfs:

bash
Skopiuj kod
cd /media/username/rootfs/etc/NetworkManager/system-connections/
Utwórz nowy plik .nmconnection:

Utwórz plik konfiguracyjny dla sieci Wi-Fi, której chcesz używać. Na przykład:

bash
Skopiuj kod
sudo nano 'TwojaSiecWiFi.nmconnection'
Dodaj konfigurację sieci:

Skonfiguruj plik, używając poniższego szablonu. Zastąp TwojaSiecWiFi, twoje-uuid, twoje_haslo odpowiednimi wartościami:

plaintext
Skopiuj kod
[connection]
id=TwojaSiecWiFi
uuid=twoje-uuid
type=wifi
interface-name=wlan0

[wifi]
mode=infrastructure
ssid=TwojaSiecWiFi

[wifi-security]
auth-alg=open
key-mgmt=wpa-psk
psk=twoje_haslo

[ipv4]
method=auto

[ipv6]
addr-gen-mode=default
method=auto
Uwaga: UUID to unikalny identyfikator, który możesz wygenerować za pomocą komendy uuidgen w terminalu:

bash
Skopiuj kod
uuidgen
Zapisz plik i zamknij edytor:

Po wprowadzeniu zmian, zapisz plik (Ctrl + X, Y, Enter).

Ustaw uprawnienia dla pliku:

Upewnij się, że plik ma odpowiednie uprawnienia:

bash
Skopiuj kod
sudo chmod 600 'TwojaSiecWiFi.nmconnection'
Krok 3: Odmontowanie karty SD i uruchomienie Raspberry Pi
Odmontuj kartę SD:

bash
Skopiuj kod
sudo umount /media/username/rootfs
sudo umount /media/username/bootfs
Włóż kartę SD z powrotem do Raspberry Pi i uruchom urządzenie.

Krok 4: Sprawdzenie połączenia
