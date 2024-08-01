More Stuff
==========

Webhooks
--------

Trigger a Github Action from somewhere else (ex: a deploy)





Self-Hosted Runners
-------------------

Run actions on your own infrastructure.  
See: https://github.com/itenium-be/Github-Actions/actions/runners








Environments
------------

Configure dev, staging, prod, ...  
See: https://github.com/itenium-be/Github-Actions/settings/environments


```yaml
name: Deployment
on:
  push:
    branches:
      - master

jobs:
  deployment:
    runs-on: ubuntu-latest
    # Name of the environment
    environment: production
    # Only one job with this concurrency string will execute
    # (it doesn't have to match an environment!)
    concurrency: production
    steps:
      - run: echo "Deploying!"
```

Concurrency can also be configured at the top level:

```yaml
name: Deployment
concurrency:
  group: production
  cancel-in-progress: true

on: push
```








Job Outputs
-----------

Output stuff for downstream jobs.

Output something with:

```yaml
echo "test=hello" >> "$GITHUB_OUTPUT"
```

Example:

```yaml
# Like 06.inputs.yml:
name: Reusable workflow
on:
  workflow_call:
    outputs:
      firstword:
        description: "The first output string"
        value: ${{ jobs.example_job.outputs.output1 }}
      secondword:
        description: "The second output string"
        value: ${{ jobs.example_job.outputs.output2 }}

jobs:
  example_job:
    name: Generate output
    runs-on: ubuntu-latest
    outputs:
      output1: ${{ steps.step1.outputs.firstword }}
      output2: ${{ steps.step2.outputs.secondword }}
    steps:
      - id: step1
        run: echo "firstword=hello" >> $GITHUB_OUTPUT
      - id: step2
        run: echo "secondword=world" >> $GITHUB_OUTPUT
```









Dependent Jobs
--------------

Always run `job3` even if `job1` or `job2` failed:

```yaml
jobs:
  job1:
  job2:
    needs: job1
  job3:
    if: ${{ always() }}
    needs: [job1, job2]
```
