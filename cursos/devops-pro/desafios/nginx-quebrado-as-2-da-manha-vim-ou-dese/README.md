# Nginx quebrado às 2 da manhã: Vim ou desespero?
Você é SRE e precisa corrigir um erro de configuração no Nginx em produção via SSH, sem internet, sem VS Code, apenas com Vim e os comandos do terminal.
---
## Instrucoes
Você recebeu um alerta do PagerDuty: o serviço web está retornando 502 Bad Gateway. Ao logar no servidor de produção via SSH, você identifica que o arquivo `/etc/nginx/nginx.conf` foi alterado por um colega e agora contém um IP inválido na diretiva `proxy_pass`. O IP correto é `192.168.1.10:8080`, mas está como `192.168.1.15:8080`.

O servidor não tem internet, então você não pode instalar nada. Não há Nano instalado. Vim é sua única ferramenta. Você também não tem acesso a ferramentas gráficas ou VS Code.

**Objetivos:**
1. Abra o arquivo `/etc/nginx/nginx.conf` com Vim.
2. Localize a linha com o IP incorreto (use busca com `/`).
3. Corrija o IP para `192.168.1.10:8080`.
4. Faça um backup do arquivo original antes de editar (`cp nginx.conf nginx.conf.bak`).
5. Salve e saia do Vim corretamente.
6. Valide que a configuração está sintaticamente correta com `nginx -t`.
7. Recarregue o Nginx com `nginx -s reload`.

Não use `nano`. Não use `sed` ou `awk` — você está em modo sobrevivência, e Vim é o único editor disponível. Se errar, use `u` para desfazer. Se se perder, aperte `Esc` até voltar ao Modo Normal.

Entregue em: linuxtips-workspace/desafios/modulo3/nginx-fix/

(Dica: O arquivo já existe no sistema. Não crie nada novo. Apenas corrija e salve.)
## Arquivos para editar

- `etc/nginx/nginx.conf`

## Conteudo inicial dos arquivos

### `etc/nginx/nginx.conf`

```yaml
user nginx;
worker_processes auto;

error_log /var/log/nginx/error.log;
pid /run/nginx.pid;

events {
    worker_connections 1024;
}

http {
    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log /var/log/nginx/access.log main;

    sendfile            on;
    tcp_nopush          on;
    tcp_nodelay         on;
    keepalive_timeout   65;
    types_hash_max_size 2048;

    include             /etc/nginx/mime.types;
    default_type        application/octet-stream;

    server {
        listen       80;
        server_name  localhost;

        location / {
            proxy_pass http://192.168.1.15:8080;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
        }
    }
}
```

---
*Desafio LINUXtips - devops-pro*