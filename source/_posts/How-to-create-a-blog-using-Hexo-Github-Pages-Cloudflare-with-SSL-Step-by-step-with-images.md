---
title: >-
  How to create a blog using Hexo, Github Pages, Cloudflare with SSL - Step by
  step with images
date: 2019-01-05 19:10:34
tags:
- xexo
- blog
- github 
---
## **We need:**
- A godaddy.com account with a domain
- A cloudflare.com account
- A github.com account
- Git Bash (https://git-scm.com/downloads)
- Node JS (https://nodejs.org)

## **Process**
### 1. Enter to cloudflare.com and add a site
![](/images/20190105/1-1.jpg)
![](/images/20190105/1-2.PNG)
![](/images/20190105/1-3.PNG)
![](/images/20190105/1-4.PNG)
![](/images/20190105/1-5.PNG)
![](/images/20190105/1-6.jpg)
![](/images/20190105/1-7.jpg)

### 2. Enter to godaddy.com and config nameservers
#### Setup Nameservers
Click on "DNS"
![](/images/20190105/2-1.jpg)
In the section "Nameservers" click on "Change"
![](/images/20190105/2-2.jpg)

Select custom and add cloudflare's nameservers

Delete any nameserver if exist.

Cloudflare nameservers
- joan.ns.cloudflare.com
- nash.ns.cloudflare.com
![](/images/20190105/2-3.PNG)

#### Validate Nameservers
According to Cloudflare and Godaddy the process to change nameservers can take up to 24 hours,  but normally take a few minutes, you can validate in two ways:
    
#### From CLI
Open the terminal and paste next command
```sh
$ nslookup -type=ns randomtechguy.com
```
![](/images/20190105/2-4.PNG)

#### From web
Enter to https://dnschecker.org/
![](/images/20190105/2-5.PNG)

### 3. Create a repo in Github
Enter to github.com and create repo
![](/images/20190105/3-1.PNG)
Add a name to your new repo and click on "Create repository"
![](/images/20190105/3-2.PNG)
![](/images/20190105/3-3.PNG)

### 4. Verifies security certs availability(SSL) 
At this time your domain is management by cloudflare so we have an SSL certificate.
Enter to cloudflare.com
Click on your site created at step 1
Click on Crypto and verify certificate status, it should be active like under image:
![](/images/20190105/4-1.PNG)
Go to section "Always Use HTTPS" and check "On"
![](/images/20190105/4-2.PNG)


## From here we will use the terminal :D

### 5. Install hexo (https://hexo.io/) 
Open the terminal
```sh
# Install hexo globally
$ npm install hexo-cli -g
# Start a hexo project
$ hexo init myawesomeblog
# Enter to project folder and install dependencies
$ cd myawesomeblog
$ npm install
# Test our new blog
hexo server
# This command should return
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```
Open a browser with url http://localhost:4000.
Don't close the terminal, we will use later


The next steps are based on a awesome post by Jeff Ferrari
https://www.poweredbyjeff.com/2018/05/14/Deploying-Hexo-website-to-Github-Pages/

### 6. Setup hexo for upload to Github
#### Edit _config.yml
Edit URL
	url: https://randomtechguy.com #your domain with https

Edit public dir from public to docs
	public_dir: docs

Comment deploy config
    #deploy:
    #   type:

### 7. Generate files and upload to Github
Open the terminal
#### Generate files
```sh
# Remove plugins
$ npm uninstall hexo-generator-cname hexo-deployer-git
# Create CNAME file in source folder, use nano, vim or your prefer editor
$ nano source/CNAME #no extension, all uppercase
# The contents of this file should be the domain of your website
$ echo myawesomeblog.com > source/CNAME
# Generate files
hexo generate
# This commando should generate a folder named docs
```
#### Add remote origin and make first commit
You can see original post in Github Help
https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/
```sh
$ git init
$ git add .
$ git commit -m "First commit"
# Add remote origin URL, use URL from your repo created at the step 3
$ git remote add origin remote https://github.com/diegofcornejo/myawesomeblog.git 
# Verifies the new remote URL
git remote -v 
# Push changes
$ git push origin master
```

### 8. Setup Github Pages
Open your repo on the web
Now should see files in your repo
![](/images/20190105/8-1.PNG)
Click on Tab "Settings"
![](/images/20190105/8-2.PNG)
On section "Github Pages" Select master branch / docs folder and Save
![](/images/20190105/8-3.PNG)

### 9. Finally open a browser and write your domain, will see your blog running on your domain.