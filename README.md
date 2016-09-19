# Description

Docker container that creates a VNC connection to the virt-manager GUI application.

# Usage

Basic usage is
```
docker run --name manager -p 5901:5901 taosnet/virt-manager
```

You can setup a unique SSH tunnel with a few commands:
```
docker exec -ti manager ssh-keygen -t rsa -b 4096
docker exec -ti manager scp /root/.ssh/id_rsa.pub remote_server_address:/root/.ssh/authorized_keys
```

## Extending

virt-manager works well with connecting to remote servers via SSH and then managing them through the tunnel. You will usually have public/private key authentication setup for the server. To have a portable app, you would probably want to extend this container with your own ssh credentials. A simple docker file could look like this.
```
FROM taosnet/virt-manager

COPY ssh /root/.ssh
RUN chown -R root.root /root/.ssh \
    && chmod 700 /root/.ssh \
    && chmod 600 /root/.ssh/id_rsa \
    && chmod 644 /root/.ssh/id_rsa.pub /root/.ssh/known_hosts
```

Note that SSH is very sensitive to file permissions, so you probably want to set them explicitly in your Dockerfile.
