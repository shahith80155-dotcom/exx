// ============================================================
//  Jenkinsfile — ExpenseTrack CI/CD Pipeline
//  Trigger: GitHub Webhook on every push to main branch
// ============================================================

pipeline {

    // Run on any available Jenkins agent
    agent any

    // --------------------------------------------------------
    // ENVIRONMENT VARIABLES
    // Change DEPLOY_DIR to your web server path if needed
    // --------------------------------------------------------
    environment {
        APP_NAME    = 'expense-tracker'
        DEPLOY_DIR  = '/var/www/html/expense-tracker'
        BUILD_DATE  = sh(script: 'date +%Y-%m-%d', returnStdout: true).trim()
    }

    // --------------------------------------------------------
    // TRIGGERS — Auto-build on GitHub push
    // Set up GitHub Webhook to call:
    //   http://<jenkins-server>/github-webhook/
    // --------------------------------------------------------
    triggers {
        githubPush()
    }

    // --------------------------------------------------------
    // PIPELINE STAGES
    // --------------------------------------------------------
    stages {

        // STAGE 1: Checkout source code from GitHub
        stage('Checkout') {
            steps {
                echo '=== Stage 1: Checking out source code from GitHub ==='
                checkout scm
                echo "Branch: ${env.BRANCH_NAME}"
                echo "Commit: ${env.GIT_COMMIT}"
            }
        }

        // STAGE 2: Validate — make sure all required files exist
        stage('Validate') {
            steps {
                echo '=== Stage 2: Validating project files ==='
                sh '''
                    echo "Checking required HTML files..."
                    test -f index.html        && echo "✅ index.html found"        || (echo "❌ index.html MISSING" && exit 1)
                    test -f add-expense.html  && echo "✅ add-expense.html found"  || (echo "❌ add-expense.html MISSING" && exit 1)
                    test -f expenses.html     && echo "✅ expenses.html found"     || (echo "❌ expenses.html MISSING" && exit 1)
                    test -f budget.html       && echo "✅ budget.html found"       || (echo "❌ budget.html MISSING" && exit 1)
                    test -f about.html        && echo "✅ about.html found"        || (echo "❌ about.html MISSING" && exit 1)
                    test -f style.css         && echo "✅ style.css found"         || (echo "❌ style.css MISSING" && exit 1)
                    echo ""
                    echo "All required files are present!"
                '''
            }
        }

        // STAGE 3: Build — package files into a zip artifact
        stage('Build') {
            steps {
                echo '=== Stage 3: Packaging files as build artifact ==='
                sh '''
                    mkdir -p build
                    cp *.html build/
                    cp *.css  build/
                    cp README.md build/ 2>/dev/null || true

                    cd build
                    zip -r ../expense-tracker-${BUILD_DATE}.zip .
                    cd ..

                    echo "Build artifact created: expense-tracker-${BUILD_DATE}.zip"
                    ls -lh expense-tracker-*.zip
                '''
            }
        }

        // STAGE 4: Archive — save the artifact in Jenkins
        stage('Archive Artifact') {
            steps {
                echo '=== Stage 4: Archiving build artifact in Jenkins ==='
                archiveArtifacts artifacts: '*.zip', fingerprint: true
                echo "Artifact archived successfully!"
            }
        }

        // STAGE 5: Deploy — copy files to web server directory
        stage('Deploy') {
            steps {
                echo '=== Stage 5: Deploying to web server ==='
                sh '''
                    # Create deploy directory if it doesn't exist
                    mkdir -p ${DEPLOY_DIR}

                    # Copy all HTML and CSS files
                    cp *.html ${DEPLOY_DIR}/
                    cp *.css  ${DEPLOY_DIR}/

                    echo "Files deployed to: ${DEPLOY_DIR}"
                    ls -la ${DEPLOY_DIR}/
                    echo ""
                    echo "✅ Deployment complete!"
                    echo "🌐 App is live at: http://localhost/expense-tracker/"
                '''
            }
        }
    }

    // --------------------------------------------------------
    // POST-BUILD ACTIONS
    // --------------------------------------------------------
    post {
        success {
            echo """
            ============================================
            ✅ BUILD SUCCESSFUL
            App    : ${APP_NAME}
            Branch : ${env.BRANCH_NAME}
            Commit : ${env.GIT_COMMIT}
            Date   : ${BUILD_DATE}
            ============================================
            """
        }
        failure {
            echo """
            ============================================
            ❌ BUILD FAILED
            App    : ${APP_NAME}
            Branch : ${env.BRANCH_NAME}
            Commit : ${env.GIT_COMMIT}
            Please check the logs above for errors.
            ============================================
            """
        }
        always {
            // Clean up the build folder after every run
            sh 'rm -rf build/'
            echo "Workspace cleaned up."
        }
    }
}
