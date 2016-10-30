docker run -d --name busybox   --restart=always  -v /work/dockers-max/data/mysql:/var/lib/mysql -v /work/dockers-max/db-back:/db busybox
docker run -d --name mysql     --restart=always  --volumes-from busybox -p 3306:3306   -e MYSQL_ROOT_PASSWORD=mmmm    mysql
docker run -d --name myadmin   --restart=always  --link mysql:db        -e PMA_ARBITRARY=1 -e PMA_HOST=db  -p 8080:80 phpmyadmin/phpmyadmin
docker run -d --name xraymove  --restart=always  --link mysql:mysql     -e  WORDPRESS_DB_PASSWORD=mmmm  -e WORDPRESS_DB_NAME=xraymove  -e VIRTUAL_HOST=xray.max.com wordpress
docker run -d --name zwzwen    --restart=always  --link mysql:mysql     -e  WORDPRESS_DB_PASSWORD=mmmm  -e WORDPRESS_DB_NAME=zwzwen  -e VIRTUAL_HOST=zwz.max.com wordpress


docker run -d  --restart=always -p 80:80 --name proxy -v /var/run/docker.sock:/var/run/docker.sock -e DOCKER_HOST=unix:///var/run/docker.sock jwilder/nginx-proxy
docker run -d  --name mysql-backup --restart=always -e DB_USER=root -e DB_PASS=mmmm -e DB_DUMP_FREQ=3600 -e DB_DUMP_BEGIN=+0 -e DB_DUMP_TARGET=/db --link mysql:db --volumes-from busybox deitch/mysql-backup
