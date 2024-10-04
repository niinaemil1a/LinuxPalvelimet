# LinuxPalvelimet
Homework

`sudo apt-get update` `sudo apt-get -y dist upgrade`
`sudo apt-get install`
`pwd` Print working directory
`cd` ja `cd ..` Liikkuminen hakemistossa
`ls` Näyttää tiedostot
`ls -la`
`whoami`
`mkdir kansio/` Luo kansio
`mv vanhanimi uusinimi` Kansion uudelleen nimeäminen
`cp vanha uusi` Kansion tai tiedoston kopiointia
`cp -r alkuperäinen uusi` Kansion kopiointi uuteen sijaintiin?
`rm poistettava`
`rm -r kansio` Poistaa kansion ja alakansiot sisältöineen
`rmdir kansio` Poistaa kansion

Tulimuuri:  
`sudo apt-get -y install ufw`
`sudo ufw allow 80/tcp` (aukko http:lle)
`sudo ufw allow 22/tcp` (aukko ssh:lle)
`sudo ufw enable`
`ufw status`

APT-GET:
`sudo apt-get install micro`
`sudo apt-get -y install bash-completion`
`sudo apt-get install pwgen` Password generator ja `pwgen -s 20 1 #`
`sudo editor=micro` ja plugareita esim. `ctrl + E` ja `set colorscheme simple`
`sudo apt install wget` ?

SSH:
`sudo apt-get -y install ssh`
`systemctl status ssh`
`sudo systemctl start ssh`
`sudo ufw allow ssh` `sudo ufw enable` `sudo ufw status`
`ssh käyttäjänimi@iposoite/localhost` 
`exit` 
Julkinen avain:
`ssh-keygen` enterx3
`ssh-copy-id tunnus@iposoite` 
`sudoedit /etc/ssh/sshd_config` salasanakirjautuminen pois päältä, muokkaa tiedostoon "PasswordAuthentication No"

`sudo adduser uusikäyttäjä` `sudo adduser uusikäyttäjä sudo` uusi käyttäjä sudo ryhmään
Testaa, että kirjautuminen ssh:lla onnistuu.
`sudo usermod -- lock root` Lukitse root
`sudoedit /etc/ssh/sshd_config` muuta PermitRootLogin No

Oikeudet:
`sudo chmod ugo+rx kansio` antaa read+execute oikeudet kansiolle
`ls -l /home/käyttäjä` näyttää hakemiston oikeudet

Lokit:
`sudo tail -f /var/log/apache2/access.log`
`sudo tail -1 /var/log/apache2/access.log`
`sudo tail /var/log/aoache2/error.log`

Apache:
`sudo apt-get -y install apache2`
`sudo systemctl start apache2` Uudelleenkäynnistys
`echo "See you at niinamatela.me"|sudo tee /var/html/index.html`

`sudo a2enmod userdir` Käyttäjäkansioiden salliminen
`mkdir -p home/niinam/publicsites/hattu.example.com/` tai
`mkdir public_html` kotihakemistoon, omalle nimelle, sinne `micro index.html`
`<!doctype html>
  <html lang="en">
  <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Testisivu</title>
  </head>
  <body>
      <p>Olen testisivu</p>
  </body>
  </html>`
  Selaimesta "http://localhost/~user"  
  
 Palomuuri apachelle
 `sudo service apache2 stop` Serverin pysäytys

 `curl localhost`
 `sudoedit /etc/hosts` tiedostoon lisätään domainnimet ja ohjaus localhostiin

Name Based Virtual Host:
`sudoedit /etc/apache2/sites-available/hattu.example.com.conf`:  
  `<VirtualHost *:80>
ServerName hattu.example.com
ServerAlias www.hattu.example.com
    DocumentRoot /home/niinam/publicsites/hattu.example.com
    <Directory /home/niinam/publicsites/hattu.example.com>
      Require all granted
   </Directory>
   </VirtualHost>`  
`sudo a2ensite hattu.example.conf` Ottaa käyttöön ko. conf-tiedoston
`sudo a2dissite hattu.example.conf` Poistaa käytöstä ko. conf-tiedoston
`sudo a2dissite 000-default.conf` Poistaa käytöstä default conf-tiedoston
`sudo systemctl restart apache2`  

Komennot usealle käyttäjälle:  
