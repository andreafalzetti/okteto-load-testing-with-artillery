# Okteto Load Testing Sample using Artillery

This example shows how to use the [Okteto CLI](https://github.com/okteto/okteto) to develop a Go Sample App directly in Kubernetes and leveraging the `okteto test` command to run load testing using [Artillery.io](https://www.artillery.io/).

## How it works

The Okteto Manifest contains the following config:

```yaml
test:
  load:
    context: load-testing
    image: artilleryio/artillery:latest
    commands:
      - run run /okteto/src/artillery.yml --target "https://hello-world-${OKTETO_NAMESPACE}.${OKTETO_DOMAIN}" --output raw-artillery-report.json
      - run report raw-artillery-report.json --output visual-artillery-report.html
    artifacts:
      - raw-artillery-report.json
      - visual-artillery-report.html
```      

When you run `okteto test load --no-cache` this will launch a container in which you execute the Artillery CLI. Once finished, the Okteto CLI will retrieve the HTML and JSON report artifacts from the container and copy them on your machine.

Using Okteto CLI you offload your laptop from doing the heavy lifting of the load tests, and you can use Okteto Test GitHub Actions or directly Okteto CLI to run load tests from your CI/CD pipeline too.

