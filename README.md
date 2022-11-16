# macOS M1 Preview Documentation

The purpose of this repository is to walk you through the details of the macOS M1 preview on CircleCI.

## What are macOS M1 resources on CircleCI?

The M1 resource class is a new macOS option, running on Apple Silicon hardware, for those developing, building, testing, and signing iOS, iPadOS, macOS, WatchOS, and tvOS applications using the Xcode IDE.

### Supported Xcode Images
* [Xcode 14.0.1](https://circleci.com/docs/testing-ios#supported-xcode-versions)
* [Xcode 13.4.1](https://circleci.com/docs/testing-ios#supported-xcode-versions)

The images provided during the preview phase are pre-production and are not indicative of the final images that will ship at product launch. Software setup, software versions, and general configurations may differ compared to our Intel based images and may be updated at any time during the preview phase. We will make the best effort to align the M1 images with the current Intel images.
### Known Limitations
* ***We ask that customers limit their parallelism to 15x or less during the preview.***
   * Given the smaller capacity in the preview, some queing is expected. Reducing parallelism will help reduce the impact of queuing. 
* A security review of the new platform is underway.
* Not compatible with macOS orb.
## Pricing and Specs
The following macOS M1 resource class is available:

|Resource Class Name|vCPU|Memory
|---|---|---|
|`macos.m1.large.gen1`|8vCPU|12 GB

## Example of a CircleCI config using macOS M1 resources
```yaml
# .circleci/config.yml
version: 2.1
jobs: # a basic unit of work in a run
  build-and-test: # your job name
    macos:
      xcode: 13.4.1 # indicate your selected version of Xcode
    resource_class: macos.m1.medium.gen1
    steps: # a series of commands to run
      - checkout  # pull down code from your VCS
      - run: bundle install
      - run:
          name: Fastlane
          command: bundle exec fastlane $FASTLANE_LANE
      - store_artifacts:
          path: output
      - store_test_results:
          path: output/scan
          
workflows:
  build-test:
    jobs:
      - build-and-test
```
## How to provide feedback & frequently asked questions
Please [open an issue](https://github.com/CircleCI-Public/macos-dedicated-host-preview-docs/issues) with any feedback or to report any issues that you encounter.
### FAQ
* Can I run end-to-end tests on M1 resources?
  * We believe our M1 resource class will provide access to the GPU, allowing for full E2E testing that was previously [unavailable](https://support.circleci.com/hc/en-us/articles/360052160592-Tests-Fail-With-Error-There-is-no-available-Metal-device-on-this-system-) on macOS VMs. We'd love to hear feedback from customers on this experience and whether you run into any friction.
