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
  $$: docker build --build-arg AWS_ACCESS_KEY_ID=$AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY_ID=$AWS_SECRET_ACCESS_KEY_ID
  ...
  ```
</details>
## Summary

Small is beautiful - Faster builds and deploys

Small attack surface - Improved security

