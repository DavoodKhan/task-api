node("cd") {
    def serviceName = "task-api"
    def prodIp = "10.100.198.201"
    def proxyIp = "10.100.198.201"
    def proxyNode = "prod"
    def registryIpPort = "10.100.198.200:5000"
		
    def flow = load "/data/scripts/workflow-util.groovy"

	//println "pwd".execute().text()
		
    git url: "https://github.com/DavoodKhan/task-api.git"
    
	//"pwd".execute()
	
	//"chmod +x ./*.sh".execute()
	
	//"./movebuildfiles.sh".execute()
	
	//"./preload_ch.sh".execute()

	flow.setupCommands(serviceName)
	flow.provision("prod2.yml")
    flow.buildTests(serviceName, registryIpPort)
    flow.runTests(serviceName, "tests", "")
    flow.buildService(serviceName, registryIpPort)

    def currentColor = flow.getCurrentColor(serviceName, prodIp)
    def nextColor = flow.getNextColor(currentColor)

    flow.deployBG(serviceName, prodIp, nextColor)
    flow.runBGPreIntegrationTests(serviceName, prodIp, nextColor)
    flow.updateBGProxy(serviceName, proxyNode, nextColor)
    flow.runBGPostIntegrationTests(serviceName, prodIp, proxyIp, proxyNode, currentColor, nextColor)
}
