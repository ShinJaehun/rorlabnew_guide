# 저장소를 포킹하여 Pull Request하기

* github 웹사이트를 방문해서 본인의 계정으로 로그인합니다.

* 페이지 상단의 검색창에서 `rorla_api` 라고 입력 후에 검색하면 아래와 같은 결과가 보일 것입니다.

    ![](http://i1373.photobucket.com/albums/ag392/rorlab/p1_zps1517a3dc.png)

* 링크를 클릭하여 접속한 후 웹페이지의 우측 상단에 있는 `Fork` 버튼을 클릭합니다.

    ![](http://i1373.photobucket.com/albums/ag392/rorlab/2014-01-20_19-06-59_zps1e683196.png)

* 잠시 후에 본인의 계정에 저장소가 복사되고 좌측 상단의 저장소 이름이 아래와 같이 보이게 됩니다.

    ![](http://i1373.photobucket.com/albums/ag392/rorlab/2014-01-20_19-10-47_zpsa4930128.png)

* 이제 본인 계정으로 fork한 저장소를 로컬머신으로 cloning 합니다.

    ![](http://i1373.photobucket.com/albums/ag392/rorlab/2014-01-20_19-17-41_zps676b7e05.png)

* 웹페이지의 우측 컬럼에 위와 같이 clone URL을 볼 수 있습니다. 복사 아이콘을 클릭하면 주소를 복사할 수 있습니다.
이제 로컬머신의 터미널에서 아래와 같이 git clone 합니다.

    ![](http://i1373.photobucket.com/albums/ag392/rorlab/2014-01-20_19-20-14_zpsd31afea7.png)

* 그리고나서 프로젝트 디렉토리로 이동합니다.

    ```bash
    $ cd rorla_api
    rorla_api $>
    ```

* 이제부터 코딩 작업을 계속하게 됩니다. 본인이 의도한 데로 코딩 작업이 완료되면 변경 내용을 `staging`한 후에 커밋합니다.

    ```bash
    rorla_api $> git add .
    ```

    팁) 삭제된 파일을 포함해서 `staging`할 경우에는 `git add -A` 와 같이 `-A` 옵션을 사용하면 편리합니다.

    ```bash
    rorla_api $> git commit -m “커밋 메시지를 작성합니다”
    ```

* 이제 본인 계정의 원격 저장소로 푸시합니다.

    ```bash
    rorla_api $> git push origin master
    ```

    팁) `git push -u origin master` 와 같이 `-u` 옵션을 한번만 사용하면 이후부터는 `git push` 라고만 실행하면 자동으로 `origin` 저장소의 `master` 브랜치로 푸시됩니다.

* 이제 `github` 웹사이트의 본인 저장소에서 방금 전에 푸시한 커밋을 `upstream` 저장소(원본 저장소)로 `pull request` 하므로서 머지 요청을 하게 됩니다.

    ![](http://i1373.photobucket.com/albums/ag392/rorlab/2014-01-20_19-32-38_zps87a3ce51.png)

* 그림에서와 같이 웹페이지의 우측 상단부에 있는 `Pull Request` 링크를 클릭하면 됩니다.

지금까지 `forking` 한 후 `pull request` 하는 과정을 소개했습니다.

작성자 : 최효성
