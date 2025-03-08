# Jenkins Master-Agent con Docker Compose

[![Docker][Docker-badge]][Docker-url]
[![Jenkins][Jenkins-badge]][Jenkins-url]

Sistema dockerizado de Jenkins con configuración maestro-agente utilizando Docker Compose.

## 📋 Características

- Jenkins Master (LTS) con Docker integrado
- Agente Jenkins conectado automáticamente
- Persistencia de datos mediante volúmenes Docker
- Configuración lista para producción

## 🔧 Prerrequisitos

- Docker (versión 20.10.0+)
- Docker Compose (versión 2.0.0+)

## 🚀 Instalación y Configuración

### 1. Preparación del entorno

Clona el repositorio y prepara el directorio de Jenkins:

```bash
git clone https://github.com/tu-usuario/jenkins-docker-compose.git
cd jenkins-docker-compose
mkdir -p ./jenkins_home
sudo chown -R 1000:1000 ./jenkins_home
```

### 2. Iniciar los contenedores

```bash
docker-compose up -d
```

### 3. Configuración inicial de Jenkins

1. Obtener la contraseña inicial de administrador:

```bash
docker exec jenkins-master cat /var/jenkins_home/secrets/initialAdminPassword
```

2. Acceder a Jenkins en `http://localhost:8080`
3. Ingresar la contraseña obtenida en el paso anterior
4. Instalar los plugins sugeridos
5. Crear el primer usuario administrador

### 4. Configuración del Agente

1. Navegar a **Administrar Jenkins** > **Administrar Nodos y Nubes**
2. Seleccionar **Nuevo Nodo**
3. Configurar:
   - Nombre: `dev`
   - Tipo: **Agente permanente**
   - Método de lanzamiento: **Connect to the master**
4. Guardar y copiar el secreto generado
5. Actualizar la variable `JENKINS_SECRET` en el `docker-compose.yml`

## 🛠️ Estructura del Proyecto

jenkins-docker-compose/
├── docker-compose.yml # Configuración de servicios
├── jenkins_home/ # Volumen persistente de Jenkins
└── README.md # Documentación

## 📦 Servicios

### Jenkins Master

- Puerto: 8080 (Web UI)
- Puerto: 50000 (Comunicación con agentes)
- Volumen: `./jenkins_home:/var/jenkins_home`
- Docker socket montado para operaciones Docker

### Jenkins Agent

- Conexión automática al master
- Variables de entorno configurables:
  - `JENKINS_URL`
  - `JENKINS_AGENT_NAME`
  - `JENKINS_SECRET`

## 🔑 Variables de Entorno

| Variable           | Descripción              | Valor por defecto          |
| ------------------ | ------------------------ | -------------------------- |
| JENKINS_URL        | URL del master           | http://jenkins-master:8080 |
| JENKINS_AGENT_NAME | Nombre del agente        | dev                        |
| JENKINS_SECRET     | Secreto de autenticación | [Requerido]                |

## 📝 Notas Importantes

- Asegúrate de cambiar el secreto del agente en el `docker-compose.yml`
- Los datos de Jenkins persisten en `./jenkins_home`
- El socket de Docker está montado para permitir operaciones Docker dentro de Jenkins

## 🤝 Contribuciones

Las contribuciones son bienvenidas. Por favor:

1. Haz fork del proyecto
2. Crea una rama para tu feature (`git checkout -b feature/AmazingFeature`)
3. Commit tus cambios (`git commit -m 'Add AmazingFeature'`)
4. Push a la rama (`git push origin feature/AmazingFeature`)
5. Abre un Pull Request

## 📄 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para más detalles.

[Docker-badge]: https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white
[Docker-url]: https://www.docker.com/
[Jenkins-badge]: https://img.shields.io/badge/Jenkins-D24939?style=for-the-badge&logo=jenkins&logoColor=white
[Jenkins-url]: https://www.jenkins.io/
