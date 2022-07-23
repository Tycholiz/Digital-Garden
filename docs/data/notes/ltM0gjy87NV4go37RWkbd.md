
[[Jobs|gitlab.CI-CD.jobs]] can be configured to cache directories/files, which can be used by subsequent jobs and even subsequent pipelines.
- Subsequent jobs in the same pipeline can use the cache, if the dependencies are identical.

Use cache for dependencies, like packages you download from the internet. Cache is stored where GitLab Runner is installed 
- it is also uploaded to [[S3|aws.svc.S3]] if distributed cache is enabled.

Caching happens after `after_script` has run.

In the following pipeline, these things happen in order:
- the build-cache is checked for the `vendor` directory (perhaps jobA cached it)
  - If nothing is found, we move onto the next step and output "Hello world" to the console. Once all steps are done (ex. all before_script, script and after_script steps), the `vendor/` directory is zipped into cache.zip
  - If the `build-cache` directory is found in the cache.zip, then the cache is extracted, and we move on to the subsequent steps.

This above flow shows how the cache is extracted at the start if it contains the key, and it is filled at the end if the data hasn't been cached yet.

```yml
jobB:
  cache:
    key: build-cache
    paths:
      - vendor/
  after_script: 
    - "echo Hello world"
```

Jobs that have caches with the same key will share the cache.

Tag your runners and use the tag on jobs that share the cache.

All caches defined for a job are archived in a single cache.zip
The runner configuration defines where the file is stored.
- By default, the cache is stored on the machine where GitLab Runner is installed.
    - [exact paths](https://docs.gitlab.com/ee/ci/caching/#where-the-caches-are-stored)

For runners to work with caches efficiently, you must do one of the following:
- Use a single runner for all your jobs.
- Use multiple runners that have distributed caching, where the cache is stored in S3 buckets. Shared runners on GitLab.com behave this way. These runners can be in autoscale mode, but they don’t have to be.
- Use multiple runners with the same architecture and have these runners share a common network-mounted directory to store the cache. This directory should use NFS or something similar. These runners must be in autoscale mode.

You can have a maximum of four caches:
```yml
test-job:
  stage: build
  cache:
    - key:
        files:
          - Gemfile.lock
      paths:
        - vendor/ruby
    - key:
        files:
          - yarn.lock
      paths:
        - .yarn-cache/
  script:
    - bundle install --path=vendor
    - yarn install --cache-folder .yarn-cache
    - echo Run tests...
```

To have jobs in each branch use the same cache, define a cache with the key: $CI_COMMIT_REF_SLUG:
- note: this pattern can be followed to define any business-logic surrounding which objects should share a cache.
    - per-job and per-branch caching: `"$CI_JOB_NAME-$CI_COMMIT_REF_SLUG"`
    - per-stage and per-branch caching: `"$CI_JOB_STAGE-$CI_COMMIT_REF_SLUG"`

```yml
cache:
  key: $CI_COMMIT_REF_SLUG
```

# Resources
[Special consideration to caching Node.js dependencies](https://docs.gitlab.com/ee/ci/caching/#cache-nodejs-dependencies)
