# Usuaris i grups

**Noah Rodriguez** · Sistemes operatius en xarxa

En aquesta pràctica he creat grups i usuaris al servidor Linux, he personalitzat el directori plantilla /etc/skel, he gestionat contrasenyes i comptes (bloqueig, desbloqueig i eliminació) i he canviat la shell d'un usuari.

---

## Tasca 1. Creació de la jerarquia de grups

### Objectiu

Crear els grups devs, sysadmin i auditor i comprovar quin GID té cadascun.

### Comandes utilitzades

```bash
sudo groupadd devs
sudo groupadd sysadmin
sudo groupadd auditor
tail -n 3 /etc/group
```

### Explicació

La comanda groupadd crea un grup nou i li assigna el primer GID lliure a partir de 1000. Els grups es guarden al fitxer /etc/group, amb el format nom:contrasenya:GID:membres. Amb tail -n 3 mostro les tres últimes línies del fitxer, que són els grups que acabo de crear.

![Captura 2026-10-07 181129](/Imatges/Captura1.png)

També ho he comprovat amb la comanda getent:

```bash
getent group devs sysadmin auditor
```

![Captura 2026-10-07 184419](/Imatges/Captura2.png)

### Conclusió

Els tres grups s'han creat correctament. Els seus GID són:

- devs: 1001
- sysadmin: 1002
- auditor: 1003

---

## Tasca 2. Gestió dels perfils d'usuari

### Objectiu

Fer que tots els usuaris nous tinguin automàticament un fitxer de benvinguda, una carpeta Documents i l'alias ll, i que la shell per defecte sigui /bin/bash.

### 2.1 Fitxer de benvinguda

#### Comanda utilitzada

```bash
sudo nano /etc/skel/BIENVENIDA.txt
```

### Explicació

El directori /etc/skel (de "skeleton") és la plantilla que es copia al directori personal de cada usuari nou quan es crea. Qualsevol fitxer que hi posi apareixerà automàticament a tots els usuaris futurs. He creat BIENVENIDA.txt amb les normes d'ús del servidor.

Comprovo el contingut amb cat:

```bash
cat /etc/skel/BIENVENIDA.txt
```

![Captura 2026-10-07 184610](/Imatges/Captura4.png)

### 2.2 Carpeta Documents

#### Comandes utilitzades

```bash
sudo mkdir /etc/skel/Documents
ls -la /etc/skel
```

### Explicació

Igual que amb el fitxer, una carpeta dins de /etc/skel es crearà per defecte al directori personal dels usuaris nous. Amb ls -la veig que hi ha el fitxer BIENVENIDA.txt, la carpeta Documents i els fitxers de configuració de Bash.

![Captura 2026-10-07 184626](/Imatges/Captura5.png)

### 2.3 Alias ll

#### Comandes utilitzades

```bash
sudo nano /etc/skel/.bashrc
# afegeixo al final del fitxer:
alias ll='ls -la --color=auto'
tail -n 3 /etc/skel/.bashrc
```

### Explicació

El fitxer .bashrc l'executa Bash cada vegada que s'obre una terminal interactiva. Amb aquest alias, quan l'usuari escriu ll s'executa ls -la --color=auto, és a dir, un llistat llarg amb els fitxers ocults i en color.

![Captura 2026-10-07 184655](/Imatges/Captura6.png)

Comprovo que la línia s'ha desat al final del fitxer:

![Captura 2026-10-07 184713](/Imatges/Captura7.png)

### 2.4 Shell per defecte a /etc/adduser.conf

### Comanda utilitzada

```bash
sudo nano /etc/adduser.conf
```

### Explicació

Al fitxer /etc/adduser.conf, la variable DSHELL defineix la shell que reben els usuaris nous. El valor per defecte ja és /bin/bash, que és el que es demana.

### 2.5 Prova amb un usuari nou (prova24)

### Comandes utilitzades

```bash
sudo adduser prova24
su - prova24
ls -la ~
cat BIENVENIDA.txt
ll
exit
sudo deluser --remove-home prova24
```

### Explicació

He creat l'usuari de prova prova24 (el meu número de llista és el 24) i he iniciat sessió amb ell. Si les personalitzacions de /etc/skel funcionen, el seu directori personal ha de tenir el fitxer BIENVENIDA.txt i la carpeta Documents, i l'alias ll ha de funcionar.

Directori personal de prova24, amb BIENVENIDA.txt i Documents:

![Captura 2026-10-07 184946](/Imatges/Captura8.png)

Contingut del fitxer de benvinguda:

![Captura 2026-10-07 184958](/Imatges/Captura9.png)

Funcionament de l'alias ll:

![Captura 2026-10-07 185012](/Imatges/Captura10.png)

Per eliminar l'usuari de prova vaig provar deluser -r prova24, però el sistema va donar un error d'opció ambigua, perquè en aquesta versió -r pot ser --remove-home o --remove-all-files:

![Captura 2026-10-07 185353](/Imatges/Captura11.png)

Ho vaig resoldre escrivint l'opció completa --remove-home. Després, grep prova24 /etc/passwd no retorna res, així que l'usuari ja no existeix:

![Captura 2026-10-07 185836](/Imatges/Captura12.png)

### 2.6 Aplicar els canvis a l'usuari existent

### Comandes

```bash
cp -r /etc/skel/. ~
source ~/.bashrc
```

La primera comanda copia tot el contingut de la plantilla al meu directori personal (i sobreescriu els fitxers que ja hi eren) i la segona recarrega la configuració a la sessió actual.

### Conclusió

Els usuaris nous reben automàticament el fitxer de benvinguda, la carpeta Documents i l'alias ll, amb /bin/bash com a shell. La prova amb prova24 ho confirma.

---
