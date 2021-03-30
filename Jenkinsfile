def JOBS = ['blackbox_self_service.groovy']
def DEVOPS_GATE_URL = "https://gitlab.delphix.com/devops/qa-infra.git"

node {
  // Checkout the devops-gate to find the defined pipelines
  stage('Check out devops-gate') {
    checkout(
      changelog: false,
      poll: false,
      scm: [
        $class: 'GitSCM',
        userRemoteConfigs: [[name: 'origin', url: DEVOPS_GATE_URL, credentialsId: 'git-ci-key']],
        branches: [[name: 'master']],
        extensions: [
          [$class: 'CloneOption', shallow: false, timeout: 60],
          [$class: 'RelativeTargetDirectory', relativeTargetDir: 'devops-gate'],
          [$class: 'WipeWorkspace']
        ]
      ]
    )
  }
  for (job in JOBS) {
    def jobPath = "devops-gate/jenkins/jobs/pipelines/${job}"
    println("Executing ${jobPath}")
    load(jobPath)
  }
}
