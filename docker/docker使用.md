#### ��������
##### ʹ��pull�������ؾ���
##### ʹ��image�����г�����Ŀ¼
##### run��������
�����ʽ run <ѡ��> <��������> <Ҫ���е��ļ�>

    docker -i -t -d --name myos ubuntu /bin/bash

> -i��-t ѡ����������е�bash���������
> -d ��̨����
> -e �������� �� -e PATH=$PATH:/opt/bin/
##### ps����鿴
> -a �г�ȫ��
##### start��������
##### stopֹͣ����
##### exec���ⲿ��������������

    docker exec -it xxx /bin/bash

##### rmɾ������
##### rmiɾ������

#### ��дDockerfile
> FROM: ָ����������
> MAINTAINER: ά������Ϣ
> RUN: ����shell�ű���������
> VLUME: �����������Ŀ¼
> WORKDIR: ΪCMD�����ÿ�ִ���ļ���Ŀ¼
> EXPOSE: �����������Ķ˿ں�

##### ʹ��build���������
�ڱ���Dockerfile��Ŀ¼��ִ��

    docker build <ѡ��> <Dockerfile·��>
    docker build --tag xx:xx .

#### ʹ��history�鿴������ʷ
#### ʹ��commit������������޸��д�������

     docker commit <ѡ��> <��������> <��������>:<��ǩ>
     docker commit -a "Foo Bar <foo@bar.com>" -m "add" hello-nginx nginx:0.2

#### ʹ��diff�����������ļ����޸�
#### docker���ݾ�
-mount ʹ�����ݾ�֧���������͵����ݾ�
> volume: ��ͨ���ݾ�ӳ�䵽����/var/lib/docker/volumes·����
> bind: �����ݾ� ӳ�䵽����ָ��·����
> tmpfs: ��ʱ���ݾ�ֻ�����ڴ���

    docker run -d -P --name web --mount type=bind,source=/webapp,destination=/opt/webbapp <������> python app.py

����������ھɵ�-v���

     -v <����Ŀ¼>:<����Ŀ¼>
     docker run -d -P --name web -v /webapp:/opt/webapp <������> python app.py
#### �鿴��������

    docker top <��������>

##### �鿴��������

    docker container inspect <��������>

##### �鿴ͳ����Ϣ
cpu���ڴ桢�洢������ͳ����Ϣ
   
    docker stats <��������>

#### �鿴�˿�ӳ��

    docker container port <��������>


    
