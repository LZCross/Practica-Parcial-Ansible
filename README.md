# Práctica Parcial: Ansible con Docker

**Autor:** Lizander Cross (2021-1754)  
**Fecha:** Marzo 2026  
**Versión:** 1.0  

## 📋 Descripción

Esta práctica demuestra el uso básico de Ansible para automatizar la configuración de servidores simulados mediante contenedores Docker. El objetivo principal es instalar y configurar Nginx en múltiples nodos virtuales, desplegando una página web personalizada. Es ideal para aprender conceptos fundamentales de Ansible como inventarios, playbooks y módulos, en un entorno local seguro.

## 🎯 Objetivos de Aprendizaje

- Comprender la estructura de un proyecto Ansible.
- Configurar inventarios estáticos.
- Escribir y ejecutar playbooks básicos.
- Gestionar conexiones SSH seguras en entornos simulados.
- Integrar Ansible con Docker para pruebas locales.

## 🧰 Requisitos Previos

Antes de comenzar, asegúrate de tener instalados los siguientes componentes:

- **Docker**: Versión 20.10 o superior. [Instalación](https://docs.docker.com/get-docker/)
- **Docker Compose**: Incluido con Docker Desktop o instala por separado.
- **Ansible**: Versión 2.10 o superior. [Instalación](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
- **Git**: Opcional, para clonar el repositorio.

### Verificación de Instalación

Ejecuta los siguientes comandos para verificar que todo esté instalado correctamente:

```bash
docker --version
docker-compose --version
ansible --version
```

## 🚀 Instalación y Configuración

### 1. Clonar el Repositorio

```bash
git clone https://github.com/LZCross/Practica-Parcial-Ansible.git
cd Practica-Parcial-Ansible
```

### 2. Construir y Levantar los Contenedores

Construye las imágenes Docker y ejecuta los contenedores en segundo plano:

```bash
docker-compose up --build -d
```

Esto creará dos contenedores (`node1` y `node2`) simulando servidores Ubuntu con SSH habilitado.

### 3. Verificar los Contenedores

Asegúrate de que los contenedores estén corriendo:

```bash
docker-compose ps
```

Deberías ver algo como:

```
     Name                   Command               State                    Ports
-----------------------------------------------------------------------------------
node1   /usr/sbin/sshd -D                 Up      0.0.0.0:2222->22/tcp
node2   /usr/sbin/sshd -D                 Up      0.0.0.0:2223->22/tcp
```

## 📖 Uso

### Probar la Conexión con Ansible

Verifica que Ansible pueda conectarse a los nodos:

```bash
ansible -i inventory.ini all -m ping
```

Salida esperada:

```
node1 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
node2 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

### Ejecutar el Playbook

Ejecuta el playbook para configurar Nginx en los servidores:

```bash
ansible-playbook -i inventory.ini playbook.yml
```

El playbook realizará las siguientes tareas:
- Instalar Nginx.
- Copiar la página web personalizada.
- Iniciar y habilitar el servicio Nginx.

### Verificar el Despliegue

Accede a la página web desde tu navegador o usando curl:

```bash
curl http://localhost:2222
curl http://localhost:2223
```

Deberías ver el contenido de `site/index.html`: "¡Hola desde Ansible!".

## 📁 Estructura del Proyecto

```
Practica-Parcial-Ansible/
├── Dockerfile              # Imagen base para los contenedores simulados
├── docker-compose.yml      # Configuración de los servicios Docker
├── inventory.ini           # Inventario de hosts para Ansible
├── playbook.yml            # Playbook principal de Ansible
├── site/
│   └── index.html          # Página web personalizada
└── README.md               # Este archivo
```

## ⚙️ Configuración Avanzada

### Ignorar Advertencias SSH

El archivo `inventory.ini` incluye configuraciones para ignorar advertencias de claves SSH conocidas, facilitando el uso en entornos de prueba:

```ini
ansible_ssh_common_args='-o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null'
```

### Personalización

- **Cambiar la página web**: Edita `site/index.html`.
- **Agregar más nodos**: Modifica `docker-compose.yml` y `inventory.ini`.
- **Extender el playbook**: Agrega más tareas en `playbook.yml`.

## 🛠️ Solución de Problemas

- **Error de conexión SSH**: Asegúrate de que los contenedores estén corriendo y los puertos 2222/2223 estén libres.
- **Fallo en ping**: Verifica que Ansible esté instalado y el inventario sea correcto.
- **Problemas con Docker**: Reinicia Docker o ejecuta `docker-compose down` y vuelve a levantar.

## 🤝 Contribución

Si deseas contribuir:
1. Haz un fork del repositorio.
2. Crea una rama para tu feature (`git checkout -b feature/nueva-funcionalidad`).
3. Realiza tus cambios y haz commit (`git commit -am 'Agrega nueva funcionalidad'`).
4. Push a la rama (`git push origin feature/nueva-funcionalidad`).
5. Abre un Pull Request.

## 📄 Licencia

Este proyecto es de uso educativo y no tiene una licencia específica. Siéntete libre de usarlo y modificarlo para fines de aprendizaje.

## 📧 Contacto

**Autor:** Lizander Cross  
**Correo:** 20211754@itla.edu.do  
**GitHub:** [https://github.com/LZCross](https://github.com/LZCross)

---

¡Feliz automatización con Ansible!
