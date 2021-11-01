


名称：pod名称，在k8s中实际名称为jenkins-slave-随机值。
标签列表：此处标签即标识Jenkins agent的，如流水线中agent定义调度在哪个slave上运行。当然此处我们也可不配置，kubernetes plugin将会默认使用jenkins/jnlp-slave:alpine镜像创建。但是kubernetes-plugin官方已停止维护此镜像，而统一使用jenkins/inbound-agent。因此我们需要进行重新设置。 名称：pod中容器的名称，注意此处必须设置为jnlp，才能对镜像重写使用jenkins/inbound-agent，否则将会出现以下问题：k8s同时拉取jenkins/inbound-agent和jenkins/jnlp-slave:alpine两个镜像，第一个为重写后的实际使用镜像，第二个为默认镜像，导致jenkins-slave无法正常运行，不断重复构建。
Docker镜像：当名称设置为jnlp后，jenkins/inbound-agent即为重写后的镜像，否则默认使用jenkins/jnlp-slave:alpine。
工作目录：Jenkins slave的默认工作目录，构建时将会在此目录下创建workspace。
运行的命令和命令参数： 其中运行的命令必须要留空，否则会重写镜像的默认entrypoint，导致agent 无法连接到master，下面我们会进行演示说明。
资源限制：默认的容器是没有资源限制的，我们在此添加了cpu和memory限制，大家可根据实际情况进行修改。
