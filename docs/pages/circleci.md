# CircleCI

The following config declares the `release` job and uses it in the `build_and_release` workflow. The `release` job will only run on commits to master.

```yaml
version: 2

defaults: &defaults
  working_directory: ~/auto
  docker:
    - image: circleci/node:latest-browsers

jobs:
  install: # your install job

  release:
    <<: *defaults
    steps:
      - attach_workspace:
          at: ~/auto
      - run:
          name: Release
          command: npm auto shipit -w

workflows:
  version: 2
  build_and_release:
    jobs:
      - install:
          filters:
            tags:
              only: /.*/

      - release:
          requires:
            - install
          filters:
            branches:
              only:
                - master
```

## Troubleshooting

If you are having problems make sure you have done the following:

- `GH_TOKEN` is set
- `NPM_TOKEN` is set (with the NPM plugin)

### Problems pushing tags to github

Go to Settings -> Checkout SSH Keys -> `Create and add YOUR_USERNAME user key`. This will create a key with the ability to push to github.
