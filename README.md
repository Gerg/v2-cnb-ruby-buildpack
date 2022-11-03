# v2-cnb-ruby-buildpack

Experimental buildpack to run Cloud Native (v3) buildpacks on Cloud Foundry.

This likely won't work for most applications,
but it does appear to work for [dora](https://github.com/cloudfoundry/cf-acceptance-tests/tree/96f95fc4c56fc3d7c34f17b42d2c1eba049966c2/assets/dora).

## Initial Setup

```sh
$ zip -r /tmp/ruby_cnb.zip ./bin
...
$ cf create-buildpack ruby_cnb /tmp/ruby_cnb.zip 1
...
$ cd ~/my_ruby_app
...
$ cf push my_app -b ruby_cnb
...
```

## Updating

```sh
$ zip -r /tmp/ruby_cnb.zip ./bin && \
  cf update-buildpack ruby_cnb -p /tmp/ruby_cnb.zip && \
  cf restage my_app
```

## Design

Summary:
1. Download the CNB [reference lifecycle](https://github.com/buildpacks/lifecycle) and a handful of ruby CNB buildpacks.
1. Use the `detect` script from the [v2 ruby buildpack](github.com/cloudfoundry/ruby-buildpack).
1. In `finalize`, call only the CNB lifecycle `detector` and `builder` with a bunch of needed environment variables and toml files. If we don't call `exporter`, then we keep directories (for a CF droplet), instead of an OCI image. In the future, much of this logic should be in `supply` instead, for multiple buildpack support.
1. Harvest the PATH and other environment variables from the generated layers.
1. In `release`, parse the start command from a toml file and return it to CF.

Inspired by:
- https://github.com/tektoncd/catalog/blob/main/task/buildpacks-phases/0.2/buildpacks-phases.yaml
- https://github.com/ryanmoran/cfnb
- https://github.com/buildpacks/spec

