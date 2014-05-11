# Capistrano 3 README

Gemfile에 아래와 같이 추가한다.

``` ruby
gem 'capistrano', '~> 3.1.0'
```

그리고 번들 설치한다.

``` sh
$ bundle install
```

Capify : "Capfile" 또는 "capfile" 이 없다는 것을 확인한다.

``` sh
$ bundle exec cap install
```

이제 아래와 같은 파일을 생성하게 된다.

```
├── Capfile
├── config
│   ├── deploy
│   │   ├── production.rb
│   │   └── staging.rb
│   └── deploy.rb
└── lib
    └── capistrano
            └── tasks
```

stage를 여러개 만들 때는 아래와 같이 한다.

``` sh
$ bundle exec cap install STAGES=local,sandbox,qa,production
```

사용법
---

``` sh
$ bundle exec cap -vT

$ bundle exec cap staging deploy
$ bundle exec cap production deploy

$ bundle exec cap production deploy --dry-run
$ bundle exec cap production deploy --prereqs
$ bundle exec cap production deploy --trace
```

Tasks
---

``` ruby
server 'example.com', roles: [:web, :app]
server 'example.org', roles: [:db, :workers]
desc "Report Uptimes"
task :uptime do
  on roles(:all) do |host|
    execute :any_command, "with args", :here, "and here"
    info "Host #{host} (#{host.roles.to_a.join(', ')}):\t#{capture(:uptime)}"
  end
end
```

**주의 :**

`execute(:bundle, :install)` 과 `execute('bundle install')` 은 동일하게 동작하지 않는다.

`execute()` 는 민감하게 동작을 한다. 예를 들어, `within './directory' { execute(:bundle, :install) }`와 같이 호출하면, `execute()` 의 첫번째 인수는 whitespace가 없는 문자열이 된다. 이로써 이 명령이 여러가지 강력한 기능을 제공하는 [SSHKit::CommandMap](https://github.com/capistrano/sshkit#the-command-map)을 이용할 수 있게 된다.

`execute()`의 첫번째 인수가 whitespace를 포함할 경우, 예를 들어, `within './directory' { execute('bundle install') }`(또는 heredoc을 사용할 때)과 같을 때, Capistrano나 SSHKit가 어떻게 쉘 이스케이핑을 해야할지 알 수 없게 되어 어떠한 context나 command mapping도 수행할 수 없게 되는데, 즉, `within(){ }`(`with()`, `as()` 포함)가 효력을 가지지 못하게 된다. 이러한 문제를 해결하기 위한 시도가 있어 왔지만, 약간 직관적이지 않더라도 버그라고 생각하지 않는다.

Before / After
----

 동일한 이름의 task를 호출하는 곳에서는 포함하는 순서에 따라 실행된다.

``` ruby
# 이미 정의된 task를 호출할 때
before :starting, :ensure_user

after :finishing, :notify


# 또는 블록으로 정의할 때
before :starting, :ensure_user do
  #
end

after :finishing, :notify do
  #
end
```

가끔, 파일을 생성하는 것과 같은 경우, Rake 의 prerequisite 메카니즘이 사용될 수 있다.

``` ruby
desc "Create Important File"
file 'important.txt' do |t|
  sh "touch #{t.name}"
end
desc "Upload Important File"
task :upload => 'important.txt' do |t|
  on roles(:all) do
    upload!(t.prerequisites.first, '/tmp')
  end
end
```

다른 task를 호출하는 마지막 방법은 `invoke()` 명령을 호출하는 것이다.

``` ruby
task :one do
  on roles(:all) { info "One" }
end
task :two do
  invoke :one
  on roles(:all) { info "Two" }
end
```

이 메소드는 널리 사용된다.

사용자 입력
----

``` ruby
desc "Ask about breakfast"
task :breakfast do
  ask(:breakfast, "pancakes")
  on roles(:all) do |h|
    execute "echo \"$(whoami) wants #{fetch(:breakfast)} for breakfast!\""
  end
end
```

로컬 task를 실행하기
---------

로컬 task는 `on` 대신에 `run_locally`를 사용한다.

``` ruby
desc 'Notify service of deployment'
task :notify do
  run_locally do
    with rails_env: :development do
      rake 'service:notify'
    end
  end
end
```

물론, 로컬에서 실행할 때 항상 표준 루비 문법을 사용할 수 있다.

``` ruby
desc 'Notify service of deployment'
task :notify do
  %x('RAILS_ENV=development bundle exec rake "service:notify"')
end
```

다른 방법으로는 rake 문법을 사용할 수 있다.

``` ruby
desc "Notify service of deployment"
task :notify do
   sh 'RAILS_ENV=development bundle exec rake "service:notify"'
end
```

콘솔작업
----

주의 : 위험한 영역이다. 콘솔 기능은 아직 완성되지 않았지만, 이전 모습모다는 훨씬 깔끔하게 설계되었고 먼 장래에 더 좋아질 것이다.

임의 원격 명령을 실행한다. 이것을 사용하기 위해서는 `require 'capistrano/console` 을 추가해야 하는데, 이것은 필요한 task를 환경에 추가할 것이다.

``` sh
$ bundle exec cap staging console
```

서버 연결이 된 후에 결과는 아래와 같다.

``` sh
$ bundle exec cap production console
capistrano console - enter command to execute on production
production> uptime
 INFO [94db8027] Running /usr/bin/env uptime on leehambley@example.com:22
DEBUG [94db8027] Command: /usr/bin/env uptime
DEBUG [94db8027]   17:11:17 up 50 days, 22:31,  1 user,  load average: 0.02, 0.02, 0.05
 INFO [94db8027] Finished in 0.435 seconds command successful.
production> who
 INFO [9ce34809] Running /usr/bin/env who on leehambley@example.com:22
DEBUG [9ce34809] Command: /usr/bin/env who
DEBUG [9ce34809]  leehambley pts/0        2013-06-13 17:11 (port-11262.pppoe.wtnet.de)
 INFO [9ce34809] Finished in 0.420 seconds command successful.
 ```

PTY에 대하여
------

원격 서버에게 pty로 연결하도록 지정하는 구성 옵션이 있다. pty란 pseudo-terminal을 말하며 `interactive session`임을 알려 준다. 이러한 방법은 일반적으로 좋은 아이디어는 아니다.

자세한 내용은 [이 페이지](https://github.com/sstephenson/rbenv/wiki/Unix-shell-initialization)를 참고하기 바랍니다.

Capistrano는 원격 서버와 연결할 때 `non-login`, `non-interactive` 쉘 모드로 연결된다. 이것은 우연한 것은 아니다.

로그인과 쉘 초기화 스크립트가 로딩되지 않는, RVM과 rbenv관련된,  문제를 치료하기 위해,  종종 Capistrano를  마치 대일밴드 처럼  사용한다. 이 경우 RVM과 rbenv은 잘 못된 곳을 수리하는 연장이거나 적어도 제대로 사용되지 못하는 것이다.

한편, 루비, Node, 파이썬 등과 같은 언어 런타임의 경우에 특히, 하나의 서버에 동시에 여러 개의 버전을 실행하고 환경변수를 이용해서 버전간 이동을 하고자 하는 유횩에 삐진다. 그러나 이것은 패턴에 반하는 것이고 좋지 않은 디자인 형태이기도 하다. (예를 들면, 회사의 인프라구조에는 staging 환경에서 테스트해 볼 수 없기 때문에 production 환경에서 루비 다른 버전을 테스트하는 경우)

구성
-----

아래의 변수들은 변경할 수 있다.

| 변수명         | 설명                                                          | 주의사항                                                          |
|:---------------------:|----------------------------------------------------------------------|-----------------------------------------------------------------|
| `:repo_url`           | scm 저장소(git. hg, svn)의 URL  | file://, https://, ssh://, svn+ssh:// 이 모두 지원된다.      |
| `:branch`             | 배포하고자 하는 branch 이름  | 이것은 git와 hg 저장소에 대해서만 해당한다. svn 저장소의 branch를 지정하기 위해서는 `:repo_url`을 branch 위치로 지정한다. |
| `:scm`                | 사용하는 소스관리시스템 이름  | 현재 `:git`, `:hg`, `:svn` 만 지원한다.           |
| `:tmp_dir`            | (옵션으로) 사용하는 임시 디렉토리 (디폴트 값은 /tmp임)| shared 웹호스팅을 사용하는 경우는 이 값을 변경할 필요가 있다.(예, /home/user/tmp/capistrano). |

아래의 변수들을 _더 이상 지원하지 않는다_.

| 변수명         | 설명                                                         | 주의사항                                                           |
|:---------------------:|---------------------------------------------------------------------|-----------------------------------------------------------------|
| `:copy_exclude`       | (옵션으로) 배포시 제외되는 파일이나 폴더의 배열 | Git의 `.gitattributes`로 대체함. 자세한 내용은 [#515](https://github.com/capistrano/capistrano/issues/515)를 참고한다. |

SSHKit
---

[SSHKit](https://github.com/leehambley/sshkit)는 Capistrano에서 SSH 연결을 위해서 사용시 배후에서 동작하는 드라이버이다. 더 깊이 알 수로고 SSHKit의 인터페이스에 직접 접근할 수 있다. (구성파일이 좋은 예이다)




