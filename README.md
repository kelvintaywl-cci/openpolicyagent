# openpolicyagent

Exploring builds using Open Policy Agent (OPA) on CircleCI

Specifically, we explore how to use [the `openpolicyagent/opa` Docker image](https://hub.docker.com/r/openpolicyagent/opa) on CircleCI.

## Notes

In order to use a Docker image as an execution environment, shell is required.
This is so that a Docker executor job can run the steps' shell commands.

We need to make sure either the ENTRYPOINT or SHELL instruction of the Docker image is set to initialize a shell session then.

You can see this requirement in CircleCI's next-gen `cimg/*` images.

For example:

```console
# inspect the cimg/base:current image, and look up the CMD, ENTRYPOINT and SHELL instructions

$ docker image inspect cimg/base:current | jq '.[0].Config | with_entries(select(.key | in({"Cmd":1, "Entrypoint":1, "Shell":1})))'
{
  "Cmd": [
    "bash"
  ],
  "Entrypoint": null,
  "Shell": [
    "/bin/bash",
    "-exo",
    "pipefail",
    "-c"
  ]
}
```

For Open Policy Agent's Docker images, we want to use the `-debug` variant.
This comes with shell installed.
> See https://hub.docker.com/r/openpolicyagent/opa

We can explore the image, like above, to understand its setup better.

```console
# inspect the inspect openpolicyagent/opa:0.45.0-debug image, and look up the CMD, ENTRYPOINT and SHELL instructions

$ docker image inspect openpolicyagent/opa:0.45.0-debug | jq '.[0].Config | with_entries(select(.key | in({"Cmd":1, "Entrypoint":1, "Shell":1})))'
{
  "Cmd": [
    "run"
  ],
  "Entrypoint": [
    "/opa"
  ]
}
```

## Conclusion

You can check out the .circleci/config.yml file in this repository to see how to set up a CircleCI job using the `openpolicyagent/opa:0.45.0-debug` image.
