
注意事项：

1、tail -f setup.log 查看日志

2、要是虚拟机cpu必须最少是2个哦！切记

3、虚拟机版本：15以上，
虚拟机下载地址，破解有秘钥
链接：https://pan.baidu.com/s/1l42H2Bf5UaeJafGRA6f_8A 
提取码：g7ia




# 废话不多说开始部署
# 部署k8s集群具体实现步骤：

1、git clone https://github.com/orange-q/k8s1.15.1.git

2、cd k8s1.15.1 && chmod -R 755 .

3、编辑base.config里面的参数

	# 1 更换 yum源为ali源，2 不更换yum源
	aliyun="1"
	masterip="172.31.100.20"
	# 过滤IP段
	ip_segment="172.31.100"
	# ntp 用
	cluster_network="172.31.100.0/16"
	k8s_version="v1.15.1"
	root_passwd="123456"
	hostname="k8s"
	hostip=(
	172.31.100.20
	172.31.100.21
	)
	#不用填写，不要动！
	tocken=
	sha_value=

4、  编辑k8s1.15.1.sh 添加两个数据，  
cat >> /etc/sysctl.conf << EOF
    	  kernel.pid_max=4194303
	  net.bridge.bridge-nf-call-ip6tables = 1  #添加内容
	  net.bridge.bridge-nf-call-iptables = 1  #添加内容
5、使用谷歌或火狐浏览器访问 https://raw.githubusercontent.com/coreos/flannel/bc79dd1505b0c8681ece4de4c0d86c5cd2643275/Documentation/kube-flannel.yml
 全选保存文件名为：kube-flannel.yml   放到/root/k8s1.15.1 目录下，主要wget下载不下来，所以需要手动下载
6、sh k8s1.15.1.sh


# 部署完后进入到dashboard文件夹部署dashboard

1、cd dashboard

2、kubectl create -f .

3、然后查看部署情况以及登录的node节点端口
kubectl get service --all-namespaces | grep kubernetes-dashboard

4、例如结果：
kube-system   kubernetes-dashboard   NodePort    10.111.252.237   <none>        443:32164/TCP            25m
那么你就输入https://nodeIP:31660来登录  ,例如：https://172.31.100.20:32164/#!/login
	
5、查看登录时候的token

kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin-user | awk '{print $1}')


