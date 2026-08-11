pipeline {
  agent any

  environment {
    GITHUB_REPOSITORY = 'maufq/blog'
    PUBLISH_BRANCH = 'gh-pages'
  }

  stages {
    stage('Validate') {
      steps {
        sh '''
          test -f index.html
          test -f assets/css/styles.css
          test -f posts/bienvenido.html
          grep -q '<!doctype html>' index.html
        '''
      }
    }

    stage('Publish to GitHub Pages') {
      when {
        branch 'main'
      }
      steps {
        withCredentials([string(credentialsId: 'github-token', variable: 'GITHUB_TOKEN')]) {
          sh '''
            set -eu
            git config user.name "Jenkins"
            git config user.email "jenkins@users.noreply.github.com"
            if git show-ref --verify --quiet "refs/heads/${PUBLISH_BRANCH}"; then
              git branch -D "$PUBLISH_BRANCH"
            fi
            git checkout --orphan "$PUBLISH_BRANCH"
            git rm -rf .
            git checkout main -- index.html assets posts
            touch .nojekyll
            git add index.html assets posts .nojekyll
            git commit -m "Deploy GitHub Pages (${BUILD_NUMBER})"
            git remote set-url origin "https://x-access-token:${GITHUB_TOKEN}@github.com/${GITHUB_REPOSITORY}.git"
            git push --force origin "$PUBLISH_BRANCH"
          '''
        }
      }
    }
  }
}
