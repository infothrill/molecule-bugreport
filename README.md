# Molecule docker error with ansible 2.6+

Example role to investigate and re-produce the behaviour described in https://github.com/metacloud/molecule/issues/1384

# Molecule and Ansible details

```
molecule --version
molecule, version 2.16.0
```

```
ansible --version
ansible 2.6.1
  config file = None
  configured module search path = [u'/Users/infothrill/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
  ansible python module location = /Users/infothrill/tmp/molecule-bugreport/.tox/ansible26/lib/python2.7/site-packages/ansible
  executable location = /Users/infothrill/tmp/molecule-bugreport/.tox/ansible26/bin/ansible
  python version = 2.7.15 (default, Jun 17 2018, 12:46:58) [GCC 4.2.1 Compatible Apple LLVM 9.1.0 (clang-902.0.39.2)]
```

docker, docker-machine

```
Docker version 18.05.0-ce, build f150324
docker-machine version 0.15.0, build b48dc28
```

tox version

```
tox --version
3.1.2 imported from /Users/infothrill/.local/venvs/tox/lib/python3.6/site-packages/tox/__init__.py
```

tox pip information:

```
ansible26 create: /Users/infothrill/tmp/molecule-bugreport/.tox/ansible26
ansible26 installdeps: -rrequirements.txt, ansible>=2.6,<2.7, docker==3.4.1
ansible26 installed: ansible==2.6.1,ansible-lint==3.4.23,anyconfig==0.9.4,arrow==0.12.1,asn1crypto==0.24.0,atomicwrites==1.1.5,attrs==18.1.0,backports.functools-lru-cache==1.5,backports.ssl-match-hostname==3.5.0.1,bcrypt==3.1.4,binaryornot==0.4.4,Cerberus==1.2,certifi==2018.4.16,cffi==1.11.5,chardet==3.0.4,click==6.7,click-completion==0.3.1,colorama==0.3.9,configparser==3.5.0,cookiecutter==1.6.0,cryptography==2.2.2,docker==3.4.1,docker-pycreds==0.3.0,enum34==1.1.6,fasteners==0.14.1,flake8==3.5.0,funcsigs==1.0.2,future==0.16.0,git-url-parse==1.1.0,idna==2.7,ipaddress==1.0.22,Jinja2==2.10,jinja2-time==0.2.0,MarkupSafe==1.0,mccabe==0.6.1,molecule==2.16.0,monotonic==1.5,more-itertools==4.2.0,paramiko==2.4.1,pathspec==0.5.6,pbr==4.0.4,pexpect==4.6.0,pluggy==0.6.0,poyo==0.4.1,psutil==5.4.6,ptyprocess==0.6.0,py==1.5.4,pyasn1==0.4.3,pycodestyle==2.3.1,pycparser==2.18,pyflakes==1.6.0,PyNaCl==1.2.1,pytest==3.6.3,python-dateutil==2.7.3,python-gilt==1.2.1,PyYAML==3.13,requests==2.19.1,sh==1.12.14,six==1.11.0,tabulate==0.8.2,testinfra==1.14.0,tree-format==0.1.2,urllib3==1.23,websocket-client==0.48.0,whichcraft==0.4.1,yamllint==1.11.1
```

OS and python:

```
System Version: macOS 10.13.6 (17G65)
Python 2.7.15 (default, Jun 17 2018, 12:46:58)
[GCC 4.2.1 Compatible Apple LLVM 9.1.0 (clang-902.0.39.2)] on darwin
```

## Desired behaviour

The following commands should complete successfully:

```
mkdir ~/tmp
cd ~/tmp
git clone https://github.com/infothrill/molecule-bugreport.git
cd molecule-bugreport
tox
```

### Expected result

All tox tests pass:

```
  py27-ansible24: commands succeeded
  py27-ansible25: commands succeeded
  py27-ansible26: commands succeeded
```

## Actual Behaviour

`tox` tests for `ansible24` and `ansible25` succeed, while `ansible26` fails:

```
  py27-ansible24: commands succeeded
  py27-ansible25: commands succeeded
ERROR:   py27-ansible26: commands failed
```

The full debug output of the failed tasks:

```
    TASK [Destroy molecule instance(s)] ********************************************
    task path: /Users/infothrill/tmp/molecule-bugreport/.tox/ansible26/lib/python3.6/site-packages/molecule/provisioner/ansible/playbooks/docker/destroy.yml:8
    <127.0.0.1> ESTABLISH LOCAL CONNECTION FOR USER: infothrill
    <127.0.0.1> EXEC /bin/sh -c 'echo ~infothrill && sleep 0'
    <127.0.0.1> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /Users/infothrill/.ansible/tmp/ansible-tmp-1531643366.879078-120643033411569 `" && echo ansible-tmp-1531643366.879078-120643033411569="` echo /Users/infothrill/.ansible/tmp/ansible-tmp-1531643366.879078-120643033411569 `" ) && sleep 0'
    Using module file /Users/infothrill/tmp/molecule-bugreport/.tox/ansible26/lib/python3.6/site-packages/ansible/modules/cloud/docker/docker_container.py
    <127.0.0.1> PUT /Users/infothrill/.ansible/tmp/ansible-local-41217emtpv0jl/tmpwio4mvkf TO /Users/infothrill/.ansible/tmp/ansible-tmp-1531643366.879078-120643033411569/docker_container.py
    <127.0.0.1> PUT /Users/infothrill/.ansible/tmp/ansible-local-41217emtpv0jl/tmpd37qri64 TO /Users/infothrill/.ansible/tmp/ansible-tmp-1531643366.879078-120643033411569/async_wrapper.py
    <127.0.0.1> EXEC /bin/sh -c 'chmod u+x /Users/infothrill/.ansible/tmp/ansible-tmp-1531643366.879078-120643033411569/ /Users/infothrill/.ansible/tmp/ansible-tmp-1531643366.879078-120643033411569/docker_container.py /Users/infothrill/.ansible/tmp/ansible-tmp-1531643366.879078-120643033411569/async_wrapper.py && sleep 0'
    <127.0.0.1> EXEC /bin/sh -c '/Users/infothrill/tmp/molecule-bugreport/.tox/ansible26/bin/python3 /Users/infothrill/.ansible/tmp/ansible-tmp-1531643366.879078-120643033411569/async_wrapper.py 328598709749 7200 /Users/infothrill/.ansible/tmp/ansible-tmp-1531643366.879078-120643033411569/docker_container.py _ && sleep 0'
    changed: [localhost] => (item={'name': 'instance', 'image': 'centos:7'}) => {
        "ansible_job_id": "328598709749.41235",
        "changed": true,
        "finished": 0,
        "item": {
            "image": "centos:7",
            "name": "instance"
        },
        "results_file": "/Users/infothrill/.ansible_async/328598709749.41235",
        "started": 1
    }
    changed: [localhost] => {
        "censored": "the output has been hidden due to the fact that 'no_log: true' was specified for this result",
        "changed": true
    }

    TASK [Wait for instance(s) deletion to complete] *******************************
    task path: /Users/infothrill/tmp/molecule-bugreport/.tox/ansible26/lib/python3.6/site-packages/molecule/provisioner/ansible/playbooks/docker/destroy.yml:19
    <127.0.0.1> ESTABLISH LOCAL CONNECTION FOR USER: infothrill
    <127.0.0.1> EXEC /bin/sh -c 'echo ~infothrill && sleep 0'
    <127.0.0.1> EXEC /bin/sh -c '( umask 77 && mkdir -p "` echo /Users/infothrill/.ansible/tmp/ansible-tmp-1531643368.2400792-212868659754247 `" && echo ansible-tmp-1531643368.2400792-212868659754247="` echo /Users/infothrill/.ansible/tmp/ansible-tmp-1531643368.2400792-212868659754247 `" ) && sleep 0'
    Using module file /Users/infothrill/tmp/molecule-bugreport/.tox/ansible26/lib/python3.6/site-packages/ansible/modules/utilities/logic/async_status.py
    <127.0.0.1> PUT /Users/infothrill/.ansible/tmp/ansible-local-41217emtpv0jl/tmpngdxn0a6 TO /Users/infothrill/.ansible/tmp/ansible-tmp-1531643368.2400792-212868659754247/async_status.py
    <127.0.0.1> EXEC /bin/sh -c 'chmod u+x /Users/infothrill/.ansible/tmp/ansible-tmp-1531643368.2400792-212868659754247/ /Users/infothrill/.ansible/tmp/ansible-tmp-1531643368.2400792-212868659754247/async_status.py && sleep 0'
    <127.0.0.1> EXEC /bin/sh -c '/Users/infothrill/tmp/molecule-bugreport/.tox/ansible26/bin/python3 /Users/infothrill/.ansible/tmp/ansible-tmp-1531643368.2400792-212868659754247/async_status.py && sleep 0'
    <127.0.0.1> EXEC /bin/sh -c 'rm -f -r /Users/infothrill/.ansible/tmp/ansible-tmp-1531643368.2400792-212868659754247/ > /dev/null 2>&1 && sleep 0'
    The full traceback is:
      File "/var/folders/3n/jz7c9xjs6xg4psnmn9ds28k80000gp/T/ansible_6q24d2ap/ansible_modlib.zip/ansible/module_utils/docker_common.py", line 184, in __init__
        super(AnsibleDockerClient, self).__init__(**self._connect_params)
      File "/Users/infothrill/tmp/molecule-bugreport/.tox/ansible26/lib/python3.6/site-packages/docker/api/client.py", line 158, in __init__
        self._version = self._retrieve_server_version()
      File "/Users/infothrill/tmp/molecule-bugreport/.tox/ansible26/lib/python3.6/site-packages/docker/api/client.py", line 183, in _retrieve_server_version
        'Error while fetching server API version: {0}'.format(e)

    failed: [localhost] (item={'started': 1, 'finished': 0, 'ansible_job_id': '328598709749.41235', 'results_file': '/Users/infothrill/.ansible_async/328598709749.41235', '_ansible_parsed': True, 'changed': True, '_ansible_no_log': False, 'failed': False, 'item': {'name': 'instance', 'image': 'centos:7'}, '_ansible_item_result': True, '_ansible_ignore_errors': None, '_ansible_item_label': {'name': 'instance', 'image': 'centos:7'}}) => {
        "ansible_job_id": "328598709749.41235",
        "attempts": 1,
        "changed": false,
        "finished": 1,
        "invocation": {
            "module_args": {
                "api_version": "auto",
                "auto_remove": false,
                "blkio_weight": null,
                "cacert_path": null,
                "capabilities": null,
                "cert_path": null,
                "cleanup": false,
                "command": null,
                "cpu_period": null,
                "cpu_quota": null,
                "cpu_shares": null,
                "cpuset_cpus": null,
                "cpuset_mems": null,
                "debug": false,
                "detach": true,
                "devices": null,
                "dns_opts": null,
                "dns_search_domains": null,
                "dns_servers": null,
                "docker_host": "tcp://192.168.99.100:2376",
                "domainname": null,
                "entrypoint": null,
                "env": null,
                "env_file": null,
                "etc_hosts": null,
                "exposed_ports": null,
                "force_kill": true,
                "groups": null,
                "hostname": null,
                "ignore_image": false,
                "image": null,
                "init": false,
                "interactive": false,
                "ipc_mode": null,
                "keep_volumes": true,
                "kernel_memory": null,
                "key_path": null,
                "kill_signal": null,
                "labels": null,
                "links": null,
                "log_driver": null,
                "log_options": null,
                "mac_address": null,
                "memory": "0",
                "memory_reservation": null,
                "memory_swap": null,
                "memory_swappiness": null,
                "name": "instance",
                "network_mode": null,
                "networks": null,
                "oom_killer": null,
                "oom_score_adj": null,
                "paused": false,
                "pid_mode": null,
                "privileged": false,
                "published_ports": null,
                "pull": false,
                "purge_networks": false,
                "read_only": false,
                "recreate": false,
                "restart": false,
                "restart_policy": null,
                "restart_retries": null,
                "security_opts": null,
                "shm_size": null,
                "ssl_version": "1.0",
                "state": "absent",
                "stop_signal": null,
                "stop_timeout": null,
                "sysctls": null,
                "timeout": 60,
                "tls": false,
                "tls_hostname": "localhost",
                "tls_verify": false,
                "tmpfs": null,
                "trust_image_content": false,
                "tty": false,
                "ulimits": null,
                "user": null,
                "userns_mode": null,
                "uts": null,
                "volume_driver": null,
                "volumes": null,
                "volumes_from": null,
                "working_dir": null
            }
        },
        "item": {
            "ansible_job_id": "328598709749.41235",
            "changed": true,
            "failed": false,
            "finished": 0,
            "item": {
                "image": "centos:7",
                "name": "instance"
            },
            "results_file": "/Users/infothrill/.ansible_async/328598709749.41235",
            "started": 1
        },
        "msg": "Error connecting: Error while fetching server API version: ('Connection aborted.', BadStatusLine('\\x15\\x03\\x01\\x00\\x02\\x02\\n',))"
    }
    fatal: [localhost]: FAILED! => {
        "censored": "the output has been hidden due to the fact that 'no_log: true' was specified for this result",
        "changed": false
    }

    PLAY RECAP *********************************************************************
    localhost                  : ok=1    changed=1    unreachable=0    failed=1


ERROR:
ERROR: InvocationError for command '/Users/infothrill/tmp/molecule-bugreport/.tox/ansible26/bin/molecule --debug test' (exited with code 2)
___________________________________________________________________________________________________ summary ____________________________________________________________________________________________________
ERROR:   ansible26: commands failed
```

## Attempts to reduce the scope of the error so far

All the following led to no improvement.

* upgraded tox to latest
* ansible 2.6.0 exhibits the same issue
* downgraded `docker` in several steps down to 3.1.1 with no success
* re-provisioned the docker-machine using `docker-machine create -d virtualbox --virtualbox-memory "4096" --virtualbox-cpu-count "3" --virtualbox-disk-size "30000" default`
* Used python interpreter version 3.6.5
* rebooted OS
