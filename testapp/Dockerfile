FROM minhvo/centos7-jre:1.8.121

MAINTAINER  Minh VO <minhvv91@gmail.com>

WORKDIR /var/opt/testapp

ADD . .

EXPOSE 8080

CMD java -cp ./testapp.jar kixeye.testapp.Main
