# git-crucible-review-creator
### Create Code Reviews on Crucible Automatically During "git push".

This script is a git pre-push hook to create a code review entry on Crucible. A pre-push hook is called by `git push` command after it has checked the remote status, but before anything has been pushed.

This script should be placed on `.git/hooks` directory of the project. Once `git push <remote> <branch>` command is executed, the script will collect the difference from the pushed revision to the latest. Then it will call the Crucible REST API as configured on the script to create a new code review entry.

## Install

#### Automated Installation
It is highly recommended to use the siblings project [git-crucible-cli - The Interactive command-line tool for "git-crucible-review-creator"](https://github.com/kelumkps/git-crucible-cli) to install and manage the pre-push script. git-crucible-cli is a hassle free way to download the latest stable release from Github, apply configurations and install the "git-crucible-review-creator" on any git project.

Please refer [git-crucible](https://www.npmjs.com/package/git-crucible) available on [npm](https://www.npmjs.com/package/git-crucible) for detailed instructions.

#### Manual Installation
1. Download the `git-crucible-review-creator` from latest stable release on Github.
2. Update below configurations on the file `pre-push`.

    - crucible_url="#HOST_PORT#"
    - project_key="#PROJECT_KEY#"
    - username="#USERNAME#"
    - password_base64="#PASSWORD_IN_BASE64#"
    - reviewers="#COMMA_SEPERATED_USERNAMES#"
3. Copy the updated file in to `<your-git-project>/.git/hooks` directory.
4. Following table describes each configuration which should be modified.

configuration | Description | Example
--------------|-------------|---------
crucible_url | URL of the Crucible Server (without trailing forward slash). |crucible_url="https://my-crucible-server:8080"
project_key | The Crucible Project Key where Code review entry should be created | project_key="CR-FOO"
username | A valid username which as access to create code reviews on the above Crucible server. | username="myuser"
password_base64 | Base64 encoded password of above user | password_base64="MTIzNDU="
reviewers | One or more comma seperated usersnames to include as reviewers in created code review. | reviewers="scott,joe,krish"  

## How it works
1. Perform several commits to your local repository to a desired branch with proper/meaningful commit messages (JIRA ID, User Story ID, description… etc).
2. When you are ready to push the changes to your remote repository, just invoke `git push <remote> <branch_name>` as usual way.
3. The script will automatically determine the change set up to the last pushed revision for the git user and create a new code review entry on Crucible with given reviewers.
4. Code review URL will be printed to the console.
5. Latest commit message will be the name/title of the code review and all commit messages of the change set will be available under the description/objectives. (You can edit these things later after login to Crucible)

**Note:** The `git-crucible-review-creator` supports both _**Centralized Workflow**_ and _**Forking Workflow**_ (and other sub workflows based on these two) which are described [here](https://www.atlassian.com/git/tutorials/comparing-workflows).

## API Compatibility.
The script was implemented to consume the [Crucible REST API](https://docs.atlassian.com/fisheye-crucible/latest/wadl/crucible.html) documented [here](https://docs.atlassian.com/fisheye-crucible/latest/wadl/crucible.html).
Below APIs are used when creating code reviews and please make sure they are available in your Crucible server.

##### POST /rest-service/reviews-v1
##### POST /rest-service/reviews-v1/{id}/reviewers
##### POST /rest-service/reviews-v1/{id}/remind

License
=======
MIT License

Copyright (c) 2016 [Kelum Senanayake](https://github.com/kelumkps/)

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
