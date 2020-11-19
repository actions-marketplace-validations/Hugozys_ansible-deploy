# ansible-deploy docker action

![Action Build](https://github.com/Hugozys/ansible-container/workflows/.github/workflows/main.yaml/badge.svg?branch=master&event=push)
![Image Build](https://github.com/Hugozys/ansible-container/workflows/.github/workflows/image-push.yaml/badge.svg?branch=master&event=push)
![Docker Image Version (tag latest semver)](https://img.shields.io/docker/v/hugozzys/ansible-deploy/latest?label=Docker%20Image)

This action setups a docker container with ansible runtime installed. It is sufficient for my personal use to embed this action as part of the CD process of my application development pipeline.

## Inputs

### `directory`

**Optional** Path to the root directory of your ansible hierarchy. This acion assumes that everything used by ansible runtime will be put in this directory, including package, playbook, roles, group_vars, host_vars, etc. 

**Default**  `.`

### `playbook`

**Optional** Path to the plabook you want the action to run.

**Default**  `playbook.yaml`.

When you explicitly specify your playbook path, you MUST put the playbook under the root directory, and the path you specify MUST be a relative path to the root directory.

For instance, say all your ansible code lives in `./Ansible` directory in your repository, and your playbook lives in `./Ansible/playbook/playbook.yaml`. You only need to pass `playbook/playbook.yaml` into this input paramater.

### `envvars`

**Optional** extra key value pairs you want to pass to the ansible environment. They will be set as environment variables of the container which can also be accessed by ansible playbook in runtime.

### `extvars`

**Optional** extra key value pairs you want to pass to the `ansible-playbook` command. These kv pairs will be passed as the value of option `--extra-vars` when executing `ansible-playbook` command.

## Example usage

```yaml
uses: actions/ansible-deploy@v1
# run ./playbook.yaml
```

```yaml
uses: actions/ansible-deploy@v1
with:
    directory:"./ansible"
    playbook:"deploy.yaml" # actual file lives in ./ansible/deploy.yaml
```

```yaml
uses: actions/ansible-deploy@v1
with:
    directory:"./ansible"
    playbook:"deploy.yaml" # actual file lives in ./ansible/artifact/app.zip
    extvars:|
        package=artifact/app.zip
        username=hugozzys
    envvars:|
        API_TOKEN:fake-token
```

### NOTE

Currently the action doesn't support expclitly passing host file into `ansible-playbook`. You need to create an `inventory` directory underneath your ansible root directory and put your host file there. The playbook will read all the files defined in that directory. For instance, if your root directory is `./ansible`, create `./ansible/inventory` and your `host` file will be `./ansible/inventory/host`

### TODO

- [ ] Move away from bash
- [ ] Add support for explicit inventory
- [ ] Add more tests into workflow
