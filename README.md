Mit Hilfe von mod_rewrite & .htaccess lassen sich auch bei der Nutzung des Ilch CMS benutzerfreundliche (suchmaschinenfreundliche) URLs manipulieren.
Diese Methode funktioniert aber nicht bei jedem Anbieter.Mehr Info´s dazu findest du auf http://www.modrewrite.de/ .
Diese Tutorial wurde mit Hilfe des Ilch Forum erstellt.

Mit der Umsetzung entnehmen wir das index.php? der URL . Z.b. ist dann das Forum anstatt nur mit www.meine-seite.de/index.php?forum auch unter www.meine-seite.de/forum ereichbar.

Bevor es an die Umsetzung geht, sollte die Webseite gesichert werden, um ggf. die Änderungen an den Dateien rückängig machen zu können!

1.0

Als erstes erstellen wir die .htaccess.Wenn bereits ein htaccess Datei vorhanden ist, muss nur der angegebene Codesatz eingefügt werden.
Erstelle dazu ein Text Datei ( htaccess.txt) und füge folgenden Inhalt ein:

Quellcode: .htaccess
```ruby
    RewriteEngine On
    RewriteBase /
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteRule ^(.*)$ index.php?$1 [L,QSA]
```


Speichere die Datei und lade diese auf den Root / ftp auf. Die htaccess.txt Datei muss sich in dem Ordner abgelegt werden wo sich auch die index.php und die admin.php befinden.
Benenne nun die htaccess.txt in .htaccess um.
Jetzt kannst du schon prüfen, ob du deine Seiten ohne index.php? aufrufen kannst.Sollte das der Fall sein, kann es weiter gehen mit den nächsten Schritten.
Wenn nicht, verhindert ggf. der Provider die Nutzung dieser Dateien. Dazu weitere Infos im oben angegeben Link einholen oder/und Provider kontaktieren.

1.1

Das Aufrufen der URL ohne index.php? ist damit umgesetzt.Nun passen wird noch die Bereiche im Ilch CMS an, da vieles auf das index.php ausgelegt ist.
Fangen wir mit den Menü´s an.Dazu muss die include/includes/class/design.php öffnen.In Zeile 279 siehst du folgenden Eintrag:

PHP-Quellcode
```ruby
    list ($wmpA, $wmpE, $wmpTE, $wmpTEE) = explode ('|', $tpl->list_get ($hovmenup, array ($menuTarget, ($subhauptx == 8 ? '' : 'index.php?') . $row['path'], $row['name'])));
```


Entferne dort nur das index.php?

Suche nun nach der Funktion footer. Ersetze diese mit dem folgenden Codesatz:

PHP-Quellcode
```ruby
    		 function footer ($exit = 0) {
    	  echo $this->html[1];
    		unset ($this->html[1]);
     
    $c = ob_get_clean();
    $c = preg_replace ('%href=\"\?([^\"]+)\"%Uis',"href=\"index.php?\\1\"",$c);
    $c = preg_replace ('%href=\"index.php\?([-0-9A-Z]+)#([a-zA-Z0-9]+)\">%Uis',"href=\"\\1#\\2\">",$c);
    $c = preg_replace ('%href=\"index.php\?([-0-9A-Z]+)\">%Uis',"href=\"\\1\">",$c);
    $c = preg_replace ('%action=\"\?([^\"]+)\"%Uis',"action=\"index.php?\\1\"",$c);
    $c = preg_replace ('%URL=\?([^\"]+)\"%Uis',"URL=index.php?\\1\"",$c);
    echo $c;
     
     
    	if ($exit == 1) {
    	  exit();
    	}
    	}
```


Speichere die Datei.
Die erste Änderung hätte man auch mit der funktion footer Umsetzung auffangen können.Allerdings hat man ja gerade die Datei offen.

Öffne nun die index.php und füge gleich nach <?php folgendes an:

PHP-Quellcode
```ruby
    ob_start();
```


Alternativ zur Änderung der Funktion footer können auch die Dateien direkt bearbeitet werden.Es müssten also alle index.php? Verlinkungen in den Dateien entfernt werden.
Die manuelle Änderungen an diversen Dateien ( u.a. Linkanpassungen vom Template) wird wohl nicht ausbleiben. :)

Wie oben bereits erwähnt, vorher die Dateien der Seite sichern! ( Backup des ftp)