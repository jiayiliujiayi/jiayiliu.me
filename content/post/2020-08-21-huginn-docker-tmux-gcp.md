---
title: Deploying Huginn with Docker on Google Cloud Platform
author: JIAYI  
date: '2020-08-21'
categories:
  - programming
tags:
  - work
  - programming
---

This is a step by step tutorial for new vps/huginn hobbyists who are not quite familiar with [Docker](https://www.docker.com/), [Huggin](https://github.com/huginn/huginn), [Tmux](https://github.com/tmux/tmux/wiki) or [Google Cloud Platform](https://console.cloud.google.com/).  



Prerequisites:

You have a google account

Steps:

1. Open Terminal on your own computer, copy, paste and press enter:  

   ```bash
   ssh-keygen -t rsa -f ~/.ssh/gcloud -C "yourname"
   cat .ssh/gcloud.pub
   ```

2. Copy the output (your public key).  It looks like such:  

   ```bash
   ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDqHVubWmyWj5R3I8Wa7kZdTcT9DclFfO/bXnIMps28281IS9iJjKWRVpWCZby6fc/QxZZguRrFvWL7bBm5r3lzW37bvBhy/tcjYd4Kq66gPklK9mygeelubvR1m2TRcN6ty5oi8w28Ik0I0iN04YVa0rx0qW4LrG4SCXyIdHuw2HWBruszY8eNFPFY1YFO81Q1Yfsl5b2fzPlNwc2zaGOYy2naKPXdXPb/tTGNvJrO3JuP2M8b+e7vmTyZVecHeNjWrce0xd2W9qiFLoj6aklcVyFqoDfHT1hEZ7t5/prbgD0k3cFsjexBYzI/bwfGKstkNSKty4/58NeF43pPBHUiC1Mpby4u4ChRFGRDzwsh84YQXgr5AZTjTok9QAnYtiUPc1yVMLLnAdQv0mtpAvXxDW7SuZ/8jpP6ivxGlKVCeMkB9hqTy/Qoajv4OskswML7qL+kJUQz2z+aD6CIzf+aQpm+gI1hWnwIhMdghE7IyDgrJe/D7gcUBBSb2af2icM= username
   ```

3. Go to your [Google Cloud Platform Console](https://console.cloud.google.com/)  

4. Click on the hamburger on the top left corner --> 'COMPUTE' --> 'Compute Engine' --> 'VM instances'  

5. Click 'Create instance'.  Tick the boxes: "Allow HTTP traffic" and "Allow HTTPS traffic" then click save.  

6. Go back to "VM instances" --> copy the 'External IP' of the instance (address as such: 192.168.1.1).    

7. Go to the instance that you've just created and edit the 'ssh keys' by copying your public key (generated in step2) in the box.  

8. Click on the hamburger on the top left -->  'NETWORKING' --> 'VPC Networks' --> 'Firewall' (left side bar)  

9. 'CREAT FIREWALL RULE', edit the followings:   

   1. Change the **Targets** dropdown to **All instances in the network**.
   2. Specify the **Source IP ranges** to be `0.0.0.0/0`.
   3. Protocols and ports: Specified protocols and ports
   4. tcp: 3000

10. Open Terminal on your own computer, connect to your instance via ssh:  

    ```bash
    ssh -i .ssh/gcloud username@192.168.1.1
    ```

11. Update the system, install docker and tmux, and run the huginn image:  

    ```bash
    # update the system
    sudo apt-get update

    # install docker, change docker socket mode
    sudo curl -sSL https://get.docker.com/ | sh
    sudo chmod 666 /var/run/docker.sock

    # install tmux
    sudo apt install tmux

    # run huginn with docker in a new tmux session
    tmux new -s huginn
    docker run -it -p 3000:3000 huginn/huginn
    ```

12. Open your web browser and visit: http://192.168.1.1:3000  

13. Default username: admin  Default password: password  
