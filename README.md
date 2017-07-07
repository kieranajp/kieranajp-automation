### Les Steps

1. Create a server in Digital Ocean dashboard. Ensure you feed it SSH keys.
2. Run `root.yml` playbook. This runs as `root` user and:
    - Creates users and groups
    - Sets up passwordless sudo
    - Adds authorized keys for users from [Github public keys](https://github.com/kieranajp.keys)
    - Generates SSH keys for users
    - Adds the generated SSH keys to Gitlab, via its API
    - Disables root SSH access
3. Run `kieranajp.yml` playbook. This should run as your newly created user and:
    - Installs defined `apt` packages
    - Installs node.js and NPM, and specified NPM global packages
    - Installs Caddy server and starts it as a service. The provided Caddyfile :
        - Serves from /srv/www/kieranajp.uk
        - Clones site source code from Gitlab (hence adding the SSH key)
        - Performs a `git pull` ever 10 minutes
        - Runs `brunch` and `hugo` to build the SASS and site after each pull
