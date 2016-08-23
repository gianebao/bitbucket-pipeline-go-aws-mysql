# bitbucket-pipeline-go-aws-mysql
An inspiration from: [smartapps/bitbucket-pipelines-php-mysql](https://hub.docker.com/r/smartapps/bitbucket-pipelines-php-mysql/)
Extended from:
[gianebao/bitbucket-pipeline-go-mysql](https://hub.docker.com/r/gianebao/bitbucket-pipeline-go-mysql)

[Bitbucket Pipelines](https://bitbucket.org/product/features/pipelines) [Go/Golang](https://golang.org/) [Amazon AWS](https://aws.amazon.com/cli/) [MySQL](https://www.mysql.com) (with built in [Goose Migrator](https://bitbucket.org/liamstask/goose/))

Docker image based on [golang](https://hub.docker.com/_/golang/)
Docker image at [gianebao/bitbucket-pipeline-go-aws-mysql](https://hub.docker.com/r/gianebao/bitbucket-pipeline-go-aws-mysql/)

Built in's:
  - ENV GOOSE_DIR
  - ENV MYSQL_ROOT_PASSWORD
  - mysql-server (`/etc/init.d/mysql start`)
  - redis (`/etc/init.d/redis-server start`)
  - goose (`cp -v ./migrations $GOOSE_DIR/migrations && cd / && goose -env=test up`)
  - mysql-client (`mysql --user=root --password=$MYSQL_ROOT_PASSWORD -e "CREATE DATABASE test;"`)
  - aws (`aws help`)
Sample `bitbucket-pipelines.yml`:

```YAML
image: gianebao/bitbucket-pipeline-go-aws-mysql
pipelines:
  default:
    - step:
        script:
          - /etc/init.d/mysql start
          - mysql --user=root --password=$MYSQL_ROOT_PASSWORD -e "CREATE DATABASE test;"
          - cp -v ./migrations $GOOSE_DIR/migrations && cd / && goose -env=test up
          - aws deploy push --application-name MyGo_App --description "This is my deployment" --ignore-hidden-files --s3-location s3://CodeDeployDemoBucket/MyGoApp.zip --source /tmp/MyLocalDeploymentFolder/
```
