# OpenTelemetry Demo Fork

This repository is a fork of the existing OpenTelemetry demo application, specifically used for creating Docker containers for the checkout service.

## Docker Containers

Two Docker containers have been created in `tomondree/otel-demo-checkout` with the following tags:

- `correct`: Contains the checkout service with correct payment service configuration
- `incorrect`: Contains the checkout service with incorrect payment service configuration

## Usage in Simulation

These containers are used in our simulation scenario:

1. The `correct` tag represents the previous deployment state
2. The `incorrect` tag represents the current deployment state
3. After analysis is completed, the `incorrect` revision is rolled back to ensure proper functionality

## Container Images

The containers are available at:
- `tomondree/otel-demo-checkout:correct`
- `tomondree/otel-demo-checkout:incorrect`
