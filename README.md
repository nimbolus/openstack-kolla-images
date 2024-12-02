# openstack-kolla-images

## Build

Preparation:
```sh
# create venv
python3 -m venv venv
# activate venv
source venv/bin/activate
# install requirements
pip install -r requirements.txt
# install kolla
pip install git+https://opendev.org/openstack/kolla@stable/2024.1
```

Build images:
```sh
kolla-build --config-file kolla-build.conf --profile custom
```
