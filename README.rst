Continuous Integration with Docker Compose
==========================================


Docker Compose
--------------

도커 컴포즈는 컨테이너 기반의 오케스트레이션 툴로서 연결된 서비스들로 구성된다.
쿠버네틱스나 스윔과 같은 클러스터 기반의 툴과 달리, 싱글 도커 호스트의 오케스트레이션을 목적으로 한다.


Continuous Integration
----------------------
Docker CI?

  CI단계는 일반적인으로 개발자의 커밋으로 부터 시작하고 결과적으로 성공적으로 테스트된 아티팩트를 목적으로 한다.
  컨테이너 서비스의 경우 이것은 도커 이미지가 된다. 
  만약 개발자가 포괄적인 개발 환경을 제공받는다면, 이것은 CI환경에서도 동일하게 적용가능하다.

Docker CI Test?

  CI는 단순히 도커 이미지를 생성하는 것 뿐아니라 새로운 코드의 무결성을 검증하는 테스트를 실행하는 단계로도 사용할 수 있다.
  Docker Compose CLI 명령어의 인자로서 다수의 docker-compose.yml파일을 사용할 수 있다. 
  이것은 파일 순서에 따라 서비스의 정의가 변경될 수 있음을 의미한다. 

docker-compose.yml

.. code-block:: text

  services:
    web:
      image: web:1.0
      ports:
        - "5000:5000"

docker-compose.test.yml

.. code-block:: text

  services:
    web:
      build:
        context: .
        dockerfile: Dockerfile.test
      image: web:${tag:-latest}


command-line

.. code-block:: text

  $ docker-compose -f docker-compose.yml -f docker-compose.test.yml up web -d

부연설명

  만약, docker-compose  안에 image와  build가 같이 포함되어 있다면, docker compose는 image의 값으로 이미지 이름을 부여한다.
  아래 예에서는 "webapp:tag"가 이미지 이름이 될 것이다.

.. code-block:: text

  build: ./dir
  image: webapp:tag


Reference
---------
- https://semaphoreci.com/blog/2017/09/13/continuous-integration-with-docker-compose.html