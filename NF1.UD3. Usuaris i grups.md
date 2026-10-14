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
