semana2-sites
Trabalhando com NGINX
Estes são os arquivos .conf que adicionei em /etc/nginx/conf.d 
Usei a opção de colocar em /conf.d pela quantidade de sites pequena nesse projeto. 

Por padrão:

server {
    listen       [PORTA];
    server_name  [DOMÍNIO DOS SITES];

    # note that these lines are originally from the "location /" block
    root   /var/www/html #[CAMINHO AO QUAL O NGINX DEVE BUSCAR O ARQUIVO A SER CARREGADO NA PÁGINA];
    index index.html index.htm; #[TIPOS DE ARQUIVO QUE SE LÊ]
    access_log /var/log/nginx/site/access.log; #[CAMINHO PARA OS LOGS CASO TENHA ERRO]
    error_log /var/log/nginx/site/error.log; #[CAMINHO PARA OS LOGS CASO TENHA ERRO]

    location / {
        try_files $uri $uri/ =404;
    }
    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;
    location = /50x.html {
        root /usr/share/nginx/html;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass unix:/var/run/php/php-fpm.sock; #CAMINHO DO SOCK QUE É USADO PARA CONEXÃO DO NGINX COM PHP
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}

Como foi muito simples, em /var/www/html ficaram os arquivos responsáveis por cada página do site, ou seja, apenas no "site" houve um arquivo .html simples. No BLOG, coloquei os arquivos do WordPress descompactados, para que a página fosse carregada diretamente pelo domínio correspondente. 

Nas demais coisas feitas, temos alguns detalhes como: 

- Foi usado um mesmo banco mysql para o BLOG e a LOJA.
- É importante dar permissão ao NGINX em todos os processos. Exemplo: "chown -R nginx:nginx /var/lib/php/session/"
- O magento não aceita a última versão do PHP, no caso usei a 7.0.
- É importante manter o Firewall desativado para conexão das portas, ou ainda, instalar o Firewalld para adicionar permissões de porta. 
- Foi muito importante o uso de logs para obter um histórico do que aconteceu em cada erro. 

Deixo aqui as fontes que usei para montar o ambiente:

https://www.howtoforge.com/tutorial/how-to-install-magento-2-1-on-centos-7/
https://darrenoneill.eu/?p=455


