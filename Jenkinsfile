node {

    def scmvars = checkout(scm)
    def commitHash = scmvars.GIT_COMMIT

    try {
        stage('Determine Jenkinsfile to build') {
            
            // def sout = sh(returnStdout: true, script: 'git diff --name-only origin/main...HEAD')
            def sout = getChangedFilesList()
            def j = findJenkinsfileToRun(sout)

            if (j.toString() == "${pwd()}/Jenkinsfile") {
                println("Building the whole world... (But here I not put nothing =] )")
            } else {
                println("Building ${j.toString()}")
                load "${j.toString()}"
            }
        
        }
        currentBuild.result = 'SUCCESS'
    } catch (err) {
        println("ERR: ${err}")
        currentBuild.result = 'FAILED'
    }
}

@NonCPS
def String getChangedFilesList() {
    changedFiles = []
    for (changeLogSet in currentBuild.changeSets) { 
        for (entry in changeLogSet.getItems()) { // for each commit in the detected changes
            for (file in entry.getAffectedFiles()) {
                println("Arquivo: ${file.getPath()}")
                changedFiles.add("${file.getPath()}") // add changed file to list
            }
        }
    }
    return changedFiles
}

@NonCPS
def createFilePath(def path) {
    if (env['NODE_NAME'].equals("master")) {
        File localPath = new File(path)
        return new hudson.FilePath(localPath);
    } else {
        return new hudson.FilePath(Jenkins.getInstance().getComputer(env['NODE_NAME']).getChannel(), path);
    }
}

@NonCPS
def findMostSpecificJenkinsFile(filePath) {
    if (filePath.name == "Jenkinsfile" && ! filePath.isDirectory()) {
        return filePath;
    }

    def potentialJenkinsFile=createFilePath("${filePath.getParent().toString()}/Jenkinsfile")

    if (potentialJenkinsFile.exists()) {
        return potentialJenkinsFile
    }

    return findMostSpecificJenkinsFile(filePath.getParent())
}

@NonCPS
def findJenkinsfileToRun(paths) {

    def foundJenkinsFiles = []

    println("Finding most specific Jenkinsfile for changed files:\n${paths}")

    for (path in paths) {
        def f = createFilePath("${pwd()}/${path}")
        if (f != null) {
            def j = findMostSpecificJenkinsFile(f)
            if (j != null || j != "") {
                foundJenkinsFiles = foundJenkinsFiles << j
            }
        }
    }

    println("Selecting from:\n${foundJenkinsFiles}")

    foundJenkinsFiles.unique()

    if (foundJenkinsFiles.size() == 1) {
        return foundJenkinsFiles.get(0)
    } else {
        return "${pwd()}/Jenkinsfile"
    }
}