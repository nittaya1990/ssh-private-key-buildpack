# ssh-private-key-buildpack

A Buildpack for setting the ssh private key as part of the application build. It's meant to be used as part of a setup [using multiple buildpacks](https://doc.scalingo.com/platform/deployment/buildpacks/multi).

# Example usage

## Configure Multiple Buildpacks

The buildpack should be configured using a `.buildpacks` in combination with the [`multi-buildpack`](https://doc.scalingo.com/platform/deployment/buildpacks/multi)

    $ scalingo --app my-app env-set BUILDPACK_URL=https://github.com/Scalingo/multi-buildpack.git

The same example given for the CLI use would have the following `.buildpacks` file.

    $ cat .buildpacks
    https://github.com/Scalingo/ssh-private-key-buildpack.git
    https://github.com/Scalingo/nodejs-buildpack.git

## Configure SSH Key

Set the private key environment variable `SSH_KEY` of your app (note that the key needs to be base64 encoded).

    $ scalingo --app my-app env-set SSH_KEY="$(cat path/to/your/keys/id_rsa | base64)"

By default the buildpack adds GitHub to `known_hosts`. However you can configure your app to allow custom hosts, too. All that's needed is the set `SSH_HOSTS` for you app to a comma-separated list of hosts, e.g. `git@github.com,example.com`

    $ scalingo --app my-app env-set SSH_HOSTS="git@github.com,example.com"

## SSH Key Management Strategy

Because the buildpack cannot provide a password, the provided SSH key cannot be encrypted. Thus our advice to fetch private GitHub repository is to setup a GitHub Deploy Key, unencrypted and read-only only for the target repositories you want to fetch.
