## Jenkins 

1. Install java dependencies:

```shell
sudo apt install openjdk-11-jdk -y
```

2. Install jenkins;
```shell
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \
    /etc/apt/sources.list.d/jenkins.list'
sudo apt-get update
sudo apt-get install jenkins
```

3. Check the status of Installation:
```shell
sudo systemctl status jenkins
```
