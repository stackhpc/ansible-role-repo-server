Intranet Repo Server
====================

During OpenStack deployments, we often want to have a local repo server
on the site intranet, in order to be able to cache remote content,
and to manage the distribution of locally-generated repos and packages.

Requirements
------------

A containerised deployment of nginx is used to serve repo content.
The repo host supplied in the inventory must be capable of supporting
the Docker ecosystem.

Role Variables
--------------

`repo_server_name`: Name to assign to the nginx container.
It defaults to `repo_server`

`repo_server_port`: Port to listen on for HTTP.
Defaults to 80.

`repo_server_workspace`: Directory on the container host that
holds the nginx configuration and document root for the repo server.
Individual locations for these can be set using `repo_server_conf` and
`repo_server_docroot` respectively.

`repo_server_tarballs`: A list of filenames of gzipped tarballs to
unpack into the repo workspace.  It is implicitly assumed that each
tarball will unpack into a subdirectory with the same name as the
basename of the tarball filename.

Dependencies
------------

Example Playbook
----------------

The following playbook generates a guest image and uploads it to OpenStack:

    ---
    - hosts: seed

      roles:
        - role: repo_server
          become: true
          repo_server_name: "alaska_repo"
          repo_server_port: 4120
          repo_server_workspace: "/home/stack"
          repo_server_tarballs:
            - MLNX_OFED_LINUX-4.0-2.0.0.1-rhel7.3-x86_64.tgz
            - ALaSKA-Extras.tgz

Author Information
------------------

- Stig Telfer (<stig@stackhpc.com>)
