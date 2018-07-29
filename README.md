# decentral.solutions


## Pre-reqs

### Nodejs
Recommend version v10.3.0    

  
### Hexo
We use Hexo for our content management system and markdown template renderer. When you run it, it starts a node server that static serves.   
```
sudo npm install hexo-cli -g
```

## Running Locally

### Clone the repository with submodules
```
git clone https://github.com/lingqingmeng/decentral.solutions.git --recursive
```

### Install dependencies
Use yarn (npm also ok)   
```
yarn
```

### Start the local server
Run the hexo process   
```
hexo server
```
navigate to `localhost:4000` on your webbrowser


## Deploying to Production

### SSH into our EC2
Get the SSH key and public IP from [Ling Qing](https://github.com/lingqingmeng)
```
ssh -i ~/.ssh/<key_pair_file> ubuntu@<ip_address>
```

### Check if currently running
Previously, someone may have kept a daemon of our Hexo server up. Check that if it exists, kill the daemon.
```
sudo kill $(sudo lsof -t -i:4000)
```

### Test 
Navigate to server directory  
```
cd ~/Github/blog
```
make sure you have the latest and correct branch
```
git branch
git pull origin master
```
Test the server (`hexo s` is an alias for `hexo server`) is running on https://decentral.solutions
```
hexo s
```
If it all looks good daemonize to keep the server process up indefinitely
```
hexo s &
```


