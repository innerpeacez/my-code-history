yum install -y epel-release

yum install -y python-pip (如果无法安装，可能epel被禁用了)

pip install --upgrade pip

pip install shadowsocks

systemctl stop firewalld && systemctl disable firewalld

ssserver -p 1080 -k xxx -m rc4-md5 -d start （修改自己的密码， -k xxx）
