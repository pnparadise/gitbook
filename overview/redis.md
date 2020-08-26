批量删除redis 集群
```
#!/bin/bash
 
COMMOND_HOME=/etc/bin/redis-cli
pattern=$1
masterNodeAdress=192.168.0.168
masterNodePort=7000
password=passwD9923
echo "batch del keys ${pattern}" 
#batch del redis cluster
#1. fetch master nodes hostandport

echo "1. fetch master nodes hostandport"
HostAndPort=`${COMMOND_HOME} -c -h ${masterNodeAdress} -c -p ${masterNodePort} -a ${password} cluster nodes|grep master|awk '{print $2}'|awk -F "@" '{print $1}'`
 
#2. iter HostAndPort array to del keys by pattern
echo "2. iter HostAndPort array to del keys by pattern"
for node in ${HostAndPort[@]}
  do
    echo "start batch del ${node} keys ${pattern}"
    ip=`echo ${node}|cut -d : -f 1`
    port=`echo ${node}|cut -d : -f 2`
    items=`${COMMOND_HOME} -h ${ip} -p ${port} -a ${password} scan 0 match ${pattern} count 6000 |awk 'NR>1 {print $1}'`
    echo "${items[@]}"
    for key in ${items}
    	do
    		formateKey=`echo ${key}|awk '{print $1}'`
    		echo "${COMMOND_HOME} -c -h ${ip} -p ${port} -a ${password} del ${formateKey[@]} hello"
    		${COMMOND_HOME} -c -h ${ip} -p ${port} -a ${password} del ${formateKey[@]}
    		sleep 0.5   
    	done
  done
echo "completed"
```