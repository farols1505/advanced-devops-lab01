# Vendored chart

Source: https://github.com/iankisali/microservice-deployment, directory
`chart/microservice`, commit `8ed96c8d98625c73b67c7172b6703383ea9e46e9`
(the same commit `vars/microservices.yml` pins for the values files).

The first commit that adds this directory is an unchanged copy. Every change
after it is ours and is visible with:

    git log -p -- charts/microservice

Changes (Lab01 extension):

| File | Change |
|---|---|
| `templates/deployment.yaml` | optional `resources`, `startupProbe`, `readinessProbe`, `livenessProbe` |
| `templates/pdb.yaml` | new: optional PodDisruptionBudget (`pdb.enabled`, `minAvailable` or `maxUnavailable`) |
| `values.yaml` | defaults for the above: empty / off, so upstream behaviour is unchanged |
| `Chart.yaml` | version `0.1.0` -> `0.2.0-lab01` |

The upstream values files (`values/*.yaml`) are not vendored: they are still
read from the pinned clone, unchanged.
