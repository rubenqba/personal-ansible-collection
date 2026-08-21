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
- Adjunta los Docker secrets existentes definidos en `traefik.secrets` al servicio Swarm; el rol no crea ni modifica esos secrets.
- Si `traefik.secrets` no está vacío durante una ejecución standalone, la ejecución falla porque Docker secrets solo están disponibles para servicios Swarm.

## Variables principales

| Variable                              | Valor por defecto          | Descripción                                        |
| ------------------------------------- | -------------------------- | -------------------------------------------------- |
| `traefik.image`                       | `traefik:v3.7`             | Imagen de Traefik                                  |
| `traefik.network_name`                | `traefik`                  | Red Docker/Swarm                                   |
| `traefik.create_network`              | `true`                     | Crear la red si no existe                          |
| `traefik.dynamic_config_dir`          | `/etc/traefik/dynamic`     | Directorio de configuración dinámica del host      |
| `traefik.dynamic_config_enabled`      | `true`                     | Habilitar el montaje del directorio dinámico       |
| `traefik.command`                     | ver defaults               | Argumentos comunes de Traefik para ambos modos     |
| `traefik.extra_command`               | `[]`                       | Argumentos adicionales para ambos modos            |
| `traefik.ports`                       | ver defaults               | Puertos publicados en standalone y Swarm           |
| `traefik.labels`                      | `{}`                       | Labels del contenedor o servicio Traefik           |
| `traefik.secrets`                     | `[]`                       | Secrets Docker existentes que se adjuntan en Swarm |
| `traefik.swarm_placement_constraints` | `['node.role == manager']` | Restricciones del servicio Swarm                   |

Para una red overlay existente, use `traefik.create_network: false` y establezca el mismo `traefik.network_name`.

Para cambiar los puertos publicados, defina `traefik.ports` con `published_port`, `target_port`, `protocol` y `publish_mode`; el rol transforma esa variable al formato de `docker_container` en standalone y reutiliza sus mismos valores para el servicio Swarm.

Para usar un secret existente en Swarm, indique al menos `secret_name`; también puede definir `filename`, `uid`, `gid` y `mode` según las propiedades admitidas por `community.docker.docker_swarm_service`:

```yaml
        secrets:
          - secret_name: letsencrypt_account
            filename: /run/secrets/letsencrypt_account
            uid: "0"
            gid: "0"
            mode: 0400
```

```yaml
- name: Deploy Traefik
  hosts: docker
  become: true
  roles:
    - role: rubenqba.servers.traefik
      traefik:
        image: traefik:v3.7
        dynamic_config_dir: /srv/traefik/dynamic
        extra_command:
          - --accesslog=true
```
