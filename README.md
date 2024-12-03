# openstack-kolla-images

Pipeline for building custom [OpenStack Kolla](https://docs.openstack.org/kolla/latest/index.html) container images.

## Build

### Development

Prepare environment:
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

### GitHub Workflow

Go to [Actions](https://github.com/nimbolus/openstack-kolla-images/actions/workflows/build.yml) and click on `Run workflow`.

Images are pushed to [quay.io/nimbolus/openstack-kolla](https://quay.io/repository/nimbolus/openstack-kolla).

## Customization

### Add a Patch

1. Add patch file to `patches/<image-name>/<patch-name>.patch`.
2. Add patch file name to `patches/<image-name>/series` file.
3. Create additions archive in [kolla-build.conf](./kolla-build.conf):

```ini
[<image-name>-additions-patches]
type = local
location = patches/<image-name>/
```

4. Install `patch` binary and apply the patches in [template_overrides.j2](./template_overrides.j2):

```python
{% set <image-name>_packages_append = ['patch'] %}
{% block <image-name>_footer %}
ADD additions-archive /
RUN bash -c 'while read patch; do patch -d /var/lib/kolla/venv/lib/python3.9/site-packages -p 1 -i /additions/$patch; done < /additions/series'
{% endblock %}
```
