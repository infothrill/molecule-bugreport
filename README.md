Role Name
=========

Example role to re-produce the behaviour described in https://github.com/metacloud/molecule/issues/1384

Re-producing
------------

The following leads to the error described (on macOS 10.13.6)

    tox -e ansible26

The following command work fine

    tox -e ansible24
    tox -e ansible25

License
-------

MIT

