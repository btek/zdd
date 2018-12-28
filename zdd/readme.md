zdd
Zero-downtime deploy tool for Amazon ELB and backend Amazon EC2 instances.

Usage
zdd -k 'AWS-ACCESS-KEY' -s 'AWS_ACCESS_SECRET' -n 'ELB-NAME' -c 'DEPLOY-COMMAND'

Install
git clone http://github.com/btek/zdd.git
cd zdd
git submodule update --init

Deploy process

zdd works with the following deploy process.

1. List the instances in the load balancer. 
2. Wait until statuses of all the services will be "In Service".
3. Deregister one instance from the load balancer. 
4. Run your deploy command.
5. Register the instance with the load balancer. 
6. Repeat them for all instances. 

Deploy command template 

You can use variables for deploy command.

instanceId
imageId
instanceState
privateDnsName
dnsName
keyName
instanceType
launchTime
availabilityZone
kernelId
subnetId
vpcId
privateIpAddress
ipAddress
tag.XXX(XXX is EC2 instance tag)

If the application name is specified with EC2 instance tag "ContextName" and you will deploy through ssh, you can use command template. 

zdd -k 'AWS-ACCESS-KEY' -s 'AWS-ACCESS-SECRET' -n 'ELB-NAME' -c 'ssh ${dnsName} deploy ${tag.ContextName}'

Options

Usage: zdd -n <elb-name> -c <command> -k <aws-key> -s <aws-secret>

-n --elb-name            instance name of Amazon ELB
-d --dependant-elb-names instance name of dependant Amazon ELBs (comma separated)
-c --command             shell command
-k --aws-key             AWS access key
-s --aws-secret          AWS access secret
   --region              region of Amazon EC2 ELB
   --health-check-interval interval of Amazon ELB health check
   --graceful-period       sleep time for register and   deregister instance for Amazon ELB
   --concurrency          the number of hosts to be deployed at the same time
   --help                 help

Dependant ELB means ELB having the same instance as the target ELB. The instance to be deployed will be also deregistered from dependant ELB. 