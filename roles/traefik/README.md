# Traefik

Despliega Traefik como servicio usando Docker standalone o Docker Swarm.
El rol detecta automáticamente si el daemon está unido a un Swarm.

## Requisitos

- Docker instalado y accesible por el usuario remoto.
- La colección `community.docker` instalada (`ansible-galaxy collection install community.docker`).
- En Swarm, el play debe ejecutarse contra un nodo manager (`ControlAvailable: true`).

## Comportamiento

- En Docker standalone crea un contenedor con `restart_policy: unless-stopped`.
- En Docker Swarm crea un servicio con la restricción `node.role == manager`.
- Crea la red automáticamente si `traefik.create_network` es `true`: bridge en standalone y overlay attachable en Swarm.
- Monta el socket Docker en modo lectura para que Traefik escuche los eventos del runtime correspondiente.
- Crea y monta `traefik.dynamic_config_dir` como el directorio del file provider. Los ficheros YAML/TOML colocados allí se recargan con `--providers.file.watch=true`.

## Variables principales

| Variable | Valor por defecto | Descripción |
| --- | --- | --- |
| `traefik.image` | `traefik:v3.0` | Imagen de Traefik |
| `traefik.network_name` | `traefik` | Red Docker/Swarm |
| `traefik.create_network` | `true` | Crear la red si no existe |
| `traefik.dynamic_config_dir` | `/etc/traefik/dynamic` | Directorio de configuración dinámica del host |
| `traefik.dynamic_config_enabled` | `true` | Habilitar el montaje del directorio dinámico |
| `traefik.command` | ver defaults | Argumentos base de Traefik para Docker standalone |
| `traefik.swarm_command` | ver defaults | Argumentos base de Traefik para Docker Swarm |
| `traefik.extra_command` | `[]` | Argumentos adicionales para ambos modos |
| `traefik.published_ports` | `['80:80', '443:443']` | Puertos del contenedor standalone |
| `traefik.swarm_publish` | ver defaults | Publicación de puertos del servicio Swarm |
| `traefik.labels` | `{}` | Labels del contenedor o servicio Traefik |
| `traefik.swarm_placement_constraints` | `['node.role == manager']` | Restricciones del servicio Swarm |

Para una red overlay existente, use `traefik.create_network: false` y establezca el mismo `traefik.network_name`.

## Ejemplo

```yaml
- name: Deploy Traefik
  hosts: docker
  become: true
  roles:
    - role: rubenqba.servers.traefik
      traefik:
        image: traefik:v3.0
        dynamic_config_dir: /srv/traefik/dynamic
        extra_command:
          - --accesslog=true
```
