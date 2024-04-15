# Installation (Hot to install docker CE/EE):
    1.Uninstall old versions:
    sudo yum remove -y docker \
                    docker-client \
                    docker-client-latest \
                    docker-common \
                    docker-latest \
                    docker-latest-logrotate \
                    docker-logrotate \
                    docker-engine

    2.Install Docker CE Add the Utilities needed for Docker:
    sudo yum install -y yum-utils \
    device-mapper-persistent-data \
    lvm2

    3.Set up the stable repository:
    sudo yum-config-manager \
        --add-repo \
        https://download.docker.com/linux/centos/docker-ce.repo

    4.Install Docker CE:
        sudo yum -y install docker-ce

    5.Enable and start Docker:
        sudo systemctl start docker && sudo systemctl enable docker

    6.Add cloud_user to the docker group:
        sudo usermod -aG docker cloud_user

    7.check if their are any permission porblems for the user and the group