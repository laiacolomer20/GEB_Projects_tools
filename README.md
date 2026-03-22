# Seminari 2
L'objectiu de la sessió ha estat apendre a utilitzar les plataformes **GitHub** i **PlatformIO**, mitjançant **Visual Studio Code** (VS Code), per programar una placa **ESP32**. 

------
## 1. Eines de Projectes d’Enginyeria
### 1.1. Configuració de l'entorn
La directora del grup ha fet un _fork_ del repositori proporcionat pel professor i, a partir d'aquest, hem afegit els col·laboradors, permetent la lliure edició sobre el projecte el qual es manté sempre actualitzat al núvol. 

Un cop configurat el repositori, hem clonat el projecte al nostre ordinador. Per fer-ho hem creat una carpeta de treball al _Desktop_ i l'hem obert amb VS Code. Des del terminal hem executat la comanda ```git clone``` amb l'adreça del repositori del director, creant una còpia local exacta del projecte. Seguidament hem obert la carpeta clonada des del VS Code per començar a treballar sobre el repositori local. 

Per sincronitzar els canvis realitzats respecte la versió original, cal configurar les credencials de _Git_ amb el nostre correu i nom d'usuari. Això cal sols la primera vegada. A continuació, des del menú _Source Control_ s'han confirmat els canvis mitjançant un ```commit``` i s'ha enviat al servidor amb ```push```. Per últim, hem verificat la correcta sincronització de les actualitzacions al GitHub. 

### 1.2. Creació d’un primer projecte ESP32 amb PlatformIO
En aquesta secció hem utilitzat VS Code amb l'extensió PlatformIO, on hem aconseguit fer parpellejar un LED connectat al microcontrolador ESP32. L’**ESP32** és una placa amb microprocessador que disposa de WiFi, Bluetooth i múltiples entrades i sortides per una àmplia gamma de sensors. Aquesta l'hem connectat a l’ordinador mitjançant **USB**.





