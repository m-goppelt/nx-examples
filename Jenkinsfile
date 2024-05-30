pipeline {
    agent none
    environment {
        NX_BRANCH = env.BRANCH_NAME.replace('PR-', '')
    }
    stages {
        stage('Pipeline') {
            parallel {
                stage('Master') {
                    when {
                        branch 'master'
                    }
                    agent any
                    steps {
                        // This line enables distribution
                        // The "--stop-agents-after" is optional, but allows idle agents to shut down once the "e2e-ci" targets have been requested
                        sh "npm i --legacy-peer-deps"
                        // sh "npx nx affected --base=HEAD~1 -t lint test build e2e-ci"
                        sh "npx nx show projects --base=HEAD~1 --affected --with-target build"
                    }
                }
                stage('PR') {
                    when {
                        not { branch 'master' }
                    }
                    agent any
                    steps {
                        // This line enables distribution
                        // The "--stop-agents-after" is optional, but allows idle agents to shut down once the "e2e-ci" targets have been requested
                        // sh "npx nx-cloud start-ci-run --distribute-on='5 linux-medium-js' --stop-agents-after='e2e-ci'"
                        sh "npm ci"
                        sh "npx nx affected --base origin/${env.CHANGE_TARGET} -t lint test build e2e-ci"
                    }
                }
            }
        }
    }
}
