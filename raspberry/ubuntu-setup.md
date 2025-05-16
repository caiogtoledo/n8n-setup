# 🚀 Instalação do n8n no Raspberry Pi 3 B+ com Ubuntu Server

Este guia mostra como instalar e configurar o [n8n](https://n8n.io/) usando Docker em um Raspberry Pi 3 B+ com Ubuntu Server.

> 💡 Requisitos:
> - Raspberry Pi 3 B+ com Ubuntu 20.04 ou superior
> - Acesso à internet
> - Docker e Docker Compose instalados

---

## 📦 1. Atualize o sistema

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 🐳 2. Instale Docker

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

Adicione seu usuário ao grupo `docker` para evitar o uso do `sudo`:

```bash
sudo usermod -aG docker $USER
newgrp docker
```

---

## 🧩 3. Instale Docker Compose

```bash
sudo apt-get update
sudo apt-get install docker-compose-plugin
```

Verifique:

```bash
docker compose version
```

> ✅ Use `docker compose` (sem hífen) com essa versão.

---

> 💡 Útil para manter compatibilidade com tutoriais que usam `docker-compose` com hífen.


---

## 📁 4. Crie o diretório do projeto n8n

```bash
mkdir ~/n8n && cd ~/n8n
```

---

## 📝 5. Crie o arquivo `.env`

```env
# .env
N8N_BASIC_AUTH_ACTIVE=true
N8N_BASIC_AUTH_USER=admin
N8N_BASIC_AUTH_PASSWORD=strongpassword
N8N_HOST=raspberrypi.local
N8N_PORT=5678
```

> 🔒 Altere `admin` e `strongpassword` conforme desejar para proteger sua instância.

---

## 🐳 6. Crie o arquivo `docker-compose.yml`

```yaml
version: '3'

services:
  n8n:
    image: n8nio/n8n:latest
    restart: always
    ports:
      - "5678:5678"
    env_file:
      - .env
    volumes:
      - ./n8n_data:/home/node/.n8n
    environment:
      - TZ=America/Sao_Paulo
```

> ⚠️ Raspberry Pi 3 usa arquitetura ARMv7, e a imagem oficial `n8nio/n8n:latest` já é compatível com ARM.

---

## ▶️ 7. Suba os containers

```bash
docker-compose up -d
```

Verifique se está funcionando:

```bash
docker ps
```

---

## 🌐 8. Acesse o n8n

Abra o navegador e acesse:

```
http://<ip_do_raspberry>:5678
```

Use o usuário e senha definidos no `.env`.

---

## ✅ 9. (Opcional) Iniciar no boot

Adicione o serviço Docker Compose para iniciar com o sistema:

```bash
sudo crontab -e
```

Adicione ao final:

```bash
@reboot cd /home/<usuario>/n8n && /usr/local/bin/docker-compose up -d
```

---

## 🔐 10. (Opcional) Habilite HTTPS com Nginx + SSL (Let's Encrypt)

Se quiser acesso seguro (HTTPS), use um proxy reverso com Nginx e certbot. Posso te ajudar com isso também!

---

## 🧹 11. Parar e remover

Para parar o serviço:

```bash
docker-compose down
```

---

## 🛠️ Dicas

- Para atualizar o n8n:

```bash
docker-compose pull
docker-compose up -d
```

- Os fluxos são salvos em `~/n8n/n8n_data`.

---

## 📚 Referências

- [n8n Documentation](https://docs.n8n.io)
- [Docker Compose](https://docs.docker.com/compose/)
