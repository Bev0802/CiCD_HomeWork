## Урок 3. Continuous delivery и continuous deployment (непрерывная доставка и развертывание)

<h3><span style="color: #C8A2C8;">Домашнее задание</h3></span>

1. Добавить 2 окружения "preprod" и "production".

2. Добавить отдельные deploy job для каждой среды.

3. Добавить переменную "$MyLogin" внутри .gitlab-ci.yml, которая будет меняться в зависимости от среды.

4. Добавить переменную "$MyPassword" не используя .gitlab-ci.yml, которая так же будет меняться в зависимости от среды.

5. Добавить скрипт в .gitlab-ci.yml, который найдёт все запущенные pipeline по названии ветки(ref) и остановит их.

<h3><span style="color: #C8A2C8;">Выполнила:</h3></span>

https://gitlab.com/bev0802/cicdhomework3

[gitlab-ci.yml](gitlab-ci.yml)

```.gitlab-ci.yml
stages:
  - build
  - test
  - deploy_preprod
  - deploy_prod

variables:
  PROD_LOGIN: "production_login"
  PREPROD_LOGIN: "preprod_login"

build-job:
  stage: build
  script:
    - echo "Compiling the code..."
    - echo "Compile complete."

unit-test-job:
  stage: test
  script:
    - echo "Running unit tests... This will take about 60 seconds."
    - echo "Code coverage is 90%"

lint-test-job:
  stage: test
  script:
    - echo "Linting code... This will take about 10 seconds."
    - echo "No lint issues found."

deploy-to-preprod:
  stage: deploy_preprod
  environment: preprod
  variables:
    MyLogin: "preprod_login"
  script:
    - echo "Deploying to pre-production environment..."
    - echo "Login with $MyLogin"

deploy-to-production:
  stage: deploy_prod
  environment: production
  variables:
    MyLogin: "production_login"
  script:
    - echo "Deploying to production environment..."
    - echo "Login with $MyLogin"

pages:
  stage: deploy_prod
  script:
    - echo 'dream house'
  artifacts:
    name: "$CI_JOB_NAME"
    paths:
      - public
  only:
    - main

stop_previous_pipelines:
  stage: build
  script:
    - >
      curl --request POST --header "PRIVATE-TOKEN: $CI_JOB_TOKEN"
      "https://gitlab.com/api/v4/projects/$CI_PROJECT_ID/pipelines/$CI_PIPELINE_ID/cancel"
  only:
    - main
```

![Pipelines](Pipelines1.JPG)
