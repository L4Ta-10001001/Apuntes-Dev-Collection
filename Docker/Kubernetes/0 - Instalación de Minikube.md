Para instalar minikube, nos bajamos el binario, le damos permisos y lo configuramos:
```bash
wget https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
chmod +x minikube-linux-amd64
sudo mv minikube-linux-amd64 /usr/local/bin/minikube
```
Y ya funciona:
![[Pasted image 20240411194129.png]]
Y ahora vamos a instalar kubectl:
```bash
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
```
![[Pasted image 20240411194418.png]]
Y ahora para arrancar kubectl tenemos que ejecutar el siguiente comando:
```bash
sudo usermod -aG docker $USER && newgrp docker
```
Y luego kubectl start y ya debería funcionar bien:
![[Pasted image 20240411194702.png]]
