# INSTALL

# yum proxy configuration
To use `yum` package manage in the calculation node, you need to set up the proxy environment. 

`[root@node08]# echo "proxy=http://headnode:3128" >> /etc/yum.conf`
