### Instalação do NGINX 

1. Instale o pacote do NGINX no sistema operacional (no meu caso o ubuntu):
```bash
  sudo apt-get update
  sudo apt-get install nginx
```
2. Após a instalação, verifique se o mesmo está sendo executado:
```bash
systemctl status nginx
```
![saida](https://i.imgur.com/iRf896E.png)

> Caso o serviço não esteja rodando, execute o comando: `sudo service nginx start` e faça a verificação novamente.

### Configurando o NGINX

1. Abra o arquivo `default` localizado em `/etc/nginx/sites-available` com seu editor de texto padrão e crie o direcionamento para sua aplicação:

>  A boa prática na criação de redirecionamento, seria a de criar arquivos específicos para cada aplicação, e não diretamente no arquivo `default`, como tenho apenas uma aplicação rodando, faremos as alterações diretamente nele.

![link](https://i.imgur.com/6UWlfnE.png)
	- Em `server_name` coloque seu domínio.
	- Dentro do bloco `location` adicione o comando `proxy_pass` que cuidará de encaminhar toda requisição que chegar na raiz do seu domínio ( que é marcada pelo `/`, ou qualquer outro caminho que você desejar, por exemplo `/api`) para o endereço local da sua aplicação.

### Instalação do Certbot

1. Adicione o repositório do certbot ao seu sistema:
```bash
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
```

2. Adicione o pacote do certbot para o NGINX:
```bash
sudo apt-get install python-certbot-nginx
```
3. Logo após execute o comando a seguir para gerar seus certificados:
```bash
sudo certbot --nginx -d api.seusistema.com.br
```
> O setup ira solicitar um e-mail válido para atribuir o certificado, e mais algumas confirmações, você deve aceitar essas etapas para a criação do certificado. 

4. Após essas etapas seu domínio será configurado para HTTPS.
___
### *Bônus: 
### Configurando renovação automática de certificado

1. Ainda no seu terminal digite `crontab -e`
2. No final do documento aberto, cole o comando abaixo:
```bash
00 00 * * * sudo certbot renew
```

>Esse comando faz com que todos os dias as 00:00 seja executado um comando para verificar, e caso necessário, renovar o certificado.

> Mais detalhes sobre o funcionamento do cron nesse [link.](https://www.todoespacoonline.com/w/2015/11/cron-e-crontab-no-linux/)
3. Salve o documento e feche o arquivo, assim, adicionando uma tarefa para verificação diária do certificado.
