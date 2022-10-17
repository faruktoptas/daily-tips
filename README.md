# daily-tips

### 1. Configure Android lint to generate single report output for a module and its dependencies

```groovy
android {
  lint {
        checkDependencies true
        baseline file("lint-baseline.xml")
        setLintConfig(file("../lint.xml"))
        htmlOutput file("${project.rootDir}/build/reports/android-lint.html")
  }
}
```


### 2. Use fastlane for Android in `.gitlab-ci.yml`


* Create `CREDENTIAL_JSON_BASE64` variable and set its content with the output of `base64  -i credentials.json` command.

```yml

deploy:
  before_script:
    - apt-get -qq update
    - apt-get install -qqy --no-install-recommends build-essential ruby-full
    - gem install bundler fastlane
    - gem install json -v '2.6.2' # use if json module not found
    - bundle install
    - echo $CREDENTIAL_JSON_BASE64 | base64 -d > credentials.json
  script:
    - bundle exec fastlane deploy

  needs: [ "test","lint" ]
  only:
    - release

```
