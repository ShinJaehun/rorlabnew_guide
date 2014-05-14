# GitBook 저장소를 gh-pages로 배포하기

> 참고 : http://stackoverflow.com/a/23095566

한마디로, `grunt`를 이용하여 `publish`한다.

## 준비작업

이를 위해서 로컬 시스템에 `npm`이 설치되어 있어야 한다. 구글 검색을 이용하면 쉽게 [`npm 설치`](http://shapeshed.com/setting-up-nodejs-and-npm-on-mac-osx/)하는 방법을 찾을 수 있다.

## 추가파일

`Gruntfile.js`과 `package.json` 두개의 파일을 프로젝트 루트에 생성하여 아래와 같이 작성한다.

### Gruntfile.js

```
var path = require("path");

module.exports = function (grunt) {
    grunt.loadNpmTasks('grunt-gitbook');
    grunt.loadNpmTasks('grunt-gh-pages');
    grunt.loadNpmTasks('grunt-contrib-clean');
    grunt.loadNpmTasks('grunt-http-server');

    grunt.initConfig({
        'gitbook': {
            development: {
                input: "./",
                title: "GitBook 제목",
                description: "간단한 설명",
                github: "git_account/gitbook_prj"
            }
        },
        'gh-pages': {
            options: {
                base: '_book'
            },
            src: ['**']
        },
        'clean': {
            files: '_book'
        },
        'http-server': {
            'dev': {
                // the server root directory
                root: '_book',

                port: 4000,
                host: "127.0.0.1",

                showDir : true,
                autoIndex: true,
                defaultExt: "html",

                //wait or not for the process to finish
                runInBackground: false
            }
        }
    });

    grunt.registerTask('test', [
        'gitbook',
        'http-server'
    ]);
    grunt.registerTask('publish', [
        'gitbook',
        'gh-pages',
        'clean'
    ]);
    grunt.registerTask('default', 'gitbook');
};
```

여기서 `grunt.initConfig`에서 `title`, `description`, `github` 항목을 해당 gitbook 프로젝트에 맞게 변경한다.

### package.json

```
{
    "name": "Gitbook Project Name",
    "version": "0.0.1",
    "description": "",
    "repository": {
        "type": "git",
        "url": "https://github.com/git_account/gitbook_prj.git"
    },
    "author": "author_name <author@email.com>",
    "license": "Apache 2",
    "dependencies": {},
    "devDependencies": {
        "grunt": "~0.4.1",
        "grunt-gitbook": "0.4.2",
        "grunt-gh-pages": "0.9.1",
        "grunt-contrib-clean": "~0.5.0",
        "grunt-http-server": "0.0.4"
    },
    "peerDependencies": {
        "grunt": "~0.4.1"
    }
}
```

위의 내용 중에 해당 프로젝트에 맞도록 항목을 변경한다.

## 배포하기

자, 이제 커맨드라인에서 아래와 같이 노드 패키지를 설치한다.

```
$ npm install .
```

제대로 동작하는 것을 테스트하기 위해서는 아래와 같이 실행한다.

```
$ grunt test
```

마지막으로 `gh-pages` 브랜치로 배포한다.

```
$ grunt publish
```

## Github 서브도메인으로 확인하기

Github 웹사이트에서 해당 프로젝트로 이동한 후 `master`외에 `gh-pages` 브랜치가 생성되었는지를 확인한다. 그리고 웹페이지의 오른쪽의 `Setting` 아이콘을 클릭하여 이동한 후 `Github Pages` 박스를 보자. 아마도 10분후에 반영된다는 안내 메시지와 함께 배포된 사이트 주소링크가 보일 것이다. 10분후 이 링크를 클릭하면 배포된 사이트에서 Gitbook이 제대로 보여야 한다.

# Gitbook에 Disqus 댓글기능 추가하기

Gitbook의 모든 페이지 하단에 Disqus 댓글기능을 추가할 수 있다.
https://github.com/GitbookIO/plugin-disqus 를 참고하여 정리하면 아래와 같다. (`$ gitbook build ./ --plugins=disqus`는 할 필요없음)

`npm`을 이용하여 disqus를 위한 플러그인을 설치한다.

```
$ npm install gitbook-plugin-disqus
```

그리고 `book.json` 파일을 열어 아래와 같이 추가한다. 없으면 생성하면 된다.

```
{
  "plugins": ["disqus"],
  "pluginsConfig": {
    "disqus": {
      "shortName": "xxxxxx"
    }
  }
}
```

이제 테스트를 해 본다.

```
$ grunt test
```

제대로 동작하면 서버로 배포한다.

```
$ grunt publish
```

그리고 소스는 원격 저장소로 커밋후 푸시한다.

```
$ git add .
$ git commit -m "disqus 플러그인 설치"
$ git push
```

# Gitbook에 Google Analytics 연결하기

[참고] https://github.com/GitbookIO/plugin-ga

```
$ npm install gitbook-plugin-ga
```

그리고 `book.json` 파일을 열어 플러그인에 아래와 같이 `ga` 플리그인을 추가한다. 없으면 새로 생성하면 된다.

```
{
  "plugins": ["disqus", "ga"],
  "pluginsConfig": {
    "disqus": {
      "shortName": "xxxxxx"
    },
    "ga": {
      "token": "UA-XXXX-Y"
    }
  }
}
```

> **[주의사항]** `token`에는 Google Analytics tracking ID 값을 지정한다. `tacking ID` 값은 [계정에서 추적 코드 및 속성 ID 찾기](계정에서 추적 코드 및 속성 ID 찾기)를 참고하면 알 수 있다.

이제 테스트를 해 본다.

```
$ grunt test
```

제대로 동작하면 서버로 배포한다.

```
$ grunt publish
```

그리고 소스는 원격 저장소로 커밋후 푸시한다.

```
$ git add .
$ git commit -m "Google Analytics 플러그인 설치"
$ git push
```

# GitBook에 RichQuotes 플러그인 추가하기

[참고] https://github.com/erixtekila/gitbook-plugin-richquotes

```
$ npm install gitbook-plugin-richquotes
```

그리고 `book.json` 파일을 열어 플러그인에 아래와 같이 `richquotes` 플리그인을 추가하고 플러그인설정에도 옵션을 추가한다. 없으면 새로 생성하면 된다.

```
{
  "plugins": ["disqus", "ga", "richquotes"],
  "pluginsConfig": {
    "disqus": {
      "shortName": "xxxxxx"
    },
    "ga": {
      "token": "UA-XXXX-Y"
    },
    "richquotes": {
      "todos" : true
    }
  }
}
```

주석표기는 마크다운 블록표기를 확장한 기능이다. 지원되는 주석문자는 아래와 같다.

* `> **Info** Info`
* `> **Note** Note`
* `> **Tag** Tag`
* `> **Comment** Comment`
* `> **Hint** Hint`
* `> **Success** Success`
* `> **Warning** Warning`
* `> **Caution** Caution`
* `> **Danger** Danger`
* `> **Quote** Quote`

주석문자는 대소문자를 가리지 않는다.

또한 편집 목적으로 아래와 같은 특수문자를 사용할 수도 있다.

* `> **todo** TODO`
* `> **fixme** FIXME`
* `> **xxx** XXX`

Preview 모드에서는 주석이 영문글자로 보이지만 실제 배포시에는 아래와 같이 보이게 된다.

> **Info** Info

---

> **Note** Note

---

> **Tag** Tag

---

> **Comment** Comment

---

> **Hint** Hint

---

> **Success** Success

---

> **Warning** Warning

---

> **Caution** Caution

---

> **Danger** Danger

---

> **Quote** Quote

---

> **todo** TODO

---

> **fixme** FIXME

---

> **xxx** XXX




