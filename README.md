# osbuild-composer pulp ostree test

Steps to run this test:

1. Set up a Pulp server to test against.  The simplest setup is the [Pulp in One Container](https://pulpproject.org/pulp-in-one-container/).
2. Edit the pulp.toml file to set the `server_address` and credentials.  The `repository` and `basepath` can be set to any value.
3. Edit the `testpulp` script to set the `pulp_server` and `repo_basepath` to match the toml file.
3. Run the `testpulp` script.

The script runs the following actions:
1. Push the two blueprints to osbuild-composer.
2. Start the build of the first commit.
3. Create a local ostree repo in a temporary directory.
4. Configure the remote for the ostree repo to the expected address based on the pulp configuration.
5. Wait for the build to finish.
6. Wait for pulp to finish importing the commit to the repository (using `curl .../refs/heads/...`).
7. Run `ostree remote summary test` to print the remote summary for verification.
8. Start the build of the second commit, pointing to the repository as a parent.
9. Wait for the build to finish.

At this point, the import of the child commit to the existing ostree repo should begin.  The test does not wait for this task to finish.
