# AC-REDES-005

## Instalação do OpenVPN no docker

Com o `docker` instalado e configurado, executei o comando:

```bash
docker run -d --name openvpn-as --cap-add=NET_ADMIN -v openvpn_data:/etc/openvpn -e ADMIN_USER=openvpn -e ADMIN_PASSWORD=redes@123456 -p 943:943 -p 9443:9443 -p 1194:1194/udp openvpn/openvpn-as
```

Apesar do comando contar com variáveis de ambiente para usuário e senha do admin o `OpenVPN` gerou uma nova senha, a qual obtive nos logs do container com o comando

```bash
docker logs -f openvpn-as
```

Com isso, já posso acessar o painel web do `OpenVPN`

## Configurando o OpenVPN

Acessei o painel web na url `https://<IP-SERVER>:943/admin`, e usei o usuário e senha obtidos no passo anterior para logar.

No caminho `https://20.197.241.156:943/admin/activation` segui os passos para obter a chave de ativação do software e ativar o `OpenVPN`.

No caminho `https://20.197.241.156:943/admin/network_settings` configurei o hostname para o ip da máquina (por padrão vem o ip interno da rede docker, que não é acessível de fora da rede) para usar o ip do servidor.

No caminho `https://20.197.241.156:943/admin/advanced_vpn` fui para a seção `Inter-Client Communication` e ativei a opção `Allow multiple concurrent VPN connections for a user (automatically disabled when static VPN IP addresses are configured for users)` para que uma máquina conectada a VPN conseguisse acessar outra.

## Conctando na VPN

### Linux

Naveguei para `https://20.197.241.156:943/` e obtive o perfil de conexão (arquivo .ovpn)

No linux, usei o comando

```bash
sudo openvpn --config ./profile-userlocked.ovpn
```

para me conectar a VPN na minha máquina pessoal (openvpn client precisa ser instalado)

### Android

Naveguei para `https://20.197.241.156:943/` e obtive o perfil de conexão (arquivo .ovpn)

Baixei o aplicativo OpenVPN e usei o arquivo .ovpn para me conectar

## Teste de conexão pela rede VPN

Na máquina linux servi um HTML com `serve` (server precisa ser instalado) na porta 3000

No android, acessei o ip interno da VPN da máquina linux obtido em `https://20.197.241.156:943/admin/current_users` na porta 3000 e visualizei o HTML servido.
