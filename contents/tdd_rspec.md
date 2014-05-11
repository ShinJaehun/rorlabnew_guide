# TDD with RSpec

오픈소스 프로젝트로 진행되는 만큼 소스의 검증과정이 필수적입니다. 최근에는 이러한 테스트 환경을 쉽게 구축할 수 있어 보다 견고한 어플리케이션을 작성할 수 있습니다.

알다시피, 레일스 프레임워크를 이용하여 웹어플리케이션을 개발할 때는 이러한 테스트환경이 디폴트로 지원되기 때문에, 개발자는 손쉽게 테스트 주도하에 개발(`Test-Driven Development`)을 진행할 수 있습니다.

최근에는, 레일스 프레임워크에서 디폴트로 지원하는 테스트 프레임워크보다는, [`RSpec`](https://github.com/rspec/rspec)젬을 이용한 테스트를 선호하고 있습니다.

이미 리뉴얼저장소에는 `RSpec`젬을 이용한 테스트환경을 구축해 놓은 상태이므로 그냥 spec 파일을 작성하고 테스트하면 됩니다.

테스트 데이터를 효과적으로 사용하기 위해서 [`factory_girl`](https://github.com/thoughtbot/factory_girl) 젬도 설치되어 환경설정이 되어 있으므로 `spec/factories` 디렉토리에 `factory` 데이터를 작성하면 바로 사용할 수 있습니다.

또한 [`faker`](https://github.com/fzaninotto/Faker) 젬도 설치되어 있으므로 랜덤 데이터를 손쉽게 사용할 수 있습니다.

아래는 `로라` 프로젝트의 테스트 환경을 구축하기 위해서 작성한 Gemfile의 일부를 소개합니다.

```
group :development do
  gem 'annotate'
  gem 'better_errors'
  gem 'binding_of_caller'
  gem 'letter_opener'
end

group :development, :test do
  gem "rspec-rails", "~> 2.14.0"
  gem "factory_girl_rails", "~> 4.2.1"
  gem 'guard', '~> 2'
  gem 'guard-rspec',require: false
  gem 'spring'
  gem "spring-commands-rspec"
end

group :test do
  gem "faker", "~> 1.1.2"
  gem "capybara", "~> 2.1.0"
  gem "database_cleaner", "~> 1.0.1"
  gem "shoulda-matchers"
end
```
