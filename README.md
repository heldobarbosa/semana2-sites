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
