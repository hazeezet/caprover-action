# caprover-action
Action to deploy on Caprover server.

## Inputs

---

### server
> CapRover admin panel URL, without the captain in the url.

💠 https://root.domain.com

### app-token
> CapRover app token

### app-name
> Application name on the CapRover server. Your app must exists.

### registry-host
> You docker registry url

⏏ `registry.gitlab.com/username/caprover-docker-images`

this is used to store image temporarily before deploying

 ### registry-user
> Docker registry user.

### registry-token
> Docker registry token.

### plain
> If you are deploying plain files

### tarfile
> If you are deploying tar file
    
### branch
> If you are not deploying plain files then set the branch name to deploy

💠 **default** main

### content
> Project folder path or tar file to use to create the image or upload

### dockerfile
> Docker file path of your project

### image-name
> Image name in your registry

### image-tag
> New image tag

💠 **default** latest
        
 
 ## EXAMPLE
 > deploying image to registry then to caprover
 ```yml
 ...
 steps:
    - name: 📂 deploy image
      uses: hazeezet/caprover-action@v1
      with:
        server: ${{ secrets.SERVER }}
        app-token: ${{ secrets.APP_TOKEN }}
        app-name: ${{ secrets.APP_NAME }}
        registry-host: ${{ secrets.REGISTRY_HOST }}
        registry-user: ${{ secrets.REGISTRY_USER }}
        registry-token: ${{ secrets.REGISTRY_TOKEN }}
        content: ./
        dockerfile: ./Dockerfile
        image-name: deploy-image
        image-tag: v1.0.0
...
```

> deploying plain files to caprover
```yml
...
steps:
  - name: 📂 deploy
    uses: hazeezet/caprover-deploy-image@v1-canary-22
    with:
      server: ${{ secrets.SERVER }}
      app-token: ${{ secrets.APP_TOKEN }}
      app-name: ${{ secrets.APP_NAME }}
      plain: true
      branch: main
...
```

> deploying tar files to caprover
```yml
...
steps:
  - name: 📂 deploy
    uses: hazeezet/caprover-deploy-image@v1-canary-22
    with:
      server: ${{ secrets.SERVER }}
      app-token: ${{ secrets.APP_TOKEN }}
      app-name: ${{ secrets.APP_NAME }}
      tarfile: true
      content: build.tar
...
```
