# caprover-action
Action to deploy on Caprover server.

supports different type of deployment

## Inputs

---

| Name                    | Type   | Default  | Description                                                                                                                                                                                 | Example                                                                                    |
|-------------------------|--------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| `server`                | String |          | CapRover admin panel URL, without the captain in the url.                                                                                                                                   | `https://root.domain.com`                                                                  |
| `app-token`             | String |          | CapRover app token                                                                                                                                                                          |                                                                                            |
| `app-name`              | String |          | Application name on the CapRover server. Your app must exists.                                                                                                                              |                                                                                            |
| `registry-host`         | String |          | You docker registry url, this is used to store image temporarily before deploying                                                                                                           | `registry.gitlab.com/username/caprover-docker-images`, `registry.server.syntanext.com:996` |
| `registry-user`         | String |          | Docker registry user.                                                                                                                                                                       |                                                                                            |
| `registry-name`         | String | caprover | Name of the supported registry to deploy to                                                                                                                                                 | `caprover`, `AWS`                                                                          |
| `registry-token`        | String |          | Docker registry token.                                                                                                                                                                      |                                                                                            |
| `plain`                 | Bool   | false    | If you are deploying plain files                                                                                                                                                            | `true`, `false`                                                                            |
| `tarfile`               | Bool   |          | If you are deploying tar file                                                                                                                                                               | `true`, `false`                                                                            |
| `branch`                | String | main     | If you are deploying plain file then set the branch name to deploy                                                                                                                          | `master`, `main`, `dev`                                                                    |
| `content`               | String |          | Project folder path or tar file to use to create the image or upload                                                                                                                        | `.`, `/src`, `/output/file.tar`                                                            |
| `dockerfile`            | String |          | Docker file path of your project                                                                                                                                                            | `/path/to/Dockerfile`                                                                      |
| `image-name`            | String |          | Image name in your registry                                                                                                                                                                 |                                                                                            |
| `image-tag`             | String | latest   | New image tag                                                                                                                                                                               | |
| `deploy-app`            | Bool   | true     | Deploy newly pushed image to caprover                                                                                                                                                       | |
| `aws-account-id`        | String |          | Used if you are deploying image to AWS ECR                                                                                                                                                  | |
| `aws-access-key-id`     | String |          | Used if you are deploying image to AWS ECR                                                                                                                                                  | |
| `aws-secret-access-key` | String |          | Used if you are deploying image to AWS ECR                                                                                                                                                  | |
| `aws-region`            | String |          | Used if you are deploying image to AWS ERC                                                                                                                                                  | |
| `caprover-password`     | String |          | When you are deploying image AWS ECR, caprover need to access those images, so we programmatically update ECR token on caprover [`issue`](https://github.com/caprover/caprover/issues/1476) | |

If you are deploying to AWS ECR, only `AmazonEC2ContainerRegistryPowerUser` policy is needed
 
 ## EXAMPLES
 > deploying image to AWS ECR registry then deploy
 ```yml
 ...
 steps:
    - name: 📂 deploy image
      uses: hazeezet/caprover-action@v2
      with:
        registry-name: AWS
        aws-account-id: ${{ secrets.AWS_ACCOUNT_ID }}
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ secrets.AWS_REGION }}
        caprover-password: ${{ secrets.CAPROVER_PASSWORD }}
        server: ${{ secrets.SERVER }}
        app-token: ${{ secrets.DEV_APP_TOKEN }}
        app-name: ${{ secrets.DEV_APP_NAME }}
        image-tag: v1.0.0
        content: ./
        dockerfile: ./Dockerfile
        image-name: deploy-image
...
```

 > deploying image to a registry eg caprover then deploy
 ```yml
 ...
 steps:
    - name: 📂 deploy image
      uses: hazeezet/caprover-action@v2
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
    uses: hazeezet/caprover-action@v2
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
    uses: hazeezet/caprover-action@v2
    with:
      server: ${{ secrets.SERVER }}
      app-token: ${{ secrets.APP_TOKEN }}
      app-name: ${{ secrets.APP_NAME }}
      tarfile: true
      content: build.tar
...
```
