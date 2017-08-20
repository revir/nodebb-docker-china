# NodeBB Docker China
为中国人打造的 NodeBB Docker 构建环境，利用 Docker 部署，使用简单，一键安装适合国人使用的 NodeBB 论坛。  
主要特性如下：

* 使用 NodeBB 最新版 v1.5.x；
* 使用 MongoDB 作为数据库；
* 初始化安装了几个特别方便的 NodeBB 插件：
  * nodebb-plugin-composer-redactor：所见即所得编辑器，默认启用，如果需要启用 Markdown 编辑器，
  可以在后台禁用 Redactor 插件，启用 Default Composer 和 Markdown 插件；
  * nodebb-plugin-search-elasticsearch： 方便中文分词和搜索，默认的 NodeBB 搜索不支持中文

# 安装 docker
见docker官方的 tutorial: https://docs.docker.com/get-started/

# 准备工作

* 首先把你的账号加入docker group，然后重新登陆令其生效，否则 docker 命令要用 sudo 运行。

    sudo usermod -aG docker ${USER}

* 在 docker build 之前，先清理以前的 contailner:

    docker rm $(docker ps -a -q)

* 删除以前的 mongo-seed image, 以前的 mongo-seed 数据可能过时了，需要重新 build image:

    docker rmi mongo-seed

* 如果 mongodb 的数据需要更新，那么需要删除 mongodb 的 volume:

    docker volume rm mongodata

* 如果 mongodb 的数据更新了，那么 elasticsearch 的数据最好也更新:

    docker volume rm esdata

# 运行

    docker-compose up

# Build (Optional)

* mongodb

    docker run -d -p 27017:27017 -v mongodata:/data/db -v mongoconfig:/data/configdb --name=mongodb mongo

* 导入mongo-seed

    docker run --link=mongodb mongo-seed

* elasticsearch23

    docker run -d -p 9200:9200 -v esdata:/usr/share/elasticsearch/data --name=elasticsearch23 elasticsearch23

* nodebb web

    docker run --link=mongodb --link=elasticsearch23 -v nodebb:/usr/src/app/nodebb --name=nodebb -it -p 4567:4567 nodebb
