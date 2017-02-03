node("cd") {
    def serviceName = "taskapi"
    def prodIp = "13.91.98.190"
    def proxyIp = "13.91.98.190"
    def proxyNode = "davoodacsmgmt.westus.cloudapp.azure.com"
    def registryIpPort = "10.100.198.200:5000"

   git url: "https://github.com/DavoodKhan/task-api.git"
    
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
