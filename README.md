# CircleCI Missing Images
This repository contains Dockerfiles for stacks and services, which are not provided by CircleCI.

All images have same base as other CircleCI images, since all these files was generated using CircleCI templates. CCI dockerfile generator script does not support non-standart images(ones like myuser/myimage), python image was used as a foundation. Then, generated images was updated manually.

Each base image can contain additional logic within "BEGIN IMAGE CUSTOMIZATIONS" block. For example, .NET Core image has trx2junit unitlity set in the image, so you can easily generate junit2 logs to feed it into "store_test_results" CCI directive

## .NET Core - dotnetcore
- Only latest image is supported (currently 2.2)
- trx2junit preinstalled.

Usage example:

```
    - run:
        name: "Run tests: Prepare"
        command: dotnet restore && dotnet build
    - run:
        name: "Run tests"
        command: dotnet test --no-build --logger "trx;LogFileName=trx/Result.trx"

    - run:
        name: "Convert the logs"
        command: trx2junit TestResults/trx/Result.trx
        when: always
    - store_test_results:
        path: repo-vaynoe-qa/TestResults
```
