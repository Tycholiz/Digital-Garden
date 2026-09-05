
In the .gitlab-ci.yml file, you can define:
- The scripts you want to run.
- Other configuration files and templates you want to include.
- Dependencies and caches.
- The commands you want to run in sequence and those you want to run in parallel.
- The location to deploy your application to.
- Whether you want to run the scripts automatically or trigger any of them manually.

## Keywords
### Extends
`extends` can be used to reuse configuration
extends supports multi-level inheritance, but we should try and limit it to around 3 levels.

If you want to exclude a key from being inherited, give it value of `null`
```yml
test4:
  extends: .base
  variables: null
```

You can use `extends` to merge hashes but not arrays.
- The algorithm used for merge is “closest scope wins,” so keys from the last member always override anything defined on other levels. For example, here `.in-docker` overwrites values from `only-important`, if there is overlap.

### !references
Only one level of nesting is supported for `!reference` tags. That is, we cannot `!reference` if the thing we are referencing has already been referenced.
```yml
.vars:
  variables:
    URL: "http://my-url.internal"
    IMPORTANT_VAR: "the details"

test-vars-1:
  # the following expands both URL and IMPORTANT_VAR
  variables: !reference [.vars, variables]
```

* * *

The following runs in order:
- Pipeline starts.
- job A runs.
- before_script is executed.
- script is executed.
- after_script is executed.
```yml
before_script:
  - echo "Hello"

job A:
  stage: build
  script:
    - mkdir vendor/
    - echo "build" > vendor/hello.txt
  cache:
    key: build-cache
    paths:
      - vendor/
  after_script:
    - echo "World"
```

# Resources
[.gitlab-ci.yml Keyword Reference](https://docs.gitlab.com/ee/ci/yaml/)
[.gitlab-ci.yml Lint Tool](https://docs.gitlab.com/ee/ci/lint.html)
