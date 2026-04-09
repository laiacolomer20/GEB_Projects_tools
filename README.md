# Seminari 2
_María Anglada, Laia Colomer, Paula Pepió_     

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

<div align="center">
  <img src="https://github.com/user-attachments/assets/2332b3bb-f605-483b-8300-da3ed0a63e3c" width="300">
</div>

El _software setup_ està construït per 3 programes. Primerament, ```Endowrist_IMU``` que s’executa a l’ESP32 i s’encarrega de llegir el sensor i enviar les dades d’orientació. En segon lloc, un entorn de visualització 3D amb ```RoboDK```, que permet representar l’orientació d’un objecte virtual. Finalment, diversos scripts en Python que reben les dades enviades per l’ESP32 i les transmeten a l’entorn 3D per actualitzar l’orientació en temps real.

### 2.2. Implementació del sistema IMU-ESP32

Inicialment, hem obert el projecte Endowrist_IMU amb VS Code i hem revisat el fitxer ```main.cpp```, que ja incloïa el programa per llegir la IMU i publicar els angles RPY. Seguidament, hem adaptat el codi al nostre grup modificant l’identificador del dispositiu i l’adreça IP del receptor. Després, cal compilar el projecte i carregar-lo a la placa ESP32 amb PlatformIO, fins que apareixi el missatge confirmant que la càrrega s’ha completat correctament.

Més tard, hem obert l’entorn de visualització **3D_Orientation.rdk** a ```RoboDK```, que conté un sistema de coordenades global i un objecte 3D que es pot orientar segons les dades rebudes. Tot seguit, hem executat l’script de Python ```Receive_data_RPY_IMU_world.py```, que rep les dades enviades per l’ESP32 a través de WiFi i actualitza l’orientació de l’objecte en l’entorn virtual. Finalment, hem modificat l’script per seleccionar l’objecte que volíem moure, primer amb **"plane"** i després amb **"surgical_needle"**. Per canviar l'objecte 3D de "plane" a "surgical_needle", hem modificat l'script de Python canviant el nom de l'objecte a la línia on es selecciona l'element de RoboDK. Concretament, hem substituït "plane" per "surgical needle" en la variable que indica quin objecte ha de rebre l'orientació calculada.

### 2.3. Resultats i ajustaments

Durant les primeres proves hem observat que l’orientació no era exactament correcta. Això és degut al fet que la brúixola de la IMU és sensible a interferències magnètiques i no proporciona una referència perfecta del nord. Per aquest motiu, hem hagut d’orientar manualment el sensor i ajustar el sistema de referència perquè coincidís amb el sistema utilitzat a ```RoboDK```. Concretament, hem intentat alinear l’eix X del sensor amb el Nord i verificar que el valor de yaw fos proper a **zero**. Per verificar els angles, hem mogut el sensor en cada eix per separat: primer roll (inclinació lateral), després pitch (inclinació frontal) i finalment yaw (rotació horitzontal), comprovant que l'objecte 3D a RoboDK responia correctament en cada cas. Un cop fetes aquestes correccions, hem tornat a executar el sistema i hem comprovat que l'objecte 3D seguia correctament els moviments de la IMU. En moure el sensor, l'agulla quirúrgica virtual canviava la seva orientació en temps real, confirmant que la comunicació entre el sensor, l'ESP32, el script Python i RoboDK funcionava correctament.

Un cop fetes aquestes correccions, hem tornat a executar el sistema i hem comprovat que l’objecte 3D seguia correctament els moviments de la IMU. En moure el sensor, l’agulla quirúrgica virtual canviava la seva orientació en temps real, confirmant que la comunicació entre el sensor, l’ESP32, el script Python i ```RoboDK``` funcionava correctament. 

------
## Conclusió 
Aquesta sessió ens ha donat l'oportunitat d'aprendre a utilitzar eines clau per al desenvolupament de projectes d'enginyeria. D'una banda, GitHub i PlatformIO ofereixen un entorn de treball per gestionar el codi de manera col·laborativa i programar microcontroladors de forma eficient. D'altra banda, la combinació de la IMU amb l'ESP32, Python i ```RoboDK`` ens ha permès veure com connectar el món físic amb una simulació digital en temps real.

De cara al TFG o futurs projectes d'enginyeria, GitHub ens serà útil per mantenir un control de versions del codi i treballar en equip de manera organitzada, evitant pèrdues de feina i facilitant la col·laboració. La integració de sensors IMU amb entorns de simulació 3D pot aplicar-se en sistemes de navegació autònoma, control de robots industrials, o sistema de instrumentació quirúrgica robòtica on cal  conèixer en tot moment la posició i orientació de l'eina dins del cos del pacient. A més amb ```RoboDK``` podem validar el comportament del sistema de forma virtual abans de treballar amb el hardware real, reduint riscos i costos.
