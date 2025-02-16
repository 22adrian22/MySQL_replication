# Configuración y Ejecución del Playbook de Replicación MySQL

Este documento proporciona instrucciones detalladas para ejecutar correctamente el playbook de replicación de MySQL en un entorno gestionado por Vagrant y Ansible.

---

## **Pasos Previos**
Antes de ejecutar el playbook, es **obligatorio** ejecutar los siguientes comandos **después de levantar Vagrant (`vagrant up`)**. Esto asegurará una correcta autenticación SSH con las máquinas virtuales.

### **Eliminar claves SSH antiguas**
Ejecuta los siguientes comandos para evitar conflictos de autenticación:
```bash
ssh-keygen -f "/home/usuario/.ssh/known_hosts" -R "192.168.57.101"
ssh-keygen -f "/home/usuario/.ssh/known_hosts" -R "192.168.57.102"
```

### **Acceder a las máquinas virtuales vía SSH**

Para asegurarse de que la conexión funciona correctamente, inicia sesión en las máquinas virtuales:
```bash
ssh vagrant@192.168.57.101 -i .vagrant/machines/mysql-master/virtualbox/private_key
ssh vagrant@192.168.57.102 -i .vagrant/machines/mysql-slave/virtualbox/private_key
```

---

## **Ejecución del Playbook**
Es **necesario ejecutar el playbook dos veces seguidas** para garantizar que la replicación de MySQL se configure correctamente.

Ejecuta el siguiente comando **dos veces**:
```bash
ansible-playbook -i inventory playbook.yml
```

---

## **Verificación de la Replicación**
### **Consultar el estado del maestro**
Para verificar la información del servidor maestro, ejecuta:
```bash
mysql -u root -p'usuario' -e "SHOW MASTER STATUS\G"
```

### **Prueba de replicación**
Para comprobar que la replicación funciona correctamente, sigue estos pasos:

#### **1. Crear una base de datos en el maestro**
```bash
mysql -u root -p'usuario' -e "CREATE DATABASE prueba_replica1;"
```

#### **2. Verificar la replicación en el esclavo**
```bash
mysql -u root -p'usuario' -e "SHOW DATABASES LIKE 'prueba_replica1';"
mysql -u root -p'usuario' -e "SHOW SLAVE STATUS\G"
```
Si la base de datos `prueba_replica1` aparece en el esclavo y el estado del esclavo indica que está en ejecución, la replicación se ha configurado correctamente.

---

## **Notas Adicionales**
- Asegúrate de que las direcciones IP de las máquinas virtuales sean correctas.
- Si experimentas problemas de conexión SSH, reinicia Vagrant con `vagrant reload`.
- Si la replicación falla, revisa los logs de MySQL en `/var/log/mysql/` tanto en el maestro como en el esclavo.

---

⚡ **Ahora tu entorno está listo!** Si tienes problemas, revisa los logs y asegúrate de seguir cada paso correctamente. 🚀

