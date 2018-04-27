# CI/CD

## GitLab CI/CD via GitLab-Runner

### Install the Runner

Install the GitLab Runner on a Mac: [https://docs.gitlab.com/runner/install/osx.html](https://docs.gitlab.com/runner/install/osx.html)

	sudo curl --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-darwin-amd64
	sudo chmod +x /usr/local/bin/gitlab-runner

### Set up Runner

Register the Runner and set it up:

	gitlab-runner register

Provide as URL: `https://gitlab.com` as described in
[https://docs.gitlab.com/runner/register/index.html](https://docs.gitlab.com/runner/register/index.html)

Look for the registration token in the GitLab settings under CI/CD.

If Docker is not used, then use `shell` as the executor.

### Run Runner

Install the Runner as a service and start it:

	gitlab-runner install
	gitlab-runner start

Continue with setting up fastline if necessary.

### GitLab pipeline

In GitLab go to the settings CI/CD section and extend the `Runners settings` section. Make sure the runner is enabled for this project and maybe disable shared, public runners.

Add a `.gitlab-ci.yml` file to the repo's root and define the jobs as described in [https://gitlab.com/help/ci/yaml/README.md](https://gitlab.com/help/ci/yaml/README.md).

### Uninstall runner

Stop and uninstall the service:

	gitlab-runner stop
	gitlab-runner uninstall

Remove the binary and any settings:

	sudo rm -r /usr/local/bin/gitlab-runner
	rm -r ~/.gitlab-runner

Delete any builds by the Runner (maybe also clean this periodically even when using the Runner to not fill the HDD with old builds):

	rm -r ~/builds

Remove the runner from all projects in GitLab. When there is no reference left the runner will be removed from available runners in GitLab.

## GitLab via AppCenter

The project is hosted at [GitHub](https://gitlab.com) with a git repo.

### Sync VSTS with GitLab

Create a project at VSTS (Visual Studio Team Services, [https://www.visualstudio.com/de/team-services](https://www.visualstudio.com/de/team-services)). Create a git by mirroring the one from GitHub (`Import a Git repository` in the `Code` tab).

Create a `Personal access token` via the `Security` entry in the profile. Change `Expires in` to `1 year`, choose the correct account and under `Authorized Scropes` select `Selected scropes` and check `Code (read and write)`. Click `Create Token` and copy the displayed token.

In GitHub go to the project's `Repository Settings` and expand the `Push to a remote repository`. Check `Remote mirror repository`, uncheck `Only protected branches` and provide the URL to the destination Git repository located on VSTS. Include the created personal access token from VSTS as the credentials in the URL, e.g. `https://MyAccessToken@indie-software.visualstudio.com/_git/MyRepo`.

Now when something is pushed to the GitHub repo it automatically also gets pushed to the VSTS repo.

### AppCenter

Log in to [AppCenter](https://appcenter.ms), and `Add new app`. Link it to VSTS and select the project and repo. Set up `Build`, `Test`, etc.
