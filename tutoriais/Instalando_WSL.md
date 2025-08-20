
## 1. Comando de Instalação do WSL

- Abra o Windows PowerShell como **administrador**

![img/Pasted image 20250820100458.png](img/Pasted%20image%2020250820100458.png)
- No Poweshell escreva  o comando e pressione a tecla `Enter` para rodar o comando
```powershell
wsl --install
```
- Reinicie seu computador

## 2. Instalando o Ubuntu 24 no WSL

Agora que você já está com o WSL instalado, podemos instalar uma disto Linux nesse ambiente. 

- Abra novamente o Windows PowerShell
- Com o comando abaixo você pode listar as distribuições Linux disponíveis
```powershell
wsl.exe --list --online
```
![img/Pasted image 20250820100958.png](img/Pasted%20image%2020250820100958.png)
- Vamos instalar o Ubuntu 24.04 LTS. Para isso use o comando
```powershell
wls.exe --install Ubuntu-24.04
```

Isso deve demorar um pouco mas instalará todo o Ubuntu no ambiente WSL.
Você deverá criar um username.

- Após a instalação do Ubuntu você pode fechar o PowerShell.
- Sempre que você quiser abrir o Ubuntu no WSL você pode abrir o PowerShell e escolher a distribuição Linux Ubuntu na setinha `v` da barra superior, como na Figura abaixo
![img/Pasted image 20250820101917.png](img/Pasted%20image%2020250820101917.png)

## 3. Instalando Pacotes 

- Para fazer update dos programas no Ubuntu você deve usar o comando
```bash
sudo apt update
```

- Em seguida você pode fazer um upgrade dos pacotes usando
```bash
sudo apt full-upgrade
```

- Instale o `build-essential` que possui o gcc e outros pacotes importantes
```bash
sudo apt -y install cmake pkg-config build-essential
```

- Você pode checar as versões do python e do gcc com os comandos 
```bash
python3 --version
```

```bash
gcc --version
```
## 4. Instalando Miniforge (Altamente Recomendado)

- Baixe o `Miniforge` direto do site oficial usando o comando
```bash
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh
```

- Instale o arquivo baixado com o comando
```bash
bash Miniforge3-$(uname)-$(uname -m).sh
```

- A instalação ocorrerá no diretório raiz `~/miniforge3` e também configurará seu perfil no arquivo `~/.bashrc`. Você pode ativar essas configurações usando o comando 
```bash
source ~/.bashrc
```

- Por padrão o `conda` ativará automaticamente o environment `base` toda a vez que você abrir um terminal do Ubuntu. Podemos desativar isso usando o comando 
```bash
conda config --set auto_activate_base false
```

- Agora, toda vez que você quiser usar o `conda` você pode ativar o environment `base` usando o comando 
```bash
conda activate 
```

- Você pode criar um novo ambiente conda com nome `[nome_do_ambiente]` (sem os colchetes) usando o comando 
```bash
conda create -n [nome_do_ambiente] -c conda-forge
```

- Em seguida você pode ativar esse ambiente usando 
```bash
conda activate [nome_do_ambiente]
```

- Para instalar os pacotes `python`, `numpy`, `matplotlib` e etc nesse ambiente, você pode usar o comando 
```bash
conda install python numpy matplotlib
```

## 5. Instalando driver NVIDIA (Opcional: deve ter GPU Nvidia)

No caso de seu computador ter uma GPU Nvidia, podemos instalar os drives da NVIDIA a partir do site oficial https://www.nvidia.com/pt-br/drivers/
![img/Pasted image 20250820104905.png](img/Pasted%20image%2020250820104905.png)

- Instale o programa que você baixou e isso permitirá que use o driver mais recente da NVIDIA no Windows. 
- Reinicie seu computador

- Abra um terminal do Ubuntu no WSL
- Vamos baixar o `cuda-keyring` direto do site oficial usando o comando 
```bash
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-keyring_1.1-1_all.deb
```
- Em seguida instalamos usando 
```bash
sudo dpkg -i cuda-keyring_1.1-1_all.deb
```
- E por último podemos instalar o cuda usando o comando
```bash
sudo apt-get update && sudo apt-get -y install cuda-toolkit-13-0
```
