trigger:
  branches:
    include:
      - main  # Trigger the pipeline when changes are pushed to the 'main' branch

pool:
  vmImage: 'ubuntu-latest'  # Use the latest Ubuntu-based VM image for the build environment

steps:
- task: Maven@3  # Maven task to run build and tests
  inputs:
    mavenPomFile: 'pom.xml'  # Path to the Maven POM file (the build configuration file)
    goals: 'clean test'  # Clean the project and run all the tests (including regression tests)
    options: '-DskipTests=false'  # Ensure tests are not skipped

- task: PublishTestResults@2  # Publish the test results to Azure DevOps
  inputs:
    testResultsFiles: '**/target/test-*.xml'  # Location of the Maven test result XML files
    testRunTitle: 'Regression Tests'  # A name for the test run in Azure DevOps
