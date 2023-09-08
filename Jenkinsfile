// Filename: Jenkinsfile
node {
  def GITREPO         = "/Users/zeesh/Documents/GitHub/Jenkins/Databricks"
  def GITREPOREMOTE   = "https://github.com/zeeshansGitHub/Databricks"
  def GITHUBCREDID    = "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"
  def CURRENTRELEASE  = "main"
  def SCRIPTPATH      = "${GITREPO}/Automation/Deployments"
  def NOTEBOOKPATH    = "${GITREPO}/Workspace"
  def LIBRARYPATH     = "${GITREPO}/Libraries"
  def BUILDPATH       = "${GITREPO}/Builds/${env.JOB_NAME}-${env.BUILD_NUMBER}"
  def OUTFILEPATH     = "${BUILDPATH}/Validation/Output"
  def TESTRESULTPATH  = "${BUILDPATH}/Validation/reports/junit/test-reports"
  def WORKSPACEPATH   = "/Workspace/Users/zeeshan.abbas@hotmail.co.uk/Test-Notebook1"
  def DBFSPATH        = "dbfs:/Test-Notebook1"
  def VENVPATH        = "${GITREPO}/.venv"
  def DBCLIPATH       = "/usr/local/bin"

  stage('Checkout') {
    withCredentials ([string(credentialsId: GITHUBCREDID, variable: 'GITHUB_TOKEN')]) {
      echo "Pulling branch '${CURRENTRELEASE}' from GitHub..."
      git branch: CURRENTRELEASE, credentialsId: GITHUB_TOKEN, url: GITREPOREMOTE
    }
  }
//   stage('Run Unit Tests') {
//     try {
//       sh """#!/bin/bash
//             # Enable venv environment for tests
//             source ${VENVPATH}/bin/activate
//             # Python tests for libs
//             python3 -m pytest --junit-xml=${TESTRESULTPATH}/TEST-libout.xml ${LIBRARYPATH}/python/dbxdemo/build/lib/dbxdemo/test_*.py || true
//            """
//     } catch(err) {
//       step([$class: 'JUnitResultArchiver', testResults: '--junit-xml=${TESTRESULTPATH}/TEST-*.xml'])
//       if (currentBuild.result == 'UNSTABLE')
//         currentBuild.result = 'FAILURE'
//       throw err
//     }
//   }
//   stage('Package') {
//     sh """#!/bin/bash
//           # Enable venv environment for packaging
//           # source ${VENVPATH}/bin/activate

//           # Package Python wheel
//           cd ${LIBRARYPATH}/python/dbxdemo
//           python3 setup.py sdist bdist_wheel
//        """
//   }
//   stage('Build Artifact') {
//     sh """mkdir -p ${BUILDPATH}/Workspace
//           mkdir -p ${BUILDPATH}/Libraries/python
//           mkdir -p ${BUILDPATH}/Validation/Output
//           #Get modified files
//           git diff --name-only --diff-filter=AMR HEAD^1 HEAD | xargs -I '{}' cp -r '{}' ${BUILDPATH}

//           # Get packaged libs
//           find ${LIBRARYPATH} -name '*.whl' | xargs -I '{}' cp '{}' ${BUILDPATH}/Libraries/python/
//        """
//   }
//   stage('Deploy') {
//     sh """#!/bin/bash
//           # Copy notebooks into build
//           cp -R ${NOTEBOOKPATH} ${BUILDPATH}

//           # Use Databricks CLI to deploy notebooks
//           ${DBCLIPATH}/databricks workspace import-dir ${BUILDPATH}/Workspace ${WORKSPACEPATH} --overwrite

//           # Use Databricks CLI to deploy Python wheel
//           ${DBCLIPATH}/databricks fs cp -r ${BUILDPATH}/Libraries/python ${DBFSPATH} --overwrite
//        """
//     sh """#!/bin/bash
//           # Enable venv environment for commands
//           source ${VENVPATH}/bin/activate

//           # Get space-delimited list of libraries
//           LIBS=\$(find ${BUILDPATH}/Libraries/python/ -name '*.whl' | sed 's#.*/##')

//           echo \$LIBS

//           # Script to uninstall, reboot if needed and install library
//           python3 ${SCRIPTPATH}/installWhlLibrary.py \
//                     --libs=\$LIBS \
//                     --dbfspath=${DBFSPATH}
//        """
//   }
//   stage('Run Integration Tests') {
//     sh """#!/bin/bash
//           # Enable venv environment for script
//           source ${VENVPATH}/bin/activate

//           python3 ${SCRIPTPATH}/executenotebook.py \
//                   --localpath=${NOTEBOOKPATH} \
//                   --workspacepath=${WORKSPACEPATH} \
//                   --outfilepath=${OUTFILEPATH}
//        """
//     sh """#!/bin/bash
//           sed -i -e 's #ENV# ${OUTFILEPATH} g' ${SCRIPTPATH}/evaluatenotebookruns.py
//           python3 -m pytest --junit-xml=${TESTRESULTPATH}/TEST-notebookout.xml ${SCRIPTPATH}/evaluatenotebookruns.py || true
//        """
//   }
//   stage('Report Test Results') {
//     sh """find ${OUTFILEPATH} -name '*.json' -exec gzip --verbose {} \\;
//           touch ${TESTRESULTPATH}/TEST-*.xml
//        """
//     junit allowEmptyResults: true, testResults: '**/test-results/*.xml'
//   }
}