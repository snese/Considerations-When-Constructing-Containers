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

### Amazon ECR Image Scanning
- Identify software vulnerabilities
- Uses the Common Vulnerabilities and Exposures (CVEs) database
- No additional cost

Ref: [Image scanning](https://docs.aws.amazon.com/AmazonECR/latest/userguide/image-scanning.html)

<details>
  <summary>CLI example</summary>

  ```zsh
  # Repository to Scan on Push
  $: aws ecr create-repository --repository-name name \
    --image-scanning-configuration scanOnPush=true
  # Retrieving Scan Findings
  $: aws ecr describe-image-scan-findings --repository-name name \
    --image-id imageTag=tag_name
  ```
</details>

### Security Best Practices

<details>
  <summary>Tips</summary>
    - Small containers have a smaller attack surface
    - Include security certificates and create users if required
    - Update packages regularly
    - Follow common application security best practices
      - Authorise changes
      - Automate security in the CI/CD pipeline
      - Validate and scan code for vulnerabilities
</details>

## Summary

Small is beautiful - Faster builds and deploys

Small attack surface - Improved security

