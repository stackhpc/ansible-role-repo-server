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

If it is not already present, a Yum repo called `docker-repo` is installed
for https://yum.dockerproject.org/

RPM packages for `tar`, `unzip`, `docker-engine` and `python-docker-py`
are installed, if not already present.

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

`repo_server_tarballs`: A list of dictionaries with the following keys:
* `path`: filename of gzipped tarball to unpack into the repo workspace. It is
implicitly assumed that each tarball will unpack into a subdirectory with the
same name as the basename of the tarball filename.
* `docroot_subpath`: The subpath under the document root that the tarball should be
extracted to.

Note: In a previous release, this variable used to contain a list of paths.
This format is deprecated and support will be removed in a future release.

Dependencies
------------

Example Playbook
----------------

The following playbook generates a guest image and uploads it to OpenStack:

    ---
    - hosts: seed

      roles:
        - role: stackhpc.repo-server
          become: true
          repo_server_name: "alaska_repo"
          repo_server_port: 4120
          repo_server_workspace: "/home/stack"
          repo_server_tarballs:
            - ALaSKA-Extras.tgz
            - path: MLNX_OFED_LINUX-4.0-2.0.0.1-rhel7.3-x86_64.tgz
              docroot_subpath: ofed

Author Information
------------------

- Stig Telfer (<stig@stackhpc.com>)
