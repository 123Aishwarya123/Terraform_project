Terraform code:

provider "aws" {
region = "us-east-1"
access_key = "ASIAQTUZNVVBZIRVFO5K"
secret_key = "+HQJsVFyY0tUQGEGi9HtcIZhYgYPuftNJf8RB7Cf"
token = "FwoGZXIvYXdzEM7//////////wEaDOLs4YNTwgAZc8ombCK/ARshnwhIII9eV7BTBWp4MRKe2ulOaF1QBDV+wvFSVcNS58+0HPJFNL5HxiRupBo7QvadkK5Yi4WJrAgQro/KFyJpx3OgcR+4EFtEZgltnRiOpAST2czEgvl1aTitCu4Q8q1601TWyqWe1VVsym5wzDkXGRGQBufw3hls51uKSBaKnplULB0JAk/zgZ+521pqFTjN7t6Xono+M6RFCKlAVYKfoOgN1M4C+PcxKjHvmgkansP9esgfDeEFkXboiqbTKKTF6pwGMi1QNIZ7/448blt743V2Ei3QDJ2KSxKqF93UDiHCtLTAEAvWx5tVTFL0g3Hfh/A="
}
# Security group settings
variable "ingress-rules" {
type = list(number)
default = [22, 8080]
}
resource "aws_security_group" "web_traffic" {
name = "Allow web traffic"
description = "SSH/Jenkins inbound, everything outbound"
dynamic "ingress" {
iterator = port
for_each = var.ingress-rules
content {
from_port = port.value
to_port = port.value
protocol = "TCP"
cidr_blocks = ["0.0.0.0/0"]
}
}
egress {
from_port = 0
to_port = 0
protocol = "-1"
cidr_blocks = ["0.0.0.0/0"]
}
}
# Type of resource to be executed
resource "aws_instance" "ec2" {
ami = "ami-04505e74c0741db8d"
instance_type = "t3.micro"
key_name = "my_work"
vpc_security_group_ids = [aws_security_group.web_traffic.id]
# Type of connection to be established
connection {
type = "ssh"
user = "ubuntu"
private_key = file("${path.module}/my_key_pair.pem")
host = self.public_ip
}
}

.........................................................................................................................................................................
Comments to execute this project:

Server ubuntu Command(Java and Python) 
java --version 
sudo apt update 
sudo apt install java

sudo apt install software-properties-common 
sudo add-apt-repository ppa:deadsnakes/ppa 
sudo apt update 
sudo apt install python3.8 
python --version
----------------------------------------

(Jenkins) 
sudo apt-get update 
sudo apt-get install openjdk-8-jdk

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

sudo apt-get update

sudo apt install jenkins
