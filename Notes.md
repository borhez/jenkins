# jenkins

Нужен для конвейеров/заданий. Они могут создаваться в веб-формах(старый вариант) и с помощью предметно-ориентированного языка DSL Jenkins, основанного на Groovy. Описать конвейер можно как в веб-форме, так и в Jenkinsfile, применять git для него, т.е. использовать принцип "пайплайн как код". Код в Jenkinsfile также можно вызвать из других библиотек, или применить DSL-операторы.  

## install on debian 11 + jdk17  
jenkins use port 8080  
требует установки jdk8+ или хотя бы jre (запускает java-программы)  
Проверить установку можно в браузере http://localhost:8080/ , там понадобится unlock сделать jenkins, по инструкции там же  

## Jenkins job builder  
Это python-утилита, позволяющая описывать задачи в YAML- или JSON форматах, которые через HTTP API загружаются на сервер. Для работы достаточно установить ее на локальной машине, написать конфигурацию с подключением и описать требуемые задачи.  

## docker compose  
чтобы запускать команды docker compose в сценариях сборки, надо:  
- через настройки Jenkins установить плагин 'Docker Compose Build Step Plugin'  
- добавить пользователя jenkins  в группу  docker:  (sudo)usermod -aG docker jenkins  
- restart сервер Jenkins

## work with github:     
- нужны установленные плагины: Git, Publish Over SSH ,  SSH Build Agents plugin  , SSH server  
в пользователе jenkins сгенерить ключи: su jenkins ; ssh-keygen ;  
в jenkins/credentials на localhost:8080  заполняем private key, а в github копируем public key  
для jenkins/job выбираем Git, ссылку на проект GitHub, и выбираем нужный credential.  
поменять ветку в веб-форме в jenkins т.к. в гитхабе по умолчанию main, а в дженкинсе */master  
также если не соединяется, можно run the following command on the Jenkins server, as the Jenkins user, to get a proper  
known_hosts file: ssh -T git@github.com  

### triggers for github 
- Для автотриггера webhook from github, нужно чтобы сервер jenkins имел ip не локальный (не 192.168...),  
иначе триггер на гитхаабе срабатывает, отправляет htpp запрос на некий локальный адрес 192.168... и   
естественно получает код ошибки 50х (проблема со стороны jenkins)   
- для триггера опроса github задаем расписание: напр, * * * * * , каждую минуту.  
- триггер remotly, придумываем сами token=TOKEN_NAME для проекта сборки.  
  Затем в залогиненной сессии пользователя создавшего этот job можем запускать ссылку в браузере:  
   JENKINS_URL/job/JON_NAME/build?token=TOKEN_NAME ; переход по этой ссылке запускает job  
  Чтобы передать эту ссылку для curl, нужен apiToken авторизации пользователя. Создать его можно здесь:  
  http://jenkinsMasterURL/me/configure  ;затем из терминала можно запускать job:
  curl -u username:apiToken "JENKINS_URL/job/JOB_NAME/build?token=TOKEN_NAME"
  











 
