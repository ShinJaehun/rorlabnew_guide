# 로컬머신에 "upstream" 저장소 추가하기

####로컬 .git/config 파일 세팅하기

: 각자의 로컬머신 터미널에서 rorla_api 디렉토리내에 .git/config 파일이 있는 것을 확인하고, 아래와 같이 `[remote "upstream"]` 와 그 이하를 복사해 넣습니다.

```bash
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = false
[remote "origin"]
	url = git@github.com:gitusername/rorla_api.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
[remote "upstream"]
	url = https://github.com/RORLabNew/rorla_api.git
	fetch = +refs/heads/*:refs/remotes/upstream/*
	fetch = +refs/pull/*/head:refs/remotes/upstream/pr/*
```

그리고 `sourceTree` 어플리케이션에서 확인해 보시면 `upstream` 원격저장소에 `pr` 폴더에 보면 `pull request` 번호들을 볼 수 있게 됩니다.
해당 `pull request` 번호로 `checkout` 할 때는 아래와 같이,

```bash
$ git checkout pr/23
```

하면 됩니다.
