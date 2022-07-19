pipeline { 
 description()
keepDependencies(false)
parameters {
  choiceParam("jobInputType", ["buildAndDeploy", "deploy"], "Some  choice  parameter")
  stringParam("BRANCH_NAME", "", "Enter your branch name, you want to build from")
}
definition {
 cpsScm {
    scm {
      git {
        remote {
            url("https://github.com/starx46/kubernatesprojects.git")
          
        }
        branch("*/main")
      }
    }
    scriptPath("kubernatesprojects/Jenkinsfile")
  }
}
disabled(false)
}
