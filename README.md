# LinuxPalvelimet Homework  

`sudo apt-get update` `sudo apt-get -y dist-upgrade`  
`sudo apt-get install`  
`pwd` Print working directory  
`cd` ja `cd ..` Liikkuminen hakemistossa  
`ls` Näyttää tiedostot  
`ls -la`  
`whoami`  
`mkdir kansio/` Luo kansio  
`mv vanhanimi uusinimi` Kansion uudelleen nimeäminen  
`cp vanha uusi` Kansion tai tiedoston kopiointia  
`cp -r alkuperäinen uusi` Kansion kopiointi uuteen sijaintiin  
`rm poistettava`  
`rm -r kansio` Poistaa kansion ja alakansiot sisältöineen  
`rmdir kansio` Poistaa kansion  
  
Tulimuuri:  
`sudo apt-get -y install ufw`  
`sudo ufw allow 80/tcp`   
`sudo ufw allow 22/tcp`  
`sudo ufw enable`  
`ufw status`  
  
APT-GET:  
`sudo apt install wget` `sudo wget --version` `sudo which wget` Wgetin avulla voi ladata ohjelmia komentoriviltä.  
`sudo apt-get install micro`  
`sudo apt-get install trash-cli`  
  `trash-put tiedostonnimi`  
  `trash-list`  
  `trash-empty`  
`sudo apt-get -y install bash-completion`  
`sudo apt-get install pwgen` Password generator ja `pwgen -s 20 1`  
`sudo editor=micro` ja plugareita esim. `ctrl + E` ja `set colorscheme simple`  
  
SSH:  
`sudo apt-get -y install ssh`  
`systemctl status ssh`  
`sudo systemctl start ssh`  
`sudo ufw allow ssh` `sudo ufw enable` `sudo ufw status`  
`ssh käyttäjänimi@iposoite/localhost`   
`exit`  
  
Julkinen avain:  
`ssh-keygen` enterx3  
`ssh-copy-id tunnus@iposoite` eli esim `ssh-copy-id niinama01@localhost`    
`sudoedit /etc/ssh/sshd_config` salasanakirjautuminen pois päältä, muokkaa tiedostoon "PasswordAuthentication No"  

`sudo adduser uusikäyttäjä` `sudo adduser uusikäyttäjä sudo` uusi käyttäjä sudo ryhmään   
Testaa, että kirjautuminen ssh:lla onnistuu.  
`sudo usermod -- lock root` Lukitse root  
`sudoedit /etc/ssh/sshd_config` muuta PermitRootLogin No  
   
Oikeudet:  
`sudo chmod ugo+rx kansio` antaa read+execute oikeudet kansiolle  
`ls -l /home/käyttäjä` näyttää hakemiston oikeudet  
`ls -la`  
`chmod +x niina/` -> drwx--x--x eli kaikilla oikeus lukea, mutta sisältöä voi muokata vain käyttäjä itse  
`chmod o+w /niina/kotisivut/` muilla (others) oikeus muokata eli write  
   
Lokit:  
`sudo tail -f /var/log/apache2/access.log`  
`sudo tail -f /var/log/aoache2/error.log`  
  
Apache:  
`sudo apt-get -y install apache2`  
`sudo systemctl enable --now apache2` Apache käynnistyy automaattisesti konetta avatessa  
`curl localhost` tai http://localhost/  
`sudo systemctl start apache2` Uudelleenkäynnistys  
`echo "See you at niinamatela.me"|sudo tee /var/html/index.html`   
`sudo a2enmod userdir` Käyttäjäkansioiden salliminen, käynnistä apache uudestaan   
`mkdir -p home/niinam/publicsites/hattu.example.com/` tai  
`mkdir public_html` kotihakemistoon, omalle nimelle, sinne `micro index.html`  
        <!doctype html>
          <html lang="en">
          <head>
              <meta charset="utf-8">
              <meta name="viewport" content="width=device-width, initial-scale=1.0">
              <title>Testisivu</title>
          </head>
          <body>
              <p>Olen testisivu</p>
          </body>
          </html>  
      
  Selaimesta "http://localhost/~user"  
  
 Palomuuri Apachelle:  
 `ssh root@ip-osoite` `sudo apt-get update` `sudo apt-get install ufw`  
 `sudo ufw allow 22/tcp` Reikä palomuuriin  
 `sudo ufw enable`  
 
 `sudo service apache2 stop` Serverin pysäytys  
  
 `curl localhost`  
 `sudoedit /etc/hosts` tiedostoon lisätään domainnimet ja ohjaus localhostiin  
   
Name Based Virtual Host:  
`echo "Default"|sudo tee /var/www.html/index.html` , testaa localhostista muuttuiko sivu  
`sudo micro /etc/apache2/sites-available/hattu.example.com.conf`:   
    <VirtualHost *:80>
  ServerName hattu.example.com
  ServerAlias www.hattu.example.com
      DocumentRoot /home/niinam/publicsites/hattu.example.com
      <Directory /home/niinam/publicsites/hattu.example.com>
        Require all granted
     </Directory>
     </VirtualHost>  
      <VirtualHost *:80>
  ServerName localhost
      DocumentRoot /home/niinam/publicsites/hattu.example.com
      <Directory /home/niinam/publicsites/hattu.example.com>
        Require all granted
     </Directory>
     </VirtualHost>  
`sudo a2ensite hattu.example.conf` Ottaa käyttöön ko. conf-tiedoston  
`sudo a2dissite hattu.example.conf` Poistaa käytöstä ko. conf-tiedoston  
`sudo a2dissite 000-default.conf` Poistaa käytöstä default conf-tiedoston  
`sudo systemctl restart apache2`  

Sivun muokkaaminen normaalina käyttäjänä:  
`mkdir -p /home/niinam/publicsites/hattu.example.com/`  
`exho hattu > /home/niinam/publicsites/hattu.example.com/index.html`    
`sudoedit /etc/hosts` Muokkaa hostit: "127.0.0.1 hattu.example.com, 127.0.0.1 www.hattu.example.com"  

Django:  
`sudo apt-get -y install virtualenv` luo uuden virtaaliympäristön  
`virtualenv --system-site-packages -p python3 env/` luo uuden virtuaalisen ympäristön, jonne asennetaan python3 env/hakemistoon  
`source env/bin/activate` ottaa käyttöön virtuaalisen ympäristön, "env" tulee näkyviin alkuun.  
`which pip` komennolla varmistetaan, että ollaan oikeassa ympäristössä   
`micro requirements.txt` , kirjoitin tiedostoon "django", tallensin ctrl+s  
`cat requirements.txt` tarkistaa, että teksti django tallentui tiedostoon. Tänne tiedostoon lisätään kaikki tarvittavat python paketit, joita projektissa käytetään.  
`pip install -r requirements.txt` requirements.txt tiedoston asennnus  
`django-admin --version`  tarkistaa uusimman version   
Seuraavaksi uuden Django projektin tekemisen:  
`django-admin startproject harjoitus1`  
`cd harjoitus1`  
`./manage.py runserver` käynnistää kehitysympäristön palvelimella 
`python manage.py migrate`  
Linkin kautta avasin urlin, ja djangon etusivu näkyi localhostin portissa 8000 ja asennus tähän asti toimi odotetusti  
Seuraavaksi luotiin projektille admin.  
`./manage.py makemigrations` ja `./manage.py migrate` Päivittävät tietokannan (tein tämän jo edellisessä kohdassa joten tietokannat olivat ajan tasalla).  
`sudo apt-get install pwgen` asentaa salasanageneraattorin  
`pwgen -s 20 1` luo satunnaisen salasanan, jossa on 20 merkkiä  
`./manage.py createsuperuser`  luo admin tunnukset, syötä käyttäjätunnus, email ja salasana.    
Seuraavaksi kirjauduin sisään "127.0.0.1:8000/admin/" käyttäen admin tunnuksia  
Tein admin käyttöliittymän kautta toisen käyttäjätunnuksen "emilia", jolle annoin myös staff ja admin oikeudet. Testasin tunnusten toimivuuden kirjautumalla tunnuksilla uudestaan admin-sivuille.  
Seuraavaksi lähdin luomaan tietokantaa:  
`./manage.py startapp crm` luo uuden "crm" kansion CRM ohjelmalle  
`micro harjoitus1/settings.py` lisättiin ohjelma installed_apps kohtaan alimmaiseksi 'crm':  

![crm](https://github.com/user-attachments/assets/519a88aa-0066-44c5-b2f5-b4ce3e919dc2) 
    
`micro crm/models.py` tiedostoon lisättiin malli, jonka avulla django luo tietokannan, "class Assistants" tai "class.Customer" eli vaihda oikea muuttuja. `/home/niinama01/publicwsgi/labraharjoitus/crm/models.py`  
`./manage.py makemigrations` ja `./manage.py migrate` migraatioiden päivitys    
`micro crm/admin.py` jolla tietokanta rekisteröidään adminille, lisää tänne "register(models.Assistants tai models.Customer jne).      
`./manage.py runserver` käynnistää serverin, kirjauduin sisään "127.0.0.1:8000/admin". Lisäsin kaksi asiakasta.  
Jotta asiakkaiden nimet saatiin näkymään customer listauksessa, muokattiin (huomaa välilyönti sensitiivisyys, kuvassa väärin eli allekkain name ja def:  
  
![nimet2](https://github.com/user-attachments/assets/a15d64fb-1fda-459e-961a-fa7208e25b87)  
  
Komennot usealle käyttäjälle:  
`micro hei` ,tiedostoon sisälle "#!/bin/bash" ja alle komennot, jotka haluat ajaa eli esim `whoami,hostname,pwd,date, echo "Hello There!"`  
`chmod a+x hei` Käyttöoikeudet kaikille  
`sudo cp hei /usr/local/bin/` tiedoston kopioiminen jotta se toimii kaikilla  
`hei`  
  
SCP:
`scp -r Kotisivut/ niina@iposoite:public_html`
