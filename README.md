# Considerations When Constructing Containers
Optimizing for speed, size, and security

## Security

### Avoid secrets in Dockerfiles
<details>
  <summary>Bad example</summary>

  #### Dockerfile
  ```Dockerfile
  FROM baseimage
  RUN ...
  ENV AWS_ACCESS_KEY_ID=...
  ENV SECRET_AWS_ACCESS_KEY_ID=...
  RUN ...
  ```
    
  #### Terminal
  ```zsh
  $: docker build --build-arg \
    AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID \
    AWS_SECRET_ACCESS_KEY_ID=$AWS_SECRET_ACCESS_KEY_ID
  ...
  ```
</details>

<details>
  <summary>Good example</summary>

  #### Dockerfile
  ```Dockerfile
  FROM alpine:3.10
  RUN ...
  RUN pip install awscli
  RUN --mount=type=secret,id=aws,target=/root/.aws/credentials \
    && aws s3 sync s3://... 
  ...
  ```
    
  #### Terminal
  ```zsh
  $: docker build --secret id=aws,src=$HOME/.aws/credentials
  ...
  ```
</details>

## Summary

Small is beautiful - Faster builds and deploys

Small attack surface - Improved security

