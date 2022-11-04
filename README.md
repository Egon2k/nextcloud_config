# Nextcloud Troubleshooting


## Update Nextcloud

1. In den `Einstellungen` gehe unter `Verwaltung` auf `Übersicht`
2. Werden hier Fehler angezeigt, sollten diese zuerst behoben werden (siehe troubleshooting unten) bevor fortgefahren wird.
3. Klicke auf `Updater öffnen` und anschließend auf `Start Update`.
4. Sollte der Update-Prozess nicht erfolgreich durchlaufen, muss die `.step` Datei gelöscht und von neuem begonnen werden (Anleitung siehe unten).


## Troubleshooting

>Dein Datenverzeichnis und deine Dateien sind wahrscheinlich vom Internet aus erreichbar. Die .htaccess-Datei funktioniert nicht. Es wird dringend empfohlen, deinen Webserver dahingehend zu konfigurieren, dass das Datenverzeichnis nicht mehr vom Internet aus erreichbar ist oder dass du es aus dem Document-Root-Verzeichnis des Webservers herausverschiebst.

1. Öffne die Apache Config.
   ```
   nano /etc/apache2/apache2.conf
   ```
2. Suche (F6 in nano) nach
   ``` xml
   <Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride None
        Require all granted
   </Directory>
   ```
   und ändere es zu 
   ``` xml
   <Directory /var/www/>
        Options Indexes FollowSymLinks
        AllowOverride All
        Require all granted
   </Directory>
   ```
3. Starte apache neu.
   ```
   /etc/init.d/apache2 restart
   ```

>Die PHP-Speichergrenze liegt unterhalb des empfohlenen Wertes von 512MB
1. Öffne die `php.ini` für apache.
   ```
   nano /etc/php/8.1/apache2/php.ini
   ```
2. Suche (F6 in nano) nach `momory_limit` und ändere es auf `512M`
3. Starte apache neu.
   ```
   /etc/init.d/apache2 restart
   ```

>Update-Prozess bleibt stecken.
1. Navigiere zu `/var/www/html/nextcloud/data`
   ```
   cd /var/www/html/nextcloud/data
   ```
2. Lösche im Ordner `/updater-occ[random-string]` die `.step` Datei.
3. Starte den Update-Prozess neu.

