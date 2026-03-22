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

Primerament hem creat un nou projecte anomenat _ESP32Test_Blink_ amb PlatformIO, seleccionant la placa _Espressif ESP32 Dev Module_ i el framework _Arduino_. PlatformIO genera automàticament l’estructura del projecte i descarrega les eines necessàries. L’estructura creada inclou la carpeta ```src```, que conté el fitxer principal ```main.cpp```, la carpeta ```lib``` per a llibreries addicionals, i el fitxer de configuració ```platformio.ini```.
A continuació, hem editat el fitxer ```main.cpp``` amb un programa senzill per fer parpellejar el LED integrat de la placa. El codi encén el LED durant un segon, l’apaga durant un altre segon i envia missatges indicant l’estat. Aquest procés es repeteix contínuament mentre la placa està en funcionament. Si obrim el ```Serial Monitor``` ens verifica que el LED de la placa parpelleja correctament cada segon, mostrant els missatges _“Led switched ON”_ i _“Led switched OFF”_.

------
## 2. Orientació 3D amb IMU
En la segona part de la pràctica s'ha connectat un **Inertial Mass Unit (IMU) sensor** a la placa ESP32 per obtenir l'orientació respecte al sistema de coordenades i visualitzar-ho en l'entorn 3D virtual.

### 2.1. Descripció del sistema

La IMU és un sensor que integra un **giroscopi** per mesurar les velocitats angulars en els eixos X, Y i Z, un **acceleròmetre** per mesurar les acceleracions lineals en aquests mateixos eixos, i una **brúixola** que permet obtenir una referència respecte al Nord. A partir d’aquestes mesures, el microprocessador calcula l’orientació 3D de l’objecte utilitzant els angles _Roll_, _Pitch_ i _Yaw_ respecte al sistema de coordenades del món. Les dades recollides per la IMU es poden transmetre a través de Wifi. Per fer-ho, hem utilitzat una xarxa comuna on cada grup tenia assignada una adreça IP específica tant per a l’ordinador com per al mòdul ESP32.

![tempImagePQ8gDx](https://github.com/user-attachments/assets/2332b3bb-f605-483b-8300-da3ed0a63e3c)

El _software setup_ està construït per 3 programes. Primerament, ```Endowrist_IMU``` que s’executa a l’ESP32 i s’encarrega de llegir el sensor i enviar les dades d’orientació. En segon lloc, un entorn de visualització 3D amb ```RoboDK```, que permet representar l’orientació d’un objecte virtual. Finalment, diversos scripts en Python que reben les dades enviades per l’ESP32 i les transmeten a l’entorn 3D per actualitzar l’orientació en temps real.


