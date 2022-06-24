def repo = params.get("REPO") 
def branch = params.get("BRANCH") 
def description = params.get("DESCRIPTION") 
def buildResult = [:]
def promoteResult = [:]

node('linux') 
{
    stage ('Poll') {
            // Node.js.
            checkout([
              $class: 'GitSCM',
              branches: [[name: 'refs/heads/main']],
              extensions: [
                [$class: 'CheckoutOption', timeout: 30],
                [$class: 'RelativeTargetDirectory', relativeTargetDir: 'utils'],
                [$class: 'CloneOption', noTags: true, reference: '', shallow: false, timeout: 30],
                [$class: 'ScmName', name: 'utils']
              ],
              userRemoteConfigs: [[url: 'https://github.com/ZOSOpenTools/utils.git']]
            ])
        }
        
        stage('Build and Test') {
                buildResult = build job: 'Port-Build', parameters: [string(name: 'PORT_GITHUB_URL', value: "${repo}"), string(name: 'BRANCH', value: "${branch}")]
        }
        
        stage('Promote') {
                build job: 'Port-Publish', parameters: [string(name: 'BUILD_SELECTOR', value: "<SpecificBuildSelector plugin=\"copyartifact@1.46.2\">  <buildNumber>${buildResult.number.toString()}</buildNumber></SpecificBuildSelector>"), string(name: 'REPO', value: "${repo}"), string(name: 'DESCRIPTION', value: "${description}")]
            
        }
}
