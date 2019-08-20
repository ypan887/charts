# JFrog Pipelines Chart Changelog
All changes to this chart to be documented in this file.

## [0.4.1] - August 20, 2019
* Fix missing docker cli for dockerBuild and dockerPush
* Update PostgreSQL chart version to 6.3.0
* Update Redis chart version to 9.1.0

## [0.4.0] - August 14, 2019
* Add Helm v3 support

## [0.3.9] - August 14, 2019
* Improve documentation

## [0.3.8] - August 05, 2019
* Fix node env vars

## [0.3.7] - July 31, 2019
* Support image pull secret for Pipelines Steps image registry

## [0.3.6] - July 30, 2019
* Fix pipelineSync permissions

## [0.3.5] - July 30, 2019
* Add pull images secret to hook jobs

## [0.3.4] - July 30, 2019
* Make Vault resources customizable

## [0.3.3] - July 29, 2019
* Update readme

## [0.3.2] - July 28, 2019
* Fix deployments resources

## [0.3.1] - July 28, 2019
* Update readme with better external secret creation example

## [0.3.0] - July 26, 2019
* Support external secret with stored passwords
* Add service account to all Pipelines components
* Set non-root user for most of Pipelines components

## [0.2.5] - July 25, 2019
* Make Pipelines image registry as global setting

## [0.2.4] - July 25, 2019
* Improve Artifactory liveness check
* Pull dependent docker image when node starts

## [0.2.3] - July 24, 2019
* Refactor API and WWW urls

## [0.2.2] - July 24, 2019
* values.yaml improvement

## [0.2.1] - July 24, 2019
* Add to Api deployment wait for Redis 

## [0.2.0] - July 24, 2019
* Fix nodes antiaffinity
* Fix external PostgreSQL
* Update PostgreSQL to chart v6.1.0
* Update Redis to chart v8.1.5 

## [0.1.14] - July 24, 2019
* Dynamic nodes creation with the statefulsets

## [0.1.13] - July 23, 2019
* Add post-upgrade default node pool nodes creation
* Replace RabbitMQ-HA with RabbitMQ

## [0.1.12] - July 16, 2019
* Deploy default k8s node pool
* Deploy 3 k8s agent nodes
* Fix Builds

## [0.1.11] - July 12, 2019
* Pipelines v0.9.1

## [0.1.10] - July 12, 2019
* Bug fixes

## [0.1.9] - July 10, 2019
* Restore post-upgrade RabbitMQ hook

## [0.1.8] - July 10, 2019
* Fix root bucket
* Add support for external Postgres

## [0.1.7] - July 9, 2019
* Fix replica count and affinity

## [0.1.6] - July 9, 2019
* Initial release
