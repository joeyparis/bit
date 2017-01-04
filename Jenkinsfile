node  {
	def releaseServer = "${env.BIT_STAGE_SERVER}"
	print releaseServer
	def env = "${environment}"
	def app = "bit"
	def currentVersion = sh script: 'cat package.json | grep version | head -1 | awk -F: \'{ print $2 }\' | sed \'s/[",]//g\' ' , returnStdout: true
	currentVersion = currentVersion.replaceAll("\\s","")
	def bundleName = "bit_${currentVersion}"
    def uploadfolder = "gs://bit-assets/release/${currentVersion}/"
    
    stage 'remove old zip files '
    sh("rm -rf *.tar.gz  && rm -rf ./distribution")

    stage 'Running tar'
    sh('cd ./scripts && ./build-tar.sh tar')

    stage 'Running brew'
    sh('cd ./scripts && ./build-brew.sh')

    stage 'Running deb'
    sh('cd ./scripts && ./build-deb.sh')


    stage 'export to google storage'
    sh("gsutil -m cp -a public-read ./distribution/brew_pkg/${bundleName}_brew.tar.gz ${uploadfolder}")
    sh("gsutil -m cp -a public-read ./distribution/debian_pkg/${bundleName}_deb.deb ${uploadfolder}")

}