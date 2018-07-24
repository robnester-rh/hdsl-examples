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
      stage("Checkout Repo"){
          deleteDir()
          git branch: 'all_in_one', url: 'https://github.com/robnester-rh/hdsl-examples'
      }

      stage("Parse Configuration"){
          parseConfig()
      }

      stage("Deploy Infra"){
          deployInfra()
      }

      stage("Configure Infra"){
          configureInfra()
      }

      stage("Execute Tests"){
          executeTests()
      }

      stage("Destroy Infra"){
          destroyInfra()
      }

      stage("Archive stuff"){
          saveArtifacts filesToExclude: 'README.md',
          filesToSave:'contra.yml'
      }
  }
}
