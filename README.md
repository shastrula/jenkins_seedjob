# jenkins_seedjob

## Jenkins Seed Job

This Jenkins seed job is configured to scan a GitHub repository for Jenkins jobs.

### Prerequisites

- Jenkins installed and running
- GitHub repository with Jenkins job definitions
- Jenkins GitHub plugin installed

### Seed Job Configuration

1. Open Jenkins and navigate to `New Item`.
2. Enter a name for the seed job and select `Pipeline` as the job type.
3. In the Pipeline section, select `Pipeline script from SCM`.
4. Choose `Git` as the SCM and enter the repository URL.
5. Set the branch to scan (e.g., `*/main`).
6. In the `Script Path`, enter the path to the Jenkinsfile (e.g., `Jenkinsfile`).

### Example Jenkinsfile

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/your-repo/your-project.git'
            }
        }
        stage('Seed Job') {
            steps {
                jobDsl scriptText: '''
                    pipelineJob('example-job') {
                        definition {
                            cpsScm {
                                scm {
                                    git {
                                        remote {
                                            url('https://github.com/your-repo/your-project.git')
                                        }
                                        branches('*/main')
                                    }
                                }
                                scriptPath('Jenkinsfile')
                            }
                        }
                    }
                '''
            }
        }
    }
}
```

### Running the Seed Job

1. Save the seed job configuration.
2. Build the seed job to scan the GitHub repository and create Jenkins jobs.

This setup will automatically scan the specified GitHub repository and create Jenkins jobs based on the definitions found in the repository.