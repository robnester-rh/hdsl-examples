// Define the openshift pod name, docker repo url, namespace, and service acct
// for the DSL pod template.
dslPodName = "contraDsl-${UUID.randomUUID()}"
dockerRepoURL = '172.30.1.1:5000'
openshiftNamespace = 'contra-sample-project'
openshiftServiceAccount = 'jenkins'

// Create the DSL podTemplate
createDslContainers podName: dslPodName,
                    dockerRepoURL: dockerRepoURL,
                    openshiftNamespace: openshiftNamespace,
                    openshiftServiceAccount: openshiftServiceAccount,
// Pass the remainder of your jenkinsfile as a closure to the createDslContainers method
{
  node(dslPodName){
      stage("pre-flight"){
          deleteDir()
          git branch: 'config_file', url: 'https://github.com/robnester-rh/hdsl-examples.git'
      }

      stage("Parse Configuration"){
          parseConfig()
      }

      stage("Deploy Infra"){
          deployInfra()
      }

      stage("Configure Infra"){
          configureInfra baseDir: 'configure'
      }

      stage("Execute Tests"){
          executeTests baseDir: 'tests'
      }

      stage("Destroy Infra"){
          destroyInfra()
      }

      stage("Archive stuff"){
          saveArtifacts()
      }
  }
}
