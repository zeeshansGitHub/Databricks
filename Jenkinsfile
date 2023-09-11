pipeline {
    agent any

    environment {
        DATABRICKS_TOKEN = credentials('databricks-token-id') // Use the ID of the Databricks credentials you created
        DATABRICKS_WORKSPACE_URL = 'https://adb-940998992005123.3.azuredatabricks.net' // Replace with your Databricks workspace URL
        NOTEBOOK_PATH = '/Users/zeeshan.abbas@hotmail.co.uk/' // Replace with the desired path in Databricks
        GITHUB_REPO_URL = 'https://github.com/zeeshansGitHub/Databricks.git' // Replace with your GitHub repository URL
        GITHUB_REPO_BRANCH = 'main' // Replace with the branch containing your notebook
        GITHUB_NOTEBOOK_PATH = 'Read-csv.ipynb' // Path to the .ipynb notebook file in your GitHub repo
        GITHUB_CREDENTIALS = credentials('aaa17aad-43a8-4f40-862f-20e2d62a45f9')
        EXISTING_CLUSTER_ID = '0907-160409-unfd8z3n'
    }

    stages {
        stage('Clone Notebook from GitHub') {
            steps {
                script {
                    // Clone the notebook from GitHub
                    checkout([$class: 'GitSCM', branches: [[name: GITHUB_REPO_BRANCH]],
                              userRemoteConfigs: [[url: GITHUB_REPO_URL, credentialsId: GITHUB_CREDENTIALS]]])
                }
            }
        }

        stage('Upload Notebook to Databricks') {
            steps {
                script {
                    // Read the notebook file content
                    def notebookContent = readFile("Test-Notebook22.ipynb")
                    def notebookPath = '/Users/zeeshan.abbas@hotmail.co.uk/Test-Notebook22.ipynb'
                    // Remove the dollar sign ('$') character
                    def cleanedContent = notebookContent.replaceAll('\\$', '')
                    def trimmedContent = cleanedContent.trim()
                    def base64Content = trimmedContent.bytes.encodeBase64().toString()
                    def existingClusterId  = "0907-160409-unfd8z3n";
                    echo "Notebook Path: $notebookPath";

                    // Define the HTTP POST request to import the notebook
                    def response = sh(script: '''
                    curl -n -X POST -H "Authorization: Bearer $DATABRICKS_TOKEN" \
                    -H "Content-Type: application/json" \
                    -d '{
                        "job_id": null,
                        "existing_cluster_id": "0907-160409-unfd8z3n",
                        "content": "'"$notebookContent"'",
                        "path": "/Users/zeeshan.abbas@hotmail.co.uk/Test-Notebook22.ipynb"
                    }' \
                    --url "$DATABRICKS_WORKSPACE_URL/api/2.0/workspace/import"
                ''', returnStdout: true)

                    // Print the response
                    echo "Databricks Import Response: $response"
                }
            }
        }
        
        // Add more stages as needed
    }
}