# macOS M1 Preview Documentation

The purpose of this repository is to walk you through the details of the macOS M1 preview on CircleCI.

## What are macOS M1 resources on CircleCI?

The M1 resource class is a new macOS option, running on Apple Silicon hardware, for those developing, building, testing, and signing iOS, iPadOS, macOS, WatchOS, and tvOS applications using the Xcode IDE.

### Supported Preview Xcode Images
* [Xcode 13.4.1](https://gist.github.com/BytesGuy/febf02b354dce391d7a14cb994b09d99#file-xcode13-txt) **(PREVIEW IMAGE - WILL BE REMOVED ON MARCH 9)**
* [Xcode 14.0.1](https://gist.github.com/BytesGuy/febf02b354dce391d7a14cb994b09d99#file-xcode14-txt) **(PREVIEW IMAGE - WILL BE REMOVED ON MARCH 9)**
* [Xcode 14.2.0](https://circle-macos-docs.s3.amazonaws.com/image-manifest/v11441/manifest.txt) **(PRODUCTION IMAGE)**

The images provided during the preview phase are pre-production and are not indicative of the final images that will ship at product launch. Software setup, software versions, and general configurations may differ compared to our Intel based images. Rosetta will not be pre-installed on the final production images, though we will make the best effort to align the M1 images with the current Intel images.

We will launch with an Xcode `14.2.0` production image and will release the remaining images post-launch. Xcode `13.4.1` is the oldest version we will support on Apple Silicon resources. 

### Known Limitations & Important Information
* Jobs requiring an active desktop session
   * If you are unable to start an active desktop session, we suggest starting a VNC session as a temporary workaround. Instructions for doing this can be found in our [support docs](https://support.circleci.com/hc/en-us/articles/360020345334-How-to-connect-to-a-macOS-container-via-VNC).
* In response to a [bug](https://github.com/aws/aws-cli/issues/7481) with S3 that impacted network performance, we have implemented a temporary workaround to use accelerated data transfer. Once the S3 fix has been deployed, we will go bad to using normal S3 data transfer. More details can be found in the [closed issue](https://github.com/CircleCI-Public/macOS-M1-Preview-Documentation/issues/1).

## Pricing and Specs
The following macOS M1 resource class is available:

|Resource Class Name|vCPU|Memory|Price
|---|---|---|---|
|`macos.m1.large.gen1`|8vCPU|12 GB|400 credits/minute

## Example of a CircleCI config using macOS M1 resources
```yaml
# .circleci/config.yml
version: 2.1
jobs: # a basic unit of work in a run
  build-and-test: # your job name
    macos:
      xcode: 13.4.1 # indicate your selected version of Xcode
    resource_class: macos.m1.large.gen1
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
* Will you be offering a medium Apple Silicon VM option?
  * We are planning to offer a medium option in the future, though no further details or timelines are available at this time. 
