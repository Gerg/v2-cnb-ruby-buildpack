# v2-cnb-ruby-buildpack

Experimental buildpack to run Cloud Native (v3) buildpacks on Cloud Foundry.

This doesn't work (yet... 🤞).

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
  cf stage my_app
```

## Design

To come...

Inspired by: https://github.com/tektoncd/catalog/blob/main/task/buildpacks-phases/0.2/buildpacks-phases.yaml
