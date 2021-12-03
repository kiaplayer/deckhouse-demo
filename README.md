Start Vagrant VM:
```bash
$ vagrant up deckhouse
```

Start Deckhouse installer:
```bash
$ docker run --rm -it \
  -v "$PWD/config.yml:/config.yml" \
  -v "$PWD/.vagrant/machines/deckhouse/virtualbox/:/tmp/.ssh/" \
  registry.deckhouse.io/deckhouse/ce/install:stable \
  bash
``` 

Install Deckhouse (from docker container):
```bash
$ dhctl bootstrap \
  --ssh-user=vagrant \
  --ssh-host=192.168.56.10 \
  --ssh-agent-private-keys=/tmp/.ssh/private_key \
  --config=/config.yml
```

Install some manifests:
```bash
$ vagrant ssh deckhouse
$ sudo -i
$ kubectl create -f /vagrant/1_after_install/ingress-nginx-controller.yml
$ kubectl create -f /vagrant/1_after_install/user.yml
```

Add worker:
```bash
$ kubectl create -f /vagrant/2_add_worker/node-group-worker.yml
$ kubectl -n d8-cloud-instance-manager get secret manual-bootstrap-for-worker -o json | jq '.data."bootstrap.sh"' -r

$ vagrant up worker
$ vagrant ssh worker
$ sudo -i
$ echo <base64> | base64 -d | bash
```
