# kubectl-connection-demo

## Initialize Project

- Clone this repo
- Initialize as a new git repo -- `rm -rf .git && git init && git add . && git commit -m 'first commit'`

## Store your k8s configs to a single file

```
kubectl config view --minify --flatten > kubeconfigdata
```

## Store the contents of the single kube config file to an environment variables file

```
docker run --rm -it -v $(pwd):/files codeship/env-var-helper cp kubeconfigdata:/root/.kube/config k8s-env
rm kubeconfigdata
```

## View contents stored by codeship/env-var-helper tool

```
docker run --rm -it -v $(pwd):/files codeship/env-var-helper ls k8s-env
```

## View individual file stored by codeship/env-var-helper tool

```
docker run --rm -it -v $(pwd):/files codeship/env-var-helper cat /root/.kube/config k8s-env
```

## Remove an individual file stored by codeship/env-var-helper tool

```
docker run --rm -it -v $(pwd):/files codeship/env-var-helper rm /root/.kube/config k8s-env
```

## Store file back to environment variable file

```
docker run --rm -it -v $(pwd):/files codeship/env-var-helper cp kubeconfigdata:/root/.kube/config k8s-env
```

## Encrypt the environment variables file

- Install our [jet cli tool](https://documentation.codeship.com/pro/jet-cli/installation/) on your local machine
- Setup your repository on your SCM of choice
- Grab the git url of the repository and create a Codeship Pro project
- From your Codeship 'Project Settings' > 'General' page, scroll down to AES key section and click 'Download Key'
- Move downloaded key to your project directory and rename to `codeship.aes`
- Add `codeship.aes` to your `.gitignore` file (!)
- Add [any additional environment variables](https://documentation.codeship.com/pro/builds-and-configuration/environment-variables/#encrypting-your-environment-variables) to the `k8s-env.env` file
- Run `jet encrypt k8s-env k8s-env.encrypted`
- The `k8s-env.encrypted` will be safe to check into your git repository
- Remove the plaintext file from your project --> `rm k8s-env`

## Run `jet steps`

- Run `jet steps`
- Steps should pass, demonstrating that kubectl configurations are now accessible to the kubectl container
